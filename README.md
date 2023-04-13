This repo currently holds files for translating Orange to Slovenian, but translations for other languages may be added.

If the project is successful, translations files will be moved to their corresponding repositories.

Here are instructions for trying it out.
Examples assume you have a functional environment for orange, `o3` and that you're translating to Spanish (`es`).

Furthermore, these instructions assume you are computer scientist, you can use shell and find your away around minor problems.

Translations uses Trubar. At some point (in particular if your language uses different plural forms)
you may need to understand how it works. See documentation at https://janezd.github.io/trubar.

## Preparation

We first need a Python environment with Orange and Trubar.
We create directories `orig` with original source and `trans` with translated sources. The latter are
installed via `pip install -e .`, so we can try them out.

```sh
# Create a new environment for Orange, e.g. o3-es, with all
# requirements installed. Use either conda or pip.
# You can clone an existing environment,
# e.g. o3 (change to the actual name on your machine)
conda create -n o3-es --clone o3

# Activate it
conda activate o3-es

# Install trubar
pip install trubar

# Clone this repo
git clone https://github.com/biolab/orange-translations.git

# Checkout parts of core Orange, make copies and install them
# into environment
mkdir orig trans
for repo in orange3 orange-widget-base orange-canvas-core
do
    git clone https://github.com/biolab/$repo.git orig/$repo
	mkdir trans/$repo
    cp -r orig/$repo/* trans/$repo
	cd trans/$repo
	pip install -e .
	cd -
done
```

At this point you should be able to run Orange by `python -m Orange.canvas`. The first run may need some time. Orange should appear in English.

## Application of translations

Here's how to use Trubar to translate the sources to Slovenian.

```sh
# This is an example for Slovenian translations, from orange-translations/si
trubar translate -s orig/orange-canvas-core/orangecanvas -d trans/orange-canvas-core/orangecanvas --static orange-translations/si/orange-canvas-static orange-translations/si/orange-canvas-core.yaml
trubar translate -s orig/orange-widget-base/orangewidget -d trans/orange-widget-base/orangewidget orange-translations/si/orange-widget-base.yaml
trubar translate -s orig/orange3/Orange -d trans/orange3/Orange orange-translations/si/orange3.jaml
```

Running `python -m Orange.canvas` should now show Slovenian Orange.

Note that messages for orange3 are stored as .jaml, while orange-widget-base and orange-canvas-core are yaml. The latter will be reformatted in the future.

## Translating to other languages

Use Trubar to create template files from Slovenian files.

Slovenian message files contain translations, while strings that must not be translated are marked as such (`false`).
`trubar template` prepares a files that contains only the former.

```sh
cd orange-translations
mkdir es
trubar template -o es/orange3.jaml si/orange3.jaml
```

This will create es/orange3.jaml that will contain all strings that need to be translated. Open it in text editor, translate what wou wish and run

```sh
trubar translate -s orig/orange3/Orange -d trans/orange3/Orange orange-translations/es/orange3.jaml
python -m Orange.canvas
```

to apply translations and check them out.

Same for orange-widget-base and orange-canvas-core.

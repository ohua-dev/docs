# Ohua Documentation [![Documentation Status](https://readthedocs.org/projects/ohua/badge/?version=latest)](https://ohua.readthedocs.io/en/latest/?badge=latest)

Read the rendered version of this documentation on
[readthedocs](https://ohua.readthedocs.org)

## Installing

### Engine and building

Ohua's documentation uses the [Sphinx](http://www.sphinx-doc.org/) to generate
html documentation from
[reStructuredText](http://docutils.sourceforge.net/rst.html).

The Sphinx engine can be installed easily with `pip install Sphinx`.

Sphinx supports several different output types and there is a makefile provided
for running the build. For instance if we want to build the html version run
`make html`. The generated html will be in `build/html`.

Update to the [online hosted documentation](https://ohua.readthedocs.io/) simply
push to the master branch of this repository.

### Theme

The ohua docs use the Sphinx theme provided by ReadTheDocs. Instructions on how
to install the theme can be found
[here](https://github.com/snide/sphinx_rtd_theme).

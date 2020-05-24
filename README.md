[![Build Status](https://travis-ci.org/oprecomp/kwpilot-doc.svg?branch=master)](https://travis-ci.org/oprecomp/kwpilot-doc)  [![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) [![Python 3.6+](https://img.shields.io/badge/python-3.6+-blue.svg)](https://www.python.org/downloads/release/python-360/) [![made-with-sphinx-doc](https://img.shields.io/badge/Made%20with-Sphinx-1f425f.svg)](https://www.sphinx-doc.org/)

# kwpilot Documentation

[Documentation and tutorials for OPRECOMP kwpilot demonstrator.](https://oprecomp.github.io/kwpilot-doc/index.html)

There are two ways to contribute to the documentation of cloudFPGA project, the automating compilation and and the manual compilation.

### Automated documentation compilation

We adopt the following tools for automating the documentation of cloudFPGA project:
* [Sphinx](https://www.sphinx-doc.org/en/master/) is a tool that makes it easy to create intelligent and beautiful documentation.
* [Read the Docs (sphinx_rtd_theme)](https://readthedocs.org/) is a sphinx theme designed to look modern and be mobile-friendly.
* [Travis CI](https://travis-ci.org/) is a hosted continuous integration service used to build and test software projects hosted at GitHub.

The overall documentation compilation process is triggered by a new commit to 
[kwpilot-doc repository](https://github.com/oprecomp/kwpilot-doc). Then `Travis CI` is building the documentation 
for the kwpilot (general information, tutorials, etc.)on a containerized environment and pushes the generated 
static HTML files on the [`gh_pages` branch of kwpilot-doc repository](https://github.com/oprecomp/kwpilot-doc/tree/gh-pages). 
The repository is configured to match this branch to 
[GitHub Pages](https://help.github.com/en/github/working-with-github-pages/getting-started-with-github-pages) and 
also bypass [jekyll](https://jekyllrb.com/) processing of `GitHub Pages` by creating an empty file named `.nojekyll` 
on the repository. Eventually the final documentation is available [here](https://oprecomp.github.io/kwpilot-doc/index.html).

#### Step 1/1: Update Documentation

```bash
git clone git@git@github.com:oprecomp/kwpilot-doc.git kwpilot-doc
cd kwpilot-doc
< ... make your changes ... >
git push
firefox https://oprecomp.github.io/kwpilot-doc/index.html & (view your changes)
```

**NOTE**: the documentation compilation on Travis CI is expected to take several minutes, so be patient when you submit changes until the time these take effect on the documentation.

***

### Manual documentation compilation
In case you need to manually compile the documentation of kwpilot project on your local development environment, please follow these steps:

#### Step 1/3: Sphinx and dependencies setup

To generate the Sphinx based python documentations, you have to setup:
```bash
which python3.6
virtualenv sphinx -p /usr/bin/python3.6
. sphinx/bin/activate
git clone git@github.com:oprecomp/kwpilot-doc.git kwpilot-doc
pip install -r ./kwpilot-doc/docsrc/requirements.txt
```
#### Step 2/3: Rebuild Documentation

```bash
. sphinx/bin/activate
git checkout gh-pages  (assuming you are on kwpilot-doc folder)
< ... make your changes ... >
make clean
make html
firefox _build/html/index.html & (view your changes locally)
```

#### Step 3/3: Update Documentation

```
git checkout gh-pages (ensure you are on this branch)
git commit -am "rebuild docs"
git push
```

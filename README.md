# Swarm Guide

This is the source of the documentation of [Swarm](https://ethswarm.org/), written in [Sphinx](http://www.sphinx-doc.org/). Its built version is hosted on http://swarm-guide.readthedocs.io

## Building the docs

The entry point will be in `./build/html/index.html`.

### Native with Python

```
pip install -r requirements.txt
make html
```

### On a Debian or derivative:

```
sudo apt-get install python-sphinx python3-sphinx-rtd-theme
make html
```

### Using Docker

This way you will get the necessary environment set up for you inside the docker image:

`make html-docker`

### Tips

While editing you may want to start a process that continuously builds the docs in the background:

```
watch -n 5 "make html"
```

## Incentivisation paper

```
make SOURCE=sw^3 latexpdf
```

## Restructured text markup, sphinx

* cheat sheets:
    * `best <https://github.com/ralsina/rst-cheatsheet/blob/master/rst-cheatsheet.rst>`_
    * `quick <reference http://docutils.sourceforge.net/docs/user/rst/quickref.html>`_
    * `official <http://docutils.sourceforge.net/docs/user/rst/cheatsheet.txt>`_ (`output <http://docutils.sourceforge.net/docs/user/rst/cheatsheet.html>`_)
* `rst primer <http://sphinx-doc.org/rest.html>`_
* `inline <http://sphinx-doc.org/markup/inline.html>`_
* `extentions <http://sphinx-doc.org/extensions.html#builtin-sphinx-extensions>`_
    * `default  <http://www.sphinx-doc.org/en/stable/domains.html#the-standard-domain>`_
    * `js API <http://www.sphinx-doc.org/en/stable/domains.html#the-javascript-domain>`_
    * `http API <http://pythonhosted.org/sphinxcontrib-httpdomain>`_
    * `go <https://pypi.python.org/pypi/sphinxcontrib-golangdomain>`_

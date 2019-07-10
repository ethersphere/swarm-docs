# Swarm Guide

This is the sources of Swarm's documentation, written in sphinx, hosted on read the docs:
http://swarm-guide.readthedocs.io

## Building the source

After building the source you will find `index.html` in `./build/html/` folder.

### Requirements

- GNU Make
- Docker or Python (pip)

### Using Docker

After you have `docker` available just call `make html-docker`.

If you build with docker, this would help a lot to use the following comment ot regulary update your state without the need for manuel build:

Execute
```
watch -n 10 "sudo make html-docker"
```

### Native with Python

Execute
```
pip install -r requirements.txt
make html
```

## Compile documentation
On a Debian or derivative:

```
sudo apt-get install python-sphinx python3-sphinx-rtd-theme
make html
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

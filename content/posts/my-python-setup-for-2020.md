---
date: "2020-01-18"
title: My Python setup for 2020
draft: true
tags:
- python
---

After some trial and errors I found a Python setup I enjoy using:

### Pyenv for Python versions

[Pyenv](https://github.com/pyenv/pyenv) is really nice in the sense that it make it a breeze to try and use new Python versions that don't come bundled with your OS. e.g. try out the [Walrus operator](https://www.python.org/dev/peps/pep-0572/) and Python 3.8:

```bash
pyenv install 3.8.0
pyenv shell 3.8.0
```

And you are ready to go.

### Pyenv-virtualenv for virtual environments

Now that you are using Pyenv, you'll probably want to create virtual environment for your different applications. You can use Python's venv module, and that is fine, but with Pyenv-virtualenv you get some niceties like out-of-the-box compatibility with Pyenv and a clear interface:

```bash
pyenv virtualenv 3.8.0 my-project
```

### Pipx for Python applications

It's bad practice to install Python packages on a global environment since it can mess it up: different packages installed can override one another. On the other hand, it's pretty annoying to create a Python virtual environment for each new program you want to install and then add it to `PATH`. [Pipx](https://pipxproject.github.io/pipx/) solves that problem by creating the virtual environment for you and sym linking it to your local bin path. So to install black, you just write:

```shell
$ pipx install black
...
$ which black
/home/noam/.local/bin/black
```

### Poetry is the new pipenv

[Poetry](https://python-poetry.org/) and [Pipenv](https://pipenv-fork.readthedocs.io/en/latest/) are similar projects that have a joint appeal - improve Python's packaging and dependency management. Pipenv was the pioneering project, it made a big splash when it came out about 2.5 years ago and I was eager to try it out.  While using it, I found that many times it was slow and buggy (this was up to a year ago so things might have changed since). Poetry came out about a year later, and offers pretty much the same features, but in contrast with Pipenv, it worked really nicely for me from the get go. There are many nice features to it, but I'll just highlight a few:

- An elaborative and relatively quick dependency resolver. It shows which packages are at conflict and why. Another nice feature is to show dependecy graph with `poetry show --tree`:

```text
pandas 0.25.3 Powerful data structures for data analysis, time series, and statistics
├── numpy >=1.13.3
├── python-dateutil >=2.6.1
│   └── six >=1.5
└── pytz >=2017.2
...
```

- It replaces the renowned `setup.py` file with a `pyproject.toml` file that is pretty simple to create and understand (you can see an [example](https://github.com/noamelf/toshl-fixer/blob/master/pyproject.toml) in a project of mine).

- The `poetry.lock` file is such a great addition since it "snapshots" the environment and allows poetry to reproduce the exact packages in each new installation without writing them explicitly in the `pyproject.toml` file. This means that the project first-hand requirement remains very clear and separate from the verbose installation requirements (unlike using a regular `requirements.txt` file).

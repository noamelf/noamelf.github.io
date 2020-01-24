---
date: "2020-01-18"
title: My Python setup for 2020
tags:
- python
---

My Python setup used to change a lot, as I would find ad-hoc solutions for my needs. These days I've settled on a Python setup that satisfies pretty much all of my different use cases and is easy to use. It is composed of these tools:

### Pyenv for Python versions

[Pyenv](https://github.com/pyenv/pyenv) is nice in the sense that it makes it a breeze to try and use new Python versions that don't come bundled with your OS. e.g. try out the [Walrus operator](https://www.python.org/dev/peps/pep-0572/) and Python 3.8:

```bash
pyenv install 3.8.0
pyenv shell 3.8.0
```

And you are ready to go.

### Pyenv-virtualenv for virtual environments

Now that you are using Pyenv, you create virtual environments with different Python interpreters for your projects. Of course you could use Python's venv module, and that is fine, but with Pyenv-virtualenv you get a clear interface and a [Virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/) like user experience:

```bash
pyenv virtualenv 3.8.0 my-project
```

### Pipx for Python applications

It's a bad practice to install Python packages in your system's Python environment since it can mess it up: different packages installed can override one another and make system utilities dysfunctional. On the other hand, it's pretty annoying to create a virtual environment for each new program you want to install and then add it to `PATH`. [Pipx](https://pipxproject.github.io/pipx/) solves that problem by creating the virtual environment for you and symlinking it to your local bin path. So to install black, you just write:

```shell
$ pipx install black
...
$ which black
/home/noam/.local/bin/black
```

### Poetry is the new pipenv

[Poetry](https://python-poetry.org/) and [Pipenv](https://pipenv-fork.readthedocs.io/en/latest/) are similar projects that have a joint appeal - improve Python's packaging and dependency management. Pipenv was the pioneering project, it made a big splash when it came out about 2.5 years ago and I was eager to try it out. While using it, I found that many times it was slow and buggy (this was up to a year ago so things might have changed since). Poetry came out about a year later and offers pretty much the same features, but in contrast with Pipenv, it worked nicely for me from the get-go. There are many nice features to it, but I'll just highlight a few that I like especially:

- **Dependency resolver** - Poetry comes with an elaborative and relatively quick dependency resolver . It shows which packages are at conflict and why, so it's easy to understand and fix (if possible). This is a pretty big deal when adding requirements to an existing project. Doing so with pip and a simple `requirements.txt` file, can cause the application to break at run time, while using poetry shows you the conflict at the time you are adding it. Another nice related feature is the ability to print the dependency graph:

```sh
$ poetry show --tree
pandas 0.25.3 Powerful data structures for data analysis, time series, and statistics
├── numpy >=1.13.3
├── python-dateutil >=2.6.1
│ └── six >=1.5
└── pytz >=2017.2
...
```

- **pyproject.toml** - Poetry replaces the renowned `setup.py` file with a `pyproject.toml` file that is pretty simple to create and understand (you can see an [example](https://github.com/noamelf/toshl-fixer/blob/master/pyproject.toml) in a project of mine).

- **poetry.lock** - The `poetry.lock` file is such a great addition since it "snapshots" the environment and allows poetry to reproduce the exact packages in each new installation without writing them explicitly in the `pyproject.toml` file. This means that the project first-hand requirement remains very clear and separate from the verbose installation requirements (unlike using a regular `requirements.txt` file).

**Tip!** To use Poetry together with Pyenv, remember to activate the Python version specified in the `pyproject.toml` file before running poetry. There are 2 ways to do so:

1. Set the default Python interpreter for that project directory with `pyenv local 3.8.0`.
2. Run `pyenv shell 3.8.0` before each time you want to use Poetry.

### Good old Pycharm for full-blown development

[Pycharm](https://www.jetbrains.com/pycharm/) is not the fastest or best-looking editor around (I'm looking at you vscode), but it's a great workhorse for full-blown Python development. For quick scripts, I might just use vim or vscode, but for bigger projects, Pycharm's Python support is still the best around IMHO.

---

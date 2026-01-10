---
number: 7714
title: "Mismatch error introduced by #2484 makes it impossible to create some trivial packages"
type: issue
state: open
author: jsbueno
labels:
  - needs-mre
assignees: []
created_at: 2024-09-26T16:04:32Z
updated_at: 2024-09-28T19:08:04Z
url: https://github.com/astral-sh/uv/issues/7714
synced_at: 2026-01-10T01:24:18Z
---

# Mismatch error introduced by #2484 makes it impossible to create some trivial packages

---

_Issue opened by @jsbueno on 2024-09-26 16:04_

Relative to issue  #2484 

What is even the purpose of that artificial restriction?

Anyway, I don't think it is mimicking whatever PIP does (if it does): 
To be specific - I am editting a package with

```toml
[tool.setuptools]
# required named for package importing:
packages = ["pgu"]
 
@ ... 


[project]
# project name as published on pypi:
name = "pygame-pgu"
```

And `uv pip` fails with this mismatch error. 

It installs without any issue with native pip.

I am ok if there is any workaround that could be set
in  a `[tool.uv] ` session, but I obviously need a workaround, or fix - 
this can't really be such an uncomon thing.  Just in the same
project I have to deal with `pygame-ce` which is imported
as `pygame` - but then the specifier is a suffix. 
Thank you for taking the time to report an issue! We're glad to have you involved with uv.


My terminal output:
```
(env312) [jsbueno@fedora pgu]$ uv pip install .
error: Failed to build: `pgu @ file:///home/jsbueno/projetos/pgu`
  Caused by: Package metadata name `pygame-pgu` does not match given name `pgu`
(env312) [jsbueno@fedora pgu]$ uv --version
uv 0.4.16
```

I am attaching the current pyproject.toml (being activelly modified) bellow



---

_Comment by @jsbueno on 2024-09-26 16:06_

```toml
[build-system]
requires = ["setuptools", "wheel", "setuptools-scm>=8"]
build-backend = "setuptools.build_meta"

[tool.setuptools]
include-package-data = true
packages = ["pgu", "pgu.scripts", "pgu.examples", "pgu.data"]

[tool.setuptools.dynamic]
version = {attr = "pgu.__version__"}

[tool.setuptools.package-dir]
"pgu" = "pgu"
"pgu.scripts" = "scripts"
"pgu.examples" = "examples"
"pgu.data" = "data"

[project]
name = "pygame-pgu"
dynamic = ["version"]
description = "Phil's Pygame Utilities - a collection of handy modules and scripts for PyGame."
readme = {file = "README.md", content-type = "text/markdown"}
requires-python = ">= 3.8"
dependencies = [
    "pygame-ce > 2.0"
]
authors = [{ name = "Joao S. O. Bueno" }, { name = "Peter Rogers" }, { name = "Phil Hassey" }]
classifiers = [
    "Development Status :: 5 - Production/Stable",
    "Intended Audience :: Developers",
    "Programming Language :: Python :: 3.8",
    "Programming Language :: Python :: 3.9",
    "Programming Language :: Python :: 3.10",
    "Programming Language :: Python :: 3.11",
    "Programming Language :: Python :: 3.12",
    "Programming Language :: Python :: 3.13",
    "Programming Language :: Python :: Implementation :: CPython",
    "Programming Language :: Python :: Implementation :: PyPy",
    "License :: OSI Approved :: GNU Lesser General Public License v3 or later (LGPLv3+)",
    "Operating System :: OS Independent",
]

[project.urls]
repository = "https://github.com/parogers/pgu"


[project.optional-dependencies]
dev = []

[tool.pytest.ini_options]
testpaths = "tests"
python_files = "test_*.py"
python_functions = "test_*"
addopts = "-v --doctest-modules"

[tool.mypy]
mypy_path = "typecheck"
implicit_reexport = true
```

---

_Comment by @charliermarsh on 2024-09-26 16:19_

Can you share the output of running with` --verbose`? The invocation you'd want to see is `pygame-pgu @ file:///home/jsbueno/projetos/pgu` -- `pgu @ file:///home/jsbueno/projetos/pgu` is legitimately wrong.

---

_Label `needs-mre` added by @charliermarsh on 2024-09-26 16:22_

---

_Comment by @jsbueno on 2024-09-26 18:14_

Thanks  -
The thing is then putting the project name separated by an `@` of the project location - typing in 
`uv install "pygame-pgu @ ."`  worked for me 

So I am ok if you consider this can be  closed now - but the behavior from `pip` still differs - `pip install .`, without the project name just works. 

Attaching the run with verbose next.  (different machine and venv)

---

_Comment by @jsbueno on 2024-09-26 18:14_

[uv_pip_verbose.txt](https://github.com/user-attachments/files/17153971/uv_pip_verbose.txt)


---

_Comment by @charliermarsh on 2024-09-26 18:23_

It looks like you have a `PKG-INFO` file in the directory that lists the name as `pgu`. Is that true?

---

_Comment by @charliermarsh on 2024-09-26 18:23_

(Also, is this open-source? Can I test with it?)

---

_Comment by @jsbueno on 2024-09-26 20:12_

Yes, there is indeed a PKG-INFO file there.

The project is here: https://github.com/parogers/pgu  -
I have had commit access there for a while, and this week I took it to "ressurect" the thing - as it provide some tooling for pygame that is not widely available.

I am setting up the shift to pyproject.toml in the "pyproject" branch - https://github.com/parogers/pgu/tree/pyproject 

---

_Comment by @charliermarsh on 2024-09-28 19:08_

The issue is that `PKG-INFO` lists `Name: pgu`. But, isn't that incorrect? That's not the name of the package.

---

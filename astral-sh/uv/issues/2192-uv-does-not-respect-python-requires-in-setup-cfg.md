```yaml
number: 2192
title: "uv does not respect `python_requires` in setup.cfg"
type: issue
state: closed
author: danielhollas
labels:
  - bug
assignees: []
created_at: 2024-03-05T02:02:26Z
updated_at: 2024-03-14T02:06:07Z
url: https://github.com/astral-sh/uv/issues/2192
synced_at: 2026-01-12T15:58:36Z
```

# uv does not respect `python_requires` in setup.cfg

---

_@danielhollas_

In the [aiidalab-widgets-base](https://github.com/aiidalab/aiidalab-widgets-base/) package, we have a mostly empty `pyproject.toml` that only contains the build-backed section (and some tools-related sections). The dependencies are specified in `setup.cfg`, including the minimum python version, which is currently 3.9 as of version `aiidalab-widgets-base==2.2.0a0`.

When running on the system with python 3.8, installing this version from PyPI indeed fails

```console
$ uv pip install aiidalab-widgets-base==2.2.0a0
  × No solution found when resolving dependencies:
  ╰─▶ Because the current Python version (3.8.10) does not satisfy Python>=3.9
      and aiidalab-widgets-base==2.2.0a0 depends on Python>=3.9, we can
      conclude that aiidalab-widgets-base==2.2.0a0 cannot be used.
      And because you require aiidalab-widgets-base==2.2.0a0, we can conclude
      that the requirements are unsatisfiable.
``` 

However, when I try to build and install the package directly from the repo, the installation proceeds "without issues".

```
$ git clone https://github.com/aiidalab/aiidalab-widgets-base && cd aiidalab-widgets-base
$ uv venv
$ uv pip install -e .
   Built file:///home/hollas/temp/uv-test/aiidalab-widgets-base                                                                                                                                                    Built 1 editable in 537ms
Resolved 163 packages in 119ms
Installed 163 packages in 53ms
 + aiida-core==2.3.1
 + aiidalab==23.3.2
 + aiidalab-eln==0.1.2
 + aiidalab-widgets-base==2.2.0a0 (from file:///home/hollas/temp/uv-test/aiidalab-widgets-base)
 + aio-pika==6.8.1
...
...
...
```

Installation with `pip` fails (as it should)
```console
$ python -m pip --require-virtualenv install -e .
Obtaining file:///home/hollas/aiidalab-widgets-base
  Installing build dependencies ... done
  Checking if build backend supports build_editable ... done
  Getting requirements to build editable ... done
  Preparing editable metadata (pyproject.toml) ... done
Requirement already satisfied: PyCifRW~=4.4 in ./.venv/lib/python3.8/site-packages (from aiidalab_widgets_base==2.1.0) (4.4.6)
Requirement already satisfied: aiida-core<3,>=2.1 in ./.venv/lib/python3.8/site-packages (from aiidalab_widgets_base==2.1.0) (2.3.1)
Requirement already satisfied: aiidalab>=21.11.2 in ./.venv/lib/python3.8/site-packages (from aiidalab_widgets_base==2.1.0) (23.3.2)
Requirement already satisfied: aiidalab-eln>=0.1.2,~=0.1 in ./.venv/lib/python3.8/site-packages (from aiidalab_widgets_base==2.1.0) (0.1.2)
Requirement already satisfied: ansi2html~=1.6 in ./.venv/lib/python3.8/site-packages (from aiidalab_widgets_base==2.1.0) (1.9.1)
Requirement already satisfied: ase~=3.18 in ./.venv/lib/python3.8/site-packages (from aiidalab_widgets_base==2.1.0) (3.22.1)
Requirement already satisfied: bokeh~=2.0 in ./.venv/lib/python3.8/site-packages (from aiidalab_widgets_base==2.1.0) (2.4.3)
Requirement already satisfied: humanfriendly~=10.0 in ./.venv/lib/python3.8/site-packages (from aiidalab_widgets_base==2.1.0) (10.0)
Requirement already satisfied: ipytree~=0.2 in ./.venv/lib/python3.8/site-packages (from aiidalab_widgets_base==2.1.0) (0.2.2)
Requirement already satisfied: traitlets~=5.9.0 in ./.venv/lib/python3.8/site-packages (from aiidalab_widgets_base==2.1.0) (5.9.0)
Requirement already satisfied: ipywidgets~=7.7 in ./.venv/lib/python3.8/site-packages (from aiidalab_widgets_base==2.1.0) (7.7.4)
Requirement already satisfied: widgetsnbextension<3.6.3 in ./.venv/lib/python3.8/site-packages (from aiidalab_widgets_base==2.1.0) (3.6.2)
Requirement already satisfied: more-itertools~=8.0 in ./.venv/lib/python3.8/site-packages (from aiidalab_widgets_base==2.1.0) (8.14.0)
Requirement already satisfied: pymysql~=0.9 in ./.venv/lib/python3.8/site-packages (from aiidalab_widgets_base==2.1.0) (0.10.1)
Requirement already satisfied: nglview~=3.0 in ./.venv/lib/python3.8/site-packages (from aiidalab_widgets_base==2.1.0) (3.0.8)
Requirement already satisfied: spglib<3,>=1.14 in ./.venv/lib/python3.8/site-packages (from aiidalab_widgets_base==2.1.0) (2.3.1)
Requirement already satisfied: vapory~=0.1.2 in ./.venv/lib/python3.8/site-packages (from aiidalab_widgets_base==2.1.0) (0.1.2)
INFO: pip is looking at multiple versions of aiidalab-widgets-base to determine which version is compatible with other requirements. This could take a while.
ERROR: Package 'aiidalab-widgets-base' requires a different Python: 3.8.10 not in '>=3.9'
```

* OS: `uname -a` Linux 5.15.0-97-generic 107~20.04.1-Ubuntu SMP Fri Feb 9 14:20:11 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
* Python version: 3.8.10
* uv version: 0.1.14


---

_Label `bug` added by @charliermarsh on 2024-03-05 02:05_

---

_Comment by @charliermarsh on 2024-03-05 02:15_

I wonder how you're supposed to enforce this, given that you can't get the `Requires-Python` until you build the package.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-05 02:26_

---

_Comment by @danielhollas on 2024-03-05 02:28_

Yeah. The last two lines from the `pip` output posted above indeed seem to only stop after resolving other requirements...

```
INFO: pip is looking at multiple versions of aiidalab-widgets-base to determine which version is compatible with other requirements. This could take a while.
ERROR: Package 'aiidalab-widgets-base' requires a different Python: 3.8.10 not in '>=3.9'
```

Interestingly, when I try to run `pip install aiidalab-widgets-base==2.0.0a0` into a venv that already has this package installed (via `uv`), it will happily declare that the requirement is already satisfied and proceeds with the rest.

---

_Comment by @charliermarsh on 2024-03-05 02:29_

Yeah I think we need to build the package, then validate against the built metadata.

---

_Comment by @danielhollas on 2024-03-05 02:50_

The number of corner cases y'all need to deal with is insane --- thanks for all the work! :love_letter: 

Just a note that AWB install on GHA went from 1m10s for pip (with cache!) to 11s (without caching). :tada: 

---

_Comment by @charliermarsh on 2024-03-05 03:44_

That's awesome :)

By the way, if you run with `--strict`, I confirmed that we will warn after-the-fact:

```shell
Built 1 editable in 474ms
Resolved 2 packages in 1ms
Installed 2 packages in 1ms
 + iniconfig==2.0.0
 + setuptools-editable==0.1.0 (from file:///Users/crmarsh/workspace/puffin/scripts/editable-installs/setuptools_editable)
warning: The package `setuptools-editable` requires Python <3.8, but `3.12.1` is installed.
```

But we should add this validation at install time.

---

_Comment by @danielhollas on 2024-03-05 04:08_

> By the way, if you run with --strict, I confirmed that we will warn after-the-fact:

Cool! Funny, I've just found out about `--strict` an hour ago, and was a bit surprised that it's not a default, since I believe pip does this (or something similar at least) by default. If that's something that's up for discussion I'm happy to open an issue with more info / motivating cases.

---

_Closed by @charliermarsh on 2024-03-05 04:20_

---

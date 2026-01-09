---
number: 13538
title: Using uv run somefile.py and python somefile.py behave differently
type: issue
state: closed
author: DigitalMechanic
labels:
  - question
assignees: []
created_at: 2025-05-19T14:32:33Z
updated_at: 2025-05-20T13:30:11Z
url: https://github.com/astral-sh/uv/issues/13538
synced_at: 2026-01-07T13:12:18-06:00
---

# Using uv run somefile.py and python somefile.py behave differently

---

_Issue opened by @DigitalMechanic on 2025-05-19 14:32_

### Question

Not sure why uv isn't picking up the yaml import but running direct with python has no problem.

PS F:\project_dev\cb> python main.py
PS F:\project_dev\cb> uv run main.py
Traceback (most recent call last):
  File "F:\project_dev\cb\main.py", line 1, in <module>
    from plc_driver import PlcDriver as pd
  File "F:\project_dev\cb\plc_driver\__init__.py", line 1, in <module>
    from .plc_driver import PlcDriver
  File "F:\project_dev\cb\plc_driver\plc_driver.py", line 3, in <module>
    from cb_tools import CbTools
  File "F:\project_dev\cb\cb_tools\__init__.py", line 1, in <module>
    from .cb_tools import CbTools
  File "F:\project_dev\cb\cb_tools\cb_tools.py", line 2, in <module>
    import yaml
ModuleNotFoundError: No module named 'yaml'

The yaml package is definitely there... also ruff check shows no issues.

Thanks in advance!
-DigitalMechanic

### Platform

Windows 11/Python 3.13

### Version

uv 0.7.4 (6fbcd09b5 2025-05-15)

---

_Label `question` added by @DigitalMechanic on 2025-05-19 14:32_

---

_Comment by @konstin on 2025-05-19 15:42_

`uv run` uses the dependencies declared in `pyproject.toml` or in the inline script metadata - did you declare a dependency on the package providing `yaml`?

---

_Comment by @FishAlchemist on 2025-05-19 15:50_

Oh, right. It looks like you haven't activated a virtual environment, so I'm guessing you're using the global Python installation. That on Windows, if Python isn't installed , the ``python`` command often just triggers the Microsoft Store link for Python, and trying to run your code like that won't give you any output either.

Based on the information you've provided, this can't be avoided, so I'd like to add something.

---

_Comment by @DigitalMechanic on 2025-05-20 11:33_

Thanks FishAlchemist... I have tried that. When I do a 'uv add yaml' i t fails with:
`Creating virtual environment at: .venv
  x No solution found when resolving dependencies:
  `-> Because there are no versions of yaml and your project depends on yaml, we can conclude that your project's
      requirements are unsatisfiable.`

trying to add yaml to the pyptoject.toml with the --frozen option succeeds and adds a yaml item in the dependencies section, but it won't run and complains yaml doesn't exist. I don't get it because my python environment works for everything else I write, and runs fine regardless of running in venv or not.

---

_Comment by @charliermarsh on 2025-05-20 11:34_

You might be looking for `uv add pyyaml`? `yaml` is not a package on PyPI: https://pypi.org/project/yaml/

---

_Comment by @DigitalMechanic on 2025-05-20 11:45_

Okay @charliermarsh, that actually works! However, I'm really unclear as to why yaml and pyyaml are different in uv. I'm doing an 'import yaml' (which has always worked outside uv). I've never used pyyaml, always just yaml. 
Regardless, I have deliverables for a client that need to get done... although I'm really uncomfortable not knowing where my dependencies are being satisfied - guess I'll have the rtfm on pyyaml... personally, I think this may be an env bug in uv.

---

_Comment by @DigitalMechanic on 2025-05-20 11:51_

> Oh, right. It looks like you haven't activated a virtual environment, so I'm guessing you're using the global Python installation. That on Windows, if Python isn't installed , the `python` command often just triggers the Microsoft Store link for Python, and trying to run your code like that won't give you any output either.
> 
> Based on the information you've provided, this can't be avoided, so I'd like to add something.

I have permanently disabled and deleted all access to the MS Store... so that's never going to happen. I have multiple dev platforms including several flavors of linux - it just so happens one of my client's requirements is this project run on Windows 11.  I just starting using uv, and for the most part it's been really helpful except for this tiny snag of not recognizing and standard library like yaml. 

---

_Comment by @charliermarsh on 2025-05-20 13:16_

> Okay @charliermarsh, that actually works! However, I'm really unclear as to why yaml and pyyaml are different in uv. I'm doing an 'import yaml' (which has always worked outside uv). I've never used pyyaml, always just yaml.

This is normal and expected. The package name on PyPI and the _module name_ that you import don't have to be the same. If you've used `import yaml` in the past, it's almost certainly the same as `pyyaml`.

---

_Comment by @charliermarsh on 2025-05-20 13:17_

In other words, they ship the package under the PyPI name `pyyaml`, but the source code ships the module `yaml` and expects you to do `import yaml`: https://pyyaml.org/wiki/PyYAMLDocumentation. 

You can see the same pattern with packages like [scikit-learn](https://pypi.org/project/scikit-learn/), where you `import sklearn`.

---

_Closed by @charliermarsh on 2025-05-20 13:17_

---

_Comment by @DigitalMechanic on 2025-05-20 13:30_

Thanks @charliermarsh ! I understand now...


---

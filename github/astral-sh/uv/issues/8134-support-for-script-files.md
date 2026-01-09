---
number: 8134
title: Support for script-files
type: issue
state: closed
author: petrprikryl
labels:
  - question
assignees: []
created_at: 2024-10-11T23:00:02Z
updated_at: 2024-12-09T18:58:04Z
url: https://github.com/astral-sh/uv/issues/8134
synced_at: 2026-01-07T13:12:17-06:00
---

# Support for script-files

---

_Issue opened by @petrprikryl on 2024-10-11 23:00_

Hi, are there any plans for `script-files` support like in [setuptools `tool.setuptools.script-files`](
https://setuptools.pypa.io/en/latest/userguide/pyproject_config.html#setuptools-specific-configuration)? E.g. for shell scripts.

Maybe useful:
* https://discuss.python.org/t/are-python-package-developers-just-not-supposed-to-package-shell-scripts-as-a-general-practice/25665
* https://docs.python.org/3.11/distutils/setupscript.html#installing-scripts
* https://github.com/python-poetry/poetry/issues/2310


---

_Comment by @charliermarsh on 2024-10-14 14:10_

@konstin -- Would this be part of the build backend work (eventually), or is it separate?

---

_Label `question` added by @charliermarsh on 2024-10-14 14:10_

---

_Comment by @konstin on 2024-10-14 14:11_

Yes, adding scripts is part of the build backend configuration (there is a dedicated scripts folder in wheels).

---

_Comment by @BenConstable9 on 2024-12-03 12:24_

Is there any plan for this? 

I would like to move from Poetry to UV but this is blocking it for one of my projects. 

I would like the equivalent of: `tool.poetry.scripts`

---

_Comment by @my1e5 on 2024-12-03 12:31_

@BenConstable9 - I believe `[project.scripts]` which is part of the official `pyproject.toml` specification will cover the functionality you are looking for? See https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#creating-executable-scripts

Also [Using uv run as a task runner](https://github.com/astral-sh/uv/issues/5903) might be relevant?


---

_Comment by @zanieb on 2024-12-03 14:48_

Note that packages using `script-files` have been the cause of various bug reports already. That may affect its inclusion in our build backend? I'm not familiar with the state of the standards there though.

The `[project.scripts]` table is the recommended way to create cross-platform scripts. You can use that today https://docs.astral.sh/uv/concepts/projects/config/#command-line-interfaces (and you can also use `setuptools` and `script-files` with uv)

---

_Comment by @konstin on 2024-12-04 10:36_

There are three different features known as "scripts":
* `project.scripts` and `project.gui-scripts`: These are standards that should be supported by every build backend, and will also be supported by uv's build backend (https://github.com/astral-sh/uv/pull/9635). Note that they already work with any build backend you're using: After installing the package, the scripts are installed and added to PATH by `uv run` or virtual environment activation. Poetry calls those `tool.poetry.scripts`.
* The `scripts` data folder. Similar to `project.scripts` and `project.gui-scripts`, they are standards and already supported if your build backend uses them . The difference to `project.scripts` is that they aren't generated on installation, but just files copied to the correct target, so they are less portable.
* A task runner, such as https://github.com/astral-sh/uv/issues/5903

If you move from poetry to uv, you currently need to configure this for the build backend your using, not in uv itself.

---

_Comment by @BenConstable9 on 2024-12-09 18:55_

Thank you all. I had somehow missed this in the documentation. I will look to use it.

---

_Closed by @zanieb on 2024-12-09 18:58_

---

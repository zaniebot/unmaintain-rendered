```yaml
number: 2813
title: "Feature Request: Expose python and venv discovery as a machine interpretable uv subcommand"
type: issue
state: open
author: strangemonad
labels:
  - enhancement
  - wish
assignees: []
created_at: 2024-04-03T16:19:22Z
updated_at: 2024-11-06T02:13:42Z
url: https://github.com/astral-sh/uv/issues/2813
synced_at: 2026-01-12T15:58:40Z
```

# Feature Request: Expose python and venv discovery as a machine interpretable uv subcommand

---

_@strangemonad_

Many tools in the python development process could benefit from a consistent way of resolving the current project's python executable and virtual env. Since uv already has [documented logic for python discovery](https://github.com/astral-sh/uv?tab=readme-ov-file#python-discovery) it would be nice if we could just build off of this mechanism.

One example is how pyright tries to do its own python discovery but in many cases doesn't work properly. The way pyright actually works when it's invoked via pylance in vscode is closer to the following invocation `pyright --pythonpath $ABS_PATH_TO_PYTHON --project $ABS_PATH_TO_PROJECT_ROOT` see [this discussion on how to improve the python experience in Zed](https://github.com/zed-industries/zed/issues/7296#issuecomment-2013158802).

VSCode and PyCharm have similar issues where the user usually has to go hunt for the path to the python interpreter. Poetry does a similar python discovery, if you've installed poetry via `pipx` this means you already have an active venv so you find yourself needed to do `pyenv ... && poetry env use $(which python)` to make sure it doesn't pick the pipx base python install (often either system python or homebrew managed python rather than the pyenv managed version you actually want).

I'm sure there are details to think through, but a first proposal might be something like the following:
```bash
uv which python [PATH(default=PWD)]
```

where the path could be a PROJECT_ROOT (containing a pyproject.toml or some other marker) or any child path of the project root. I'm assuming ruff could also share some of this project root and python language version inference logic

---

_Comment by @zanieb on 2024-04-03 16:24_

Interesting idea! I'm not sure how we'd want to expose this but I feel the pain of finding versions.

See also #2386 which defines robust logic we want to implement for interpreter discovery.

---

_Label `enhancement` added by @zanieb on 2024-04-03 16:24_

---

_Label `wish` added by @zanieb on 2024-04-03 16:24_

---

_Comment by @bersbersbers on 2024-11-05 20:54_

I came across this issue when wondering how to figure out, in a script, how `uv` was installed on a system: specifically, I would like to know if the user installed `uv` via pip (in which case I should not uninstall `uv` during `uv pip sync`) or if it's a system-wide binary (in which case I do not need to special-case a `uv` pip package). Maybe figuring out `uv` kind of installation fits into this issue.

(If there are other ways to achieve what I describe above, I'm all ears, too.)

---

_Comment by @zanieb on 2024-11-05 21:11_

@bersbersbers I guess I'd just `which uv` and see if's in the Python environment's bin directory or not? You could also see if it's in the site-packages which won't be the case for non-pip installs.

---

_Comment by @bersbersbers on 2024-11-06 02:13_

@zanieb yes, that was my idea, too. It's not straightforward, given symlinks, the various Python distributions, system installations vs. virtual environments (various ones), etc. But I assume `uv` itself may not know better, so maybe I'll just try myself :)

---

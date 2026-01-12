```yaml
number: 13539
title: "Error and mix-up when using `uv init --bare`"
type: issue
state: closed
author: dennemeyer-lpe
labels:
  - question
assignees: []
created_at: 2025-05-19T14:53:23Z
updated_at: 2025-05-20T17:25:09Z
url: https://github.com/astral-sh/uv/issues/13539
synced_at: 2026-01-12T16:01:31Z
```

# Error and mix-up when using `uv init --bare`

---

_@dennemeyer-lpe_

### Summary

Hi,

First time commenting here.
I've been digging into the uv documentation since I got to know about it.
When initializing a `--bare` project, it creates a `pyproject.toml`.
When using `--python`, it specifies the `requires-python` in the `pyproject.toml`

However, there is a typo in [here](https://github.com/astral-sh/uv/blob/2894721b6e304c54d121dd90b59d5fd421e32f94/docs/concepts/projects/init.md?plain=1#L340). Shouldn't it be `--pin-python` (corresponding to [line 561 in crates/uv/tests/it/init.rs](https://github.com/astral-sh/uv/blob/2894721b6e304c54d121dd90b59d5fd421e32f94/crates/uv/tests/it/init.rs#L693)) instead of `--python-pin`?
The `--pin-python` flag is also not present in the `uv init` section of the CLI documentation (should be written [here](https://github.com/astral-sh/uv/blob/2894721b6e304c54d121dd90b59d5fd421e32f94/docs/reference/cli.md?plain=1#L690)).

Additional minor issue:
Why does `uv init --bare --python 3.8 --pin-python` write the version into `pyproject.toml` and create the same version in `.python-version` but when i first do `uv init --bare --python 3.8` and later do `uv pin python`, it takes the default uv managed python and not the version limited to the constraints set in the `pyproject.toml`? Doesn't the flag `--no-config` tell `uv python pin` to NOT look into configuration files like `pyproject.toml` in the current directory i.e. omitting `--no-config` should set `.python-version` to match the requirements set in the `pyproject.toml`?

Thanks

### Platform

MINGW64_NT-10.0-22631 3.5.7-463ebcdc.x86_64 x86_64 Msys

### Version

uv 0.7.5 (9d1a14e1f 2025-05-16)

### Python version

Python 3.11.12

---

_Label `bug` added by @dennemeyer-lpe on 2025-05-19 14:53_

---

_Comment by @charliermarsh on 2025-05-20 13:05_

I believe `--pin-python` is correct. For booleans, we don't show flags that match the default, so it shows up as `--no-pin-python` in the help:

```
Options:
  --name <NAME>                    The name of the project
  --bare                           Only create a `pyproject.toml`
  --package                        Set up the project to be built as a Python package
  --no-package                     Do not set up the project to be built as a Python package
  --app                            Create a project for an application
  --lib                            Create a project for a library
  --script                         Create a script
  --description <DESCRIPTION>      Set the project description
  --no-description                 Disable the description for the project
  --vcs <VCS>                      Initialize a version control system for the project [possible values: git, none]
  --build-backend <BUILD_BACKEND>  Initialize a build-backend of choice for the project [possible values: hatch, flit,
                                   pdm, poetry, setuptools, maturin, scikit]
  --no-readme                      Do not create a `README.md` file
  --author-from <AUTHOR_FROM>      Fill in the `authors` field in the `pyproject.toml` [possible values: auto, git, none]
  --no-pin-python                  Do not create a `.python-version` file for the project
  --no-workspace                   Avoid discovering a workspace and create a standalone project
```

---

_Comment by @charliermarsh on 2025-05-20 13:06_

For your second issue, as long as the Python version pinned in the second invocation is valid according to the `requires-python`, this sounds reasonable to me.

---

_Label `bug` removed by @charliermarsh on 2025-05-20 13:06_

---

_Label `question` added by @charliermarsh on 2025-05-20 13:06_

---

_Renamed from "Error and mix-up when using uv init --bare" to "Error and mix-up when using `uv init --bare`" by @dennemeyer-lpe on 2025-05-20 13:07_

---

_Comment by @dennemeyer-lpe on 2025-05-20 14:06_

> I believe `--pin-python` is correct. For booleans, we don't show flags that match the default, so it shows up as `--no-pin-python` in the help:

Thanks for the response.
Based on it, if you don't show flags that match the default, why are both `--pin-python` and `--no-pin-python` then valid flags, but `--readme` is not whereas `--no-readme` is?

It's all comes down to the setting that `uv init` creates a git repo, a .gitignore, a pyproject.toml, a .python-version, a README.md and a main.py, but several of those can be removed by doing `--vcs none`, `--no-readme`, `--no-pin-python`.

`uv init --bare` however only creates a pyproject.toml file, but others can be added by doing `--vcs git` or `--pin-python`. The latter only by guessing, because it is hidden deep in the docs. 
However, the sample main.py and the README.md are not able to be added explicitly when using `uv init --bare`



---

_Comment by @zanieb on 2025-05-20 15:31_

It just sounds like a small oversight. We can add a `--readme` flag to `uv init`.

For adding the `main` file, there's no a great way to express that in a flag name. That's why it doesn't exist, afaik. If you have any suggestions, I'm happy to consider them.

---

_Comment by @dennemeyer-lpe on 2025-05-20 17:13_

My suggestions:

* add `--pin-python` to the `uv init` documentation
* add `--readme` as flag for `uv init` to explicitly add a README.md and also add it to the `uv init` documentation
* allow `uv python pin` to set the `.python-version` according to the constraints set in `pyproject.toml` if existing



---

_Comment by @charliermarsh on 2025-05-20 17:21_

For the first thing: we shouldn’t make a change here that isn’t consistent with the rest of the CLI. So if we change that, we would need to change all other flags too. We can do it, but it’s not a matter of “just” changing this one flag on this one command.

---

_Comment by @dennemeyer-lpe on 2025-05-20 17:25_

I understand.
I believe you know best how to tackle this issue.
I just wanted to bring to your attention that there might be an issue.

---

_Closed by @dennemeyer-lpe on 2025-05-20 17:25_

---

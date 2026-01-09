---
number: 6232
title: "requires-python being set as * makes ruff fail"
type: issue
state: closed
author: a666
labels: []
assignees: []
created_at: 2023-08-01T08:08:02Z
updated_at: 2023-09-22T04:13:15Z
url: https://github.com/astral-sh/ruff/issues/6232
synced_at: 2026-01-07T13:12:15-06:00
---

# requires-python being set as * makes ruff fail

---

_Issue opened by @a666 on 2023-08-01 08:08_

Given:
```toml
#pyproject.toml
[project]
#...
requires-python = "*"
#...
[tool.ruff]
target-version = "py39"
```

And running:
```shell
$ ruff --fix something.py
ruff failed
  Cause: TOML parse error at line 9, column 19
  |
9 | requires-python = "*"
  |                   ^^^
Failed to parse version:
*
^
```
Shouldn't `target-version` take priority over `requires-python`? 
Then again I'm not even sure if setting requires-python as `"*"` is correct... 
Cannot find documentation about it, but seems to be a valid value in `pdm` and `poetry`...

Running command with `--isolated` is not relevant in this case (the toml config is the problem).

```shell
$ ruff --version
ruff 0.0.281
```

---

_Comment by @flying-sheep on 2023-08-01 09:55_

>  Then again I'm not even sure if setting requires-python as "*" is correct...

almost definitely not, only very very trivial snippets could still run with Python 1.0

---

_Comment by @zanieb on 2023-08-01 14:21_

Hm this is not really used in the wild https://github.com/search?q=%22requires-python+%3D+%5C%22*%5C%22%22&type=code — I do not think we should support this.

---

_Comment by @charliermarsh on 2023-08-01 15:48_

It should probably parse though, if it's valid according to the spec. @konstin would know.

---

_Comment by @zanieb on 2023-08-01 16:14_

This option follows the PEP-440 version specifiers spec (https://peps.python.org/pep-0440/) which does not appear to allow a lonely `*`.

---

_Comment by @a666 on 2023-08-01 16:15_

Also note that both [pdm](https://pdm.fming.dev/latest/usage/dependency/#save-version-specifiers) and [poetry](https://python-poetry.org/docs/dependency-specification/#wildcard-requirements) both seem to support it. Because it solves a real issue (what if you don't care about the version and only want the latest, setting `ruff = "*"` as dev dependency is a classic example).

The real question is if `requires-python` should be parsed if ruff's `target-version` is set?



---

_Comment by @konstin on 2023-08-01 16:19_

`*` is not a valid value for `requires-python` (it's defined through [Requires-Python](https://packaging.python.org/en/latest/specifications/core-metadata/#requires-python) which allows [trailing `.*`](https://peps.python.org/pep-0440/#version-matching) but no star alone. pdm 2.8.2 generates `requires-python = ">=3.8"` if i enter `*` in the interactive dialog for python requires. Poetry currently uses `tool.poetry.dependencies.python` with their own syntax which predates `[project]` (but they have plans to move to the standard too).

The reason why ruff fails to start even though we don't actually need the value is a bit more complex: We load the whole toml file into our schema, and our schema has a parsed version object, so we can't load it even if we don't need the value. The error message could be better though (it doesn't tell you about pyproject.toml or https://packaging.python.org/en/latest/specifications/declaring-project-metadata/#declaring-project-metadata or PEP 440).

---

_Comment by @a666 on 2023-08-01 16:28_

Understood, setting requires-python to a valid version spec.

Still, the PEPs always seem to fall short, and `*` seems like a valid value to me for certain usecases...

---

_Comment by @zanieb on 2023-08-01 16:33_

@a666 for which use-case? If it's not a valid specifier and not a correct declaration of compatible Python versions why not just use `>=3.7` or `>3`?

---

_Comment by @konstin on 2023-08-01 16:42_

> Also note that both [pdm](https://pdm.fming.dev/latest/usage/dependency/#save-version-specifiers) and [poetry](https://python-poetry.org/docs/dependency-specification/#wildcard-requirements) both seem to support it. Because it solves a real issue (what if you don't care about the version and only want the latest, setting ruff = "*" as dev dependency is a classic example).

The pdm one is interesting, because they are using the `*` very liberally/just metaphorically and don't actually write one. E.g. `pdm add --save-wildcard requests` writes:

```toml
[project]
name = "a"
version = "0.1.0"
description = ""
authors = [
    {name = "konstin", email = "konstin@mailbox.org"},
]
dependencies = [
    "requests",
]
requires-python = ">=3.8"
readme = "README.md"
license = {text = "MIT"}

[build-system]
requires = ["pdm-backend"]
build-backend = "pdm.backend"
```
There's no `*` in there, they just elide the version altogether! I'd very much not recommend that (there's clearly at least a minimum version you, if you don't write it down you risk version resolution picking a too old and cause weird hard to debug breakage), but it's possible. `requires-python` is an optional field, so the equivalent would be to elide the field. Again, i recommend determining the actual lower limit of your code and setting this; Given that you set `target-version = "py39"` i'd recommend `>=3.9`

Poetry otoh extended PEP 440 with `*` because they use a toml table instead of a list of strings (`package_name = "1.2.3"` instead of `package_name==1.2.3`). I don't know whether they will keep their PEP 440 extensions when [migrating to PEP 621](https://github.com/python-poetry/roadmap/issues/3). As much as i'd like to change PEP 440, this is a contentious topic and at least the case of any version is already covered for `[project]` by eliding the version.

(sorry for the wall of specification and packaging tooling minutiae, i used to work on packaging tooling and reimplemented PEP 440 so i have way to many details on this)

---

_Comment by @a666 on 2023-08-01 16:54_

Yeah, I was writing my own wall of text, mostly related to pdm and poetry stuff...

In short, we've had some issues with dependency management.
Setting `requires-python = ">=3.7"` makes every tool/lib/dependency version that declares `requires-python = ">=3.9"` uninstalable (or reverts to the version that agrees with ">=3.7"), so setting it to `"*"` would solve the issue.

---

_Comment by @charliermarsh on 2023-08-05 16:43_

@konstin @zanieb - Anyone have a strong opinion on how to resolve?

---

_Comment by @zanieb on 2023-08-05 18:21_

I guess I don't quite understand how setting it to `*` resolves the dependency resolution issue — this sounds like a discussion for the upstream package managers.

I think that at most we should handle invalid version specifiers in `requires-python` by throwing a warning and continuing with our default / oldest compatible Python version. I'm not sure it's our role to enforce this is a valid version specifier and I'm hesitant to special case an invalid specification like `*`.

---

_Comment by @konstin on 2023-08-07 09:06_

The solution for the project would be removing `requires-python` (it's an optional field) or setting it to the effective minimal version. From our end we could improve the error message be special casing `*`, but i wouldn't want to start accepting invalid pyproject.toml for fields that ruff's behavior depends on.

---

_Comment by @charliermarsh on 2023-09-22 04:13_

I'm going to close this for now -- it sounds like we'd prefer not to special case `*`, and we're not fully clear on why `*` is necessary.

---

_Closed by @charliermarsh on 2023-09-22 04:13_

---

_Referenced in [atloo1/python-cookiecutter#42](../../atloo1/python-cookiecutter/issues/42.md) on 2025-03-13 16:44_

---

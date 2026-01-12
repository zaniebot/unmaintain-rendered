```yaml
number: 7501
title: uv build setuptools setup should default to or honor index-url pip config
type: issue
state: closed
author: blackduckx
labels:
  - needs-mre
assignees: []
created_at: 2024-09-18T14:00:02Z
updated_at: 2024-09-19T01:09:07Z
url: https://github.com/astral-sh/uv/issues/7501
synced_at: 2026-01-12T15:59:14Z
```

# uv build setuptools setup should default to or honor index-url pip config

---

_@blackduckx_

Running `uv build` command without the `[build-system]` section defined in `pyproject.tom` installs `setuptools` by default.

Right now it connects directly to *pypi* and does not honor the `pip` `index-url` configuration in the *toml* config files.

-> Defaulting to `pip` `index-url` seems to be the expected default
-> Corporate Users mostly have to go through a pypi proxy



*Context*

* uv version: 0.4.11
* `uv.toml` configured with corporate pypi proxy 
* `uv pip install` command running fine (Installation through the corporate pypi proxy)


*Failure*
```
(XXX) > uv build

Building source distribution...
error: Failed to install requirements from `setup.py` build (resolve)
  Caused by: No solution found when resolving: setuptools>=40.8.0
  Caused by: Could not connect, are you offline?
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (https://pypi.org/simple/setuptools/)
  Caused by: client error (Connect)
  Caused by: dns error: Hôte inconnu. (os error 11001)
  Caused by: Hôte inconnu. (os error 11001)
```



<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @charliermarsh on 2024-09-18 14:20_

Builds do use the user-provided index. For example, given:

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []

[project.scripts]
hello = "foo:hello"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.uv]
index-url = "https://test.pypi.org/simple"
```

`uv build` correctly fails, because it can't find `hatchling` on the Test PyPI.

My guess is you have an error in your configuration. Can you share a reproducible example?

Note that you need to use `[tool.uv]` in your configuration, not `[tool.uv.pip]`.

---

_Label `needs-mre` added by @charliermarsh on 2024-09-18 14:20_

---

_Comment by @blackduckx on 2024-09-18 17:08_

@charliermarsh thanks for taking care of my case. I used an old version of the documentation to setup my `uv.toml`

I confirm the [current documentation on configuration is accurate](https://docs.astral.sh/uv/configuration/files/)

I simply moved the `uv.toml` file from

```
[pip]
index-url = xxx
```
To
```
index-url = xxx
```
And it now does work fine. Sorry for the trouble. Closing this ticket.

---

_Closed by @blackduckx on 2024-09-18 17:08_

---

_Comment by @charliermarsh on 2024-09-19 01:09_

No worries!

---

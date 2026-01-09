---
number: 6419
title: "Better error message for invalid `[project]` table"
type: issue
state: closed
author: soof-golan
labels:
  - error messages
  - external
assignees: []
created_at: 2024-08-22T09:51:28Z
updated_at: 2024-09-14T20:03:49Z
url: https://github.com/astral-sh/uv/issues/6419
synced_at: 2026-01-07T13:12:17-06:00
---

# Better error message for invalid `[project]` table

---

_Issue opened by @soof-golan on 2024-08-22 09:51_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hey Astral team!

I found an interesting behavior around installing a git based project that is managed by poetry (as opposed to any other build system).
Since the name of the project is defined under the `tool.poetry` section uv fails to pick it up when reading the pyproject.toml file.
I'm not sure if this is considered Poetry's fault, but pip definitely succeeds in installing this exact package. I have not checked whether this issue can be also replicated locally with a relative path package.

Many thanks 🙏 

# What Happens

uv fails to install a (seemingly) valid python dependency. pip successfully installs this project.

# Expected Behavior

uv should install the package

# Reproduction

```bash
cd $(mktemp -d)
uv venv
uv pip install -v git+https://github.com/elevenlabs/elevenlabs-python.git@6db2fdd7458ce5c7f1bbdc2c3936784f995acd44
```

outputs: 

```
Using Python 3.12.5
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
DEBUG uv 0.3.1
DEBUG Searching for Python interpreter in system path
DEBUG Found `cpython-3.12.5-macos-aarch64-none` at `/private/var/folders/f0/m68x433x79n628trx7zp0x3h0000gn/T/tmp.BZTiKhI3Af/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.5 environment at .venv/bin/python3
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: git+https://github.com/elevenlabs/elevenlabs-python.git@6db2fdd7458ce5c7f1bbdc2c3936784f995acd44
DEBUG Using request timeout of 30s
DEBUG Fetching source distribution from Git: https://github.com/elevenlabs/elevenlabs-python.git
DEBUG Acquired lock for `https://github.com/elevenlabs/elevenlabs-python`
DEBUG Using existing git source `Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/elevenlabs/elevenlabs-python.git", query: None, fragment: None }`
DEBUG Acquired lock for `/Users/soof/Library/Caches/uv/built-wheels-v3/git/8be1b43a58e07c63/6db2fdd7458ce5c7`
DEBUG No static `PKG-INFO` available for: git+https://github.com/elevenlabs/elevenlabs-python.git@6db2fdd7458ce5c7f1bbdc2c3936784f995acd44 (MissingPkgInfo)
error: Failed to extract static metadata from `pyproject.toml`
  Caused by: TOML parse error at line 31, column 2
   |
31 | [project.urls]
   |  ^^^^^^^
missing field `name`
```

<img width="1291" alt="image" src="https://github.com/user-attachments/assets/69d3fef1-0ee9-432b-a19d-f7b805ce3661">

### `uv --version`

```
uv 0.3.1 (be17d132a 2024-08-21)
```

### Platform

macOS Sonoma


---

_Label `bug` added by @konstin on 2024-08-22 09:57_

---

_Comment by @konstin on 2024-08-22 09:59_

We should fall back to building instead of trying to the read the metadata here, as we're doing in other cases, this is a bug.

Is there a reason you're using `[project.urls]` here instead of `[tool.poetry.urls]`?

---

_Label `good first issue` added by @konstin on 2024-08-22 09:59_

---

_Label `good first issue` removed by @konstin on 2024-08-22 09:59_

---

_Comment by @soof-golan on 2024-08-22 10:06_

@konstin 
Thanks for the fast response :)
This is not a project I own / maintain, but an external package that is specified as a git dependency in another project.

(This is why I wrote _seemingly valid_)

> uv fails to install a (seemingly) valid python dependency

---

_Comment by @charliermarsh on 2024-08-22 12:31_

@konstin -- I'm somewhat unsure... It _is_ spec-incompliant. If we accept it here, we risk false negatives for other projects.

---

_Label `bug` removed by @konstin on 2024-08-22 12:33_

---

_Comment by @konstin on 2024-08-22 12:34_

Oh right, because it's not dynamic like in the other cases.

We should change the error message with a note about the `[project]` table spec, maybe linking to https://packaging.python.org/en/latest/guides/writing-pyproject-toml/

---

_Assigned to @konstin by @konstin on 2024-08-26 08:39_

---

_Unassigned @konstin by @konstin on 2024-08-26 13:47_

---

_Comment by @konstin on 2024-08-26 13:48_

The toml crate doesn't give us this information unfortunately, so we can add better context to the error message: https://github.com/toml-rs/toml/issues/778

---

_Renamed from "uv fails to install git pacakges managed by poetry" to "Better error message for invalid `[project]` table" by @konstin on 2024-08-26 13:48_

---

_Label `upstream` added by @konstin on 2024-08-26 13:49_

---

_Label `error messages` added by @konstin on 2024-08-26 13:49_

---

_Referenced in [elevenlabs/elevenlabs-python#349](../../elevenlabs/elevenlabs-python/pulls/349.md) on 2024-08-28 10:22_

---

_Referenced in [elevenlabs/elevenlabs-python#350](../../elevenlabs/elevenlabs-python/issues/350.md) on 2024-08-28 10:30_

---

_Referenced in [astral-sh/uv#6803](../../astral-sh/uv/pulls/6803.md) on 2024-08-29 10:54_

---

_Referenced in [astral-sh/uv#6838](../../astral-sh/uv/issues/6838.md) on 2024-08-29 22:35_

---

_Closed by @charliermarsh on 2024-09-14 20:03_

---

_Closed by @charliermarsh on 2024-09-14 20:03_

---

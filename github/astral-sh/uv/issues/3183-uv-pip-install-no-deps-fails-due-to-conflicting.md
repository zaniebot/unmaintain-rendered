---
number: 3183
title: "`uv pip install --no-deps` fails due to conflicting requirements even though the `--no-deps` flag is given"
type: issue
state: closed
author: bluthej
labels:
  - bug
assignees: []
created_at: 2024-04-22T13:02:56Z
updated_at: 2024-04-22T17:18:05Z
url: https://github.com/astral-sh/uv/issues/3183
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv pip install --no-deps` fails due to conflicting requirements even though the `--no-deps` flag is given

---

_Issue opened by @bluthej on 2024-04-22 13:02_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
## Context

I have a special use case for a client whereby I need to provide some libraries which they install via a `requirements.txt`.
The purpose of this file is only to list the libraries I provide (as local paths), and there is another requirements file for the external dependencies.
We work like this because in my company we use git dependencies for our own libraries but our client does not have access to our private git repos.

The install instructions for them are to do a `pip install` for the external dependencies (pulled from PyPI), and then a second `pip install` with the requirements file I provide them. This works fine with `pip`, but not with `uv pip` because the top-level library depends on the other libraries via git dependencies (specified in a `pyproject.toml`), but they are also listed in the `requirements.txt`. This leads to:
```shell
error: Requirements contain conflicting URLs for package `blargh`:
```
even though I provide the `--no-deps` flag. With this flag, it works fine with `pip`. So it seems to me like there is some conflict resolution that's taking place for the git dependencies that should not be happening with `--no-deps`.

Here is the full command I run:
```shell
uv pip install --no-deps -r requirements.txt
```

## Platform and `uv` version

I am on Linux with `uv 0.1.35`.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-22 14:27_

---

_Label `bug` added by @charliermarsh on 2024-04-22 14:27_

---

_Comment by @charliermarsh on 2024-04-22 14:27_

Thanks, I can guess what's going on here.

---

_Referenced in [astral-sh/uv#3191](../../astral-sh/uv/pulls/3191.md) on 2024-04-22 17:07_

---

_Closed by @charliermarsh on 2024-04-22 17:18_

---

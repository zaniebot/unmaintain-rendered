```yaml
number: 16278
title: add more desc for externally_managed error
type: pull_request
state: closed
author: lambdaq
labels: []
assignees: []
base: main
head: patch-1
created_at: 2025-10-13T09:31:43Z
updated_at: 2025-10-13T09:49:03Z
url: https://github.com/astral-sh/uv/pull/16278
synced_at: 2026-01-10T06:36:15Z
```

# add more desc for externally_managed error

---

_Pull request opened by @lambdaq on 2025-10-13 09:31_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

I Googled for hours trying to solve this error message

> This Python installation is managed by uv and should not be modified.
> Consider creating a virtual environment with `uv venv`.

I propose change the second sentence as:

> Consider creating a virtual environment with `uv venv`, or use `--break-system-packages`.


## Test Plan

No need really, just adding some description text

---

_Comment by @konstin on 2025-10-13 09:38_

We intentionally do not recommend `--break-system-packages`. If the interpreter is marked as externally managed, we should respect that and not install in it anyway. `uv venv` is the correct recommendation here, and we should not add a recommendation to ignore PEP 668 in the error message.

---

_Closed by @lambdaq on 2025-10-13 09:46_

---

_Comment by @lambdaq on 2025-10-13 09:47_

bummer. I had a python setup with base_prefix==prefix and want `uv` to install some libs for me

It's inside a docker so there is ZERO reason to setup another venv. Everything should be kept minimal

I tried.

---

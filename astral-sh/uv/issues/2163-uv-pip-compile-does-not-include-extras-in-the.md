---
number: 2163
title: "`uv pip compile` does not include extras in the generated files"
type: issue
state: closed
author: silv-io
labels: []
assignees: []
created_at: 2024-03-04T16:23:49Z
updated_at: 2024-03-04T17:01:27Z
url: https://github.com/astral-sh/uv/issues/2163
synced_at: 2026-01-10T01:23:13Z
---

# `uv pip compile` does not include extras in the generated files

---

_Issue opened by @silv-io on 2024-03-04 16:23_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

# Expected behavior

When defining dependencies with extras in the project files, `pip-compile` also puts those extras in the compiled files. 

E.g.: `"python-jose[cryptography]>=3.1.0"` turns into: `"python-jose[cryptography]==3.3.0"`

# Bug

`uv pip compile` strips the extras like this:

`"python-jose[cryptography]>=3.1.0"` turns into: `"python-jose==3.3.0"`

This might lead to issues further down the line.

# Platform

* Linux AMD64
* `uv` version: `0.1.13`

---

_Comment by @charliermarsh on 2024-03-04 17:01_

Thanks! This is a duplicate of an existing issue: https://github.com/astral-sh/uv/issues/1595. Note that including or excluding the extras doesn't have any effect, since the extras marker just adds additional dependencies, and all those dependencies are flattened anyway. `pip-tools` is changing their default to strip extras in the next release -- the behavior would be identical to uv's.

---

_Closed by @charliermarsh on 2024-03-04 17:01_

---

_Referenced in [astral-sh/uv#3343](../../astral-sh/uv/issues/3343.md) on 2024-05-03 13:15_

---

_Referenced in [open-zaak/open-zaak#1828](../../open-zaak/open-zaak/pulls/1828.md) on 2024-12-19 11:17_

---

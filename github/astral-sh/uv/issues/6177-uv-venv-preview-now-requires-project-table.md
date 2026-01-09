---
number: 6177
title: "`uv venv --preview` now requires `[project]` table"
type: issue
state: closed
author: branchv
labels:
  - bug
  - preview
assignees: []
created_at: 2024-08-17T23:43:34Z
updated_at: 2024-08-18T18:50:47Z
url: https://github.com/astral-sh/uv/issues/6177
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv venv --preview` now requires `[project]` table

---

_Issue opened by @branchv on 2024-08-17 23:43_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Thanks for all your amazing work on `uv` and `ruff` :)

I noticed after https://github.com/astral-sh/uv/commit/0dcec9eba8fff90c7863451f6435de594c9c9624, `uv venv` now accidentally requires a `[project]` table if `pyproject.toml` exists:

```shell
$ cd "$(mktemp -d)"
$ touch pyproject.toml
$ uv venv --preview
× No `project` table found in: `$TMPDIR/tmp.t05D1l2KEK/pyproject.toml`
```

Of course, projects may have a `pyproject.toml` without having yet adopted PEP 621 (like https://github.com/pypa/trove-classifiers)

---

_Referenced in [astral-sh/uv#6178](../../astral-sh/uv/pulls/6178.md) on 2024-08-17 23:47_

---

_Comment by @zanieb on 2024-08-17 23:58_

Thanks for the report! We definitely shouldn't fail here.

Blocks #6166 

---

_Label `bug` added by @zanieb on 2024-08-17 23:58_

---

_Label `preview` added by @zanieb on 2024-08-17 23:58_

---

_Closed by @charliermarsh on 2024-08-18 18:50_

---

_Closed by @charliermarsh on 2024-08-18 18:50_

---

_Referenced in [astral-sh/uv#6550](../../astral-sh/uv/issues/6550.md) on 2024-08-23 22:15_

---

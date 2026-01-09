---
number: 5942
title: Unable to install subdirectory plugin using uv pip install
type: issue
state: closed
author: thomasjpfan
labels:
  - bug
assignees: []
created_at: 2024-08-08T23:20:24Z
updated_at: 2024-08-09T00:16:39Z
url: https://github.com/astral-sh/uv/issues/5942
synced_at: 2026-01-07T13:12:17-06:00
---

# Unable to install subdirectory plugin using uv pip install

---

_Issue opened by @thomasjpfan on 2024-08-08 23:20_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
In a clean environment, `uv pip install` on a subdirectory fails:

```bash
uv pip install \
	git+https://github.com/flyteorg/flytekit.git@master#subdirectory=plugins/flytekit-flyteinteractive \
    --dry-run
```

With error message:

```
 Updated https://github.com/flyteorg/flytekit.git (9666f15c)
error: Failed to parse entry for: `flytekit`
  Caused by: Package is not included as workspace package in `tool.uv.workspace`
```

but `pip install ...` works. 

I am using uv 0.2.34 on macOS 14.5.

---

_Label `bug` added by @charliermarsh on 2024-08-08 23:22_

---

_Comment by @charliermarsh on 2024-08-08 23:22_

Sorry, that looks like a bug.

---

_Comment by @charliermarsh on 2024-08-08 23:30_

I'll take a look now.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-09 00:02_

---

_Referenced in [astral-sh/uv#5944](../../astral-sh/uv/pulls/5944.md) on 2024-08-09 00:03_

---

_Closed by @charliermarsh on 2024-08-09 00:13_

---

_Closed by @charliermarsh on 2024-08-09 00:13_

---

_Comment by @charliermarsh on 2024-08-09 00:16_

Fixed in the next release, sorry about that.

---

_Referenced in [astral-sh/uv#6093](../../astral-sh/uv/issues/6093.md) on 2024-08-14 20:29_

---

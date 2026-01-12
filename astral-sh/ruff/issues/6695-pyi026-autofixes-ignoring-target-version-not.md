```yaml
number: 6695
title: PYI026 autofixes ignoring target_version, not importing from typing_extensions
type: issue
state: closed
author: Skylion007
labels:
  - bug
assignees: []
created_at: 2023-08-19T21:08:58Z
updated_at: 2023-08-19T22:16:45Z
url: https://github.com/astral-sh/ruff/issues/6695
synced_at: 2026-01-12T15:54:46Z
```

# PYI026 autofixes ignoring target_version, not importing from typing_extensions

---

_@Skylion007_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
PYI026s autofix is broken. It addes TypeAlias annotations everywhere, but unconditionally imports `from typing` when it should import `from typing_extensions` if the target_version is less than py310. The bug appears to present when the PR was initially introduced in https://github.com/astral-sh/ruff/pull/5844

https://beta.ruff.rs/docs/rules/type-alias-without-annotation/

---

_Renamed from "PYI autofixes ignoring target_version, not importing from typing_extensions" to "PYI026 autofixes ignoring target_version, not importing from typing_extensions" by @Skylion007 on 2023-08-19 21:13_

---

_Comment by @charliermarsh on 2023-08-19 21:26_

Will take a look now.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-19 21:26_

---

_Comment by @charliermarsh on 2023-08-19 21:26_

Thanks, will fix.

---

_Label `bug` added by @charliermarsh on 2023-08-19 21:26_

---

_Closed by @charliermarsh on 2023-08-19 22:16_

---

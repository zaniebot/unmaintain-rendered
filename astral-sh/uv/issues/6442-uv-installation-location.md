```yaml
number: 6442
title: "`uv` installation location"
type: issue
state: closed
author: vwxyzjn
labels:
  - help wanted
  - question
  - configuration
assignees: []
created_at: 2024-08-22T14:51:37Z
updated_at: 2024-09-17T04:06:37Z
url: https://github.com/astral-sh/uv/issues/6442
synced_at: 2026-01-12T15:59:04Z
```

# `uv` installation location

---

_@vwxyzjn_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Would it be possible to install `uv` to a different location other than the default one? We are using a shared filesystem which lives at a difference place than `/home`.


---

_Comment by @zanieb on 2024-08-22 15:23_

See #5455 — trying to figure out how to make that simpler to document still.

---

_Label `question` added by @zanieb on 2024-08-22 15:23_

---

_Label `configuration` added by @zanieb on 2024-08-22 15:24_

---

_Label `help wanted` added by @zanieb on 2024-08-23 13:48_

---

_Comment by @zanieb on 2024-09-17 04:06_

Done in https://github.com/astral-sh/uv/pull/7107

---

_Closed by @zanieb on 2024-09-17 04:06_

---

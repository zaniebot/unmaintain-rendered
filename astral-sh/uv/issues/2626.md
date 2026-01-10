```yaml
number: 2626
title: "error: Multiple index URLs specified even if they are the same URL"
type: issue
state: closed
author: AngellusMortis
labels:
  - bug
  - configuration
assignees: []
created_at: 2024-03-22T21:56:34Z
updated_at: 2024-03-23T01:19:10Z
url: https://github.com/astral-sh/uv/issues/2626
synced_at: 2026-01-10T05:40:32Z
```

# error: Multiple index URLs specified even if they are the same URL

---

_Issue opened by @AngellusMortis on 2024-03-22 21:56_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

```
$ uv pip install -r dev-requirements.txt -r requirements.txt
error: Multiple index URLs specified: `https://us-central1-python.pkg.dev/repo/python/simple/` vs.` https://us-central1-python.pkg.dev/repo/python/simple/

$ uv --version
uv 0.1.22                               
```



---

_Renamed from "Installing multiple requirements files each with an index URL errors even if they are the same" to "error: Multiple index URLs specified even if they are the same URL" by @AngellusMortis on 2024-03-22 21:58_

---

_Comment by @zanieb on 2024-03-22 22:45_

Hi! How are you specifying index URLs? via your requirements files?

---

_Comment by @AngellusMortis on 2024-03-22 22:47_

Yeah. @BakerNet already has a [PR](https://github.com/astral-sh/uv/pull/2627) to fix it (we work together, lol). Just made the issue for them to have something to reference.

---

_Label `bug` added by @zanieb on 2024-03-22 22:48_

---

_Label `configuration` added by @zanieb on 2024-03-22 22:48_

---

_Closed by @zanieb on 2024-03-23 01:19_

---

```yaml
number: 14186
title: "Request: Autofix for `future-annotations-in-stub`/`PYI044`"
type: issue
state: closed
author: Avasam
labels:
  - fixes
  - help wanted
assignees: []
created_at: 2024-11-08T01:22:44Z
updated_at: 2024-11-08T15:47:05Z
url: https://github.com/astral-sh/ruff/issues/14186
synced_at: 2026-01-10T11:09:56Z
```

# Request: Autofix for `future-annotations-in-stub`/`PYI044`

---

_Issue opened by @Avasam on 2024-11-08 01:22_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
This one sounds like a pretty safe autofix, as the description of the rule says: "`from __future__ import annotations` has no effect in stub files". It's basically the same autofix as https://docs.astral.sh/ruff/rules/unused-import/

---

_Label `fixes` added by @MichaReiser on 2024-11-08 06:09_

---

_Label `help wanted` added by @MichaReiser on 2024-11-08 06:09_

---

_Comment by @dylwil3 on 2024-11-08 06:24_

I think this autofix already exists but it's currently in preview.

```console
➜   echo "from __future__ import annotations" > tmp.pyi
➜   ruff check tmp.pyi --select PYI --preview --fix
Found 1 error (1 fixed, 0 remaining).
➜   cat tmp.pyi
➜  
```

---

_Comment by @InSyncWithFoo on 2024-11-08 07:39_

It was added a while ago in #12676. The fix is both safe and small, so it probably won't have to stay in preview for much longer.

---

_Closed by @MichaReiser on 2024-11-08 07:40_

---

_Comment by @Avasam on 2024-11-08 15:45_

Ah I must've missed it being in preview then. I thought I did run with --preview, but maybe not. Thanks !

---

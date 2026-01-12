```yaml
number: 19467
title: "Change `latest` Python version to Python 3.14"
type: issue
state: closed
author: dylwil3
labels:
  - breaking
  - python314
assignees: []
created_at: 2025-07-21T15:28:37Z
updated_at: 2025-10-07T16:23:13Z
url: https://github.com/astral-sh/ruff/issues/19467
synced_at: 2026-01-12T15:54:56Z
```

# Change `latest` Python version to Python 3.14

---

_@dylwil3_

To finalize our stabilization of support for Python 3.14, we should address this `TODO`:

https://github.com/astral-sh/ruff/blob/5cace28c3ecc29f050aa5a57c8659e3154e3211e/crates/ruff_python_ast/src/python_version.rs#L58-L61

---

_Added to milestone `v0.13` by @dylwil3 on 2025-07-21 15:28_

---

_Label `breaking` added by @dylwil3 on 2025-07-21 15:28_

---

_Label `python314` added by @dylwil3 on 2025-07-21 15:28_

---

_Comment by @MichaReiser on 2025-07-21 15:42_

Note: We have to wait until Python 3.14 is officially released. We might also want to time the next minor to be aligned with the 3.14 release.

---

_Comment by @dylwil3 on 2025-07-21 15:51_

[Python 3.14 release dates](https://peps.python.org/pep-0745/) are:

- Candidate 1: July 22 (tomorrow, as of this writing)
- Candidate 2: August 26
- Final: October 7

If we want to wait until the _final_ release, then most likely we will have one or two minors between now and then. We can update the milestone target accordingly.

---

_Removed from milestone `v0.13` by @ntBre on 2025-09-05 13:12_

---

_Added to milestone `v0.14` by @ntBre on 2025-09-05 13:12_

---

_Closed by @ntBre on 2025-10-07 16:23_

---

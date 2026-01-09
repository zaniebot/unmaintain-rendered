---
number: 13869
title: Support for PEP 639 in RUF200
type: issue
state: closed
author: henryiii
labels:
  - rule
assignees: []
created_at: 2024-10-21T20:31:09Z
updated_at: 2024-11-18T17:20:17Z
url: https://github.com/astral-sh/ruff/issues/13869
synced_at: 2026-01-07T13:12:16-06:00
---

# Support for PEP 639 in RUF200

---

_Issue opened by @henryiii on 2024-10-21 20:31_

In `--preview`, the following error is shown for PEP 639 style licenses:

```
tests/packages/spdx/pyproject.toml:5:17: RUF200 Failed to parse pyproject.toml: wanted string or table
  |
3 | version = "1.2.3"
4 | license = "MIT OR GPL-2.0-or-later OR (FSFUL AND BSD-2-Clause)"
5 | license-files = ["LICEN[CS]E*", "AUTHORS*", "licenses/LICENSE.MIT"]
  |                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ RUF200
```

I don't fully understand the error message (`license-files` was never a string or table), but it would be good to support PEP 639, ~~at least before this leaves preview~~ (Edit: Ahh, this happens if you run `ruff check .` even without `--preview`, I always use pre-commit except for checking `--preview`).

---

_Comment by @MichaReiser on 2024-10-22 06:39_

Agree, we should change the validation here. 

@konstin do you know why we flag `license-files` if it isn't a table?

---

_Added to milestone `v0.8` by @MichaReiser on 2024-10-22 06:39_

---

_Label `rule` added by @MichaReiser on 2024-10-22 06:39_

---

_Label `help wanted` added by @MichaReiser on 2024-10-22 06:39_

---

_Comment by @konstin on 2024-10-22 07:58_

The `pyproject-toml` crate has an outdated version of the PEP, we need to fix this in that crate which will fix the rule.

---

_Referenced in [astral-sh/ruff#13902](../../astral-sh/ruff/pulls/13902.md) on 2024-10-24 09:26_

---

_Label `help wanted` removed by @konstin on 2024-10-24 09:28_

---

_Comment by @AlexWaygood on 2024-11-18 17:20_

This has been merged into the `ruff-0.8` branch; it will land in the next release (Ruff 0.8)

---

_Closed by @AlexWaygood on 2024-11-18 17:20_

---

_Referenced in [aeecleclair/Hyperion#700](../../aeecleclair/Hyperion/pulls/700.md) on 2025-05-02 22:17_

---

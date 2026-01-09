---
number: 20742
title: Conftest imports get flag as not used, but they are not.
type: issue
state: closed
author: ferwanguer
labels:
  - question
assignees: []
created_at: 2025-10-07T10:16:51Z
updated_at: 2025-10-08T08:50:47Z
url: https://github.com/astral-sh/ruff/issues/20742
synced_at: 2026-01-07T13:12:16-06:00
---

# Conftest imports get flag as not used, but they are not.

---

_Issue opened by @ferwanguer on 2025-10-07 10:16_

### Summary

When importing fixtures into conftest (using pytest) the imports are necessary for the test to work and detect the fixtures, but the imports get flagged as unnecessary. 

I normally just put a comment to ignore the linter. Anyway. Thanks!

### Version

_No response_

---

_Comment by @ntBre on 2025-10-07 18:02_

This sounds like it might be a good use for the [per-file-ignores](https://docs.astral.sh/ruff/settings/#lint_per-file-ignores) setting, if I understand correctly.

---

_Label `question` added by @ntBre on 2025-10-07 18:02_

---

_Comment by @ferwanguer on 2025-10-08 08:50_

Good enough! Thank you!

---

_Closed by @ferwanguer on 2025-10-08 08:50_

---

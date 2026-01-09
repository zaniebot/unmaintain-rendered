---
number: 18596
title: PLW0177 has false negatives for strings that represent NaN
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
  - help wanted
assignees: []
created_at: 2025-06-09T17:43:17Z
updated_at: 2025-06-19T11:05:07Z
url: https://github.com/astral-sh/ruff/issues/18596
synced_at: 2026-01-07T13:12:16-06:00
---

# PLW0177 has false negatives for strings that represent NaN

---

_Issue opened by @dscorbett on 2025-06-09 17:43_

### Summary

[`nan-comparison` (PLW0177)](https://docs.astral.sh/ruff/rules/nan-comparison/) doesn’t detect `float` calls that produce NaN if their arguments contain positive or negative signs or white space. Compare [`verbose-decimal-constructor` (FURB157)](https://docs.astral.sh/ruff/rules/verbose-decimal-constructor/), which does detect them.

```console
$ cat >plw0177.py <<'# EOF'
float("\n+nAn\xA0") == float("-NaN ")
# EOF

$ ruff --isolated check plw0177.py --select PLW0177 --preview
All checks passed!
```

### Version

ruff 0.11.13 (5faf72a4d 2025-06-05)

---

_Label `bug` added by @ntBre on 2025-06-09 17:46_

---

_Label `rule` added by @ntBre on 2025-06-09 17:46_

---

_Comment by @ntBre on 2025-06-09 17:50_

Hopefully we can factor out some of the code from https://github.com/astral-sh/ruff/pull/14098 and/or https://github.com/astral-sh/ruff/pull/14596 to help here.

---

_Label `help wanted` added by @ntBre on 2025-06-09 17:50_

---

_Referenced in [astral-sh/ruff#18630](../../astral-sh/ruff/pulls/18630.md) on 2025-06-11 17:10_

---

_Closed by @MichaReiser on 2025-06-19 11:05_

---

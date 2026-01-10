```yaml
number: 18660
title: PLW0128 skips assignment targets in square brackets and after asterisks
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
  - help wanted
assignees: []
created_at: 2025-06-13T12:32:46Z
updated_at: 2025-06-16T19:02:31Z
url: https://github.com/astral-sh/ruff/issues/18660
synced_at: 2026-01-10T11:09:58Z
```

# PLW0128 skips assignment targets in square brackets and after asterisks

---

_Issue opened by @dscorbett on 2025-06-13 12:32_

### Summary

[`redeclared-assigned-name` (PLW0128)](https://docs.astral.sh/ruff/rules/redeclared-assigned-name/) has false negatives for assignment targets in square brackets or after asterisks. It already correctly checks within parentheses.

```console
$ cat >plw0128.py <<'# EOF'
x, *x = 1, 2
[x, x] = 1, 2
# EOF

$ ruff --isolated check plw0128.py --select PLW0128
All checks passed!
```

### Version

ruff 0.11.13 (5faf72a4d 2025-06-05)

---

_Label `rule` added by @ntBre on 2025-06-13 12:47_

---

_Label `bug` added by @ntBre on 2025-06-13 12:47_

---

_Label `help wanted` added by @ntBre on 2025-06-13 12:47_

---

_Closed by @ntBre on 2025-06-16 19:02_

---

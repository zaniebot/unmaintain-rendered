---
number: 18405
title: Fixes for PERF401 and PERF403 should parenthesize lambdas and ternary expressions
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-05-31T15:17:41Z
updated_at: 2025-06-05T14:57:09Z
url: https://github.com/astral-sh/ruff/issues/18405
synced_at: 2026-01-07T13:12:16-06:00
---

# Fixes for PERF401 and PERF403 should parenthesize lambdas and ternary expressions

---

_Issue opened by @dscorbett on 2025-05-31 15:17_

### Summary

The fixes for [`manual-list-comprehension` (PERF401)](https://docs.astral.sh/ruff/rules/manual-list-comprehension/) and [manual-dict-comprehension (PERF403)](https://docs.astral.sh/ruff/rules/manual-dict-comprehension/) should insert parentheses around the condition when it is a lambda expression or a ternary conditional expression. See #15050, which fixed the same issue for assignment expressions.

```console
$ cat >perf401.py <<'# EOF'
src = [1]
dst = []
for i in src:
    if True if True else False:
        dst.append(i)
for i in src:
    if lambda: 0:
        dst.append(i)
# EOF

$ ruff --isolated check perf401.py --select PERF401 --preview --unsafe-fixes --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.

$ cat >perf403.py <<'# EOF'
src = (("x", 1),)
dst = {}
for k, v in src:
    if True if True else False:
        dst[k] = v
for k, v in src:
    if lambda: 0:
        dst[k] = v
# EOF

$ ruff --isolated check perf403.py --select PERF403 --preview --unsafe-fixes --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

### Version

ruff 0.11.12 (aee3af0f7 2025-05-29)

---

_Label `bug` added by @ntBre on 2025-05-31 20:43_

---

_Label `fixes` added by @ntBre on 2025-05-31 20:43_

---

_Label `help wanted` added by @ntBre on 2025-05-31 20:43_

---

_Referenced in [astral-sh/ruff#18412](../../astral-sh/ruff/pulls/18412.md) on 2025-06-01 15:38_

---

_Closed by @dylwil3 on 2025-06-05 14:57_

---

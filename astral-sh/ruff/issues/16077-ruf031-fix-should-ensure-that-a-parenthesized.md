---
number: 16077
title: RUF031 fix should ensure that a parenthesized single-element tuple has a comma
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - preview
assignees: []
created_at: 2025-02-10T14:29:36Z
updated_at: 2025-02-10T17:30:08Z
url: https://github.com/astral-sh/ruff/issues/16077
synced_at: 2026-01-10T01:22:57Z
---

# RUF031 fix should ensure that a parenthesized single-element tuple has a comma

---

_Issue opened by @dscorbett on 2025-02-10 14:29_

### Description

The fix for [`incorrectly-parenthesized-tuple-in-subscript` (RUF031)](https://docs.astral.sh/ruff/rules/incorrectly-parenthesized-tuple-in-subscript/) in Ruff 0.9.6 produces a syntax error when it tries to insert parentheses around a starred expression with no comma.
```console
$ cat >ruf031.py <<'# EOF'
x[*y]
# EOF

$ ruff --isolated check --preview --select RUF031 ruf031.py --config 'lint.ruff.parenthesize-tuple-in-subscript = true' --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```
The correct fix would be to insert a comma along with the parentheses:
```python
x[(*y,)]
```

---

_Label `bug` added by @MichaReiser on 2025-02-10 14:42_

---

_Label `fixes` added by @MichaReiser on 2025-02-10 14:42_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-02-10 16:16_

---

_Comment by @dylwil3 on 2025-02-10 17:04_

I think that even if the user selects `true` for `parenthesize-tuple-in-subscript` we should keep this as `x[*y]` (just for tuples of size one that are starred like this). It feels like the parenthesization in that case hurts readability in a way probably not intended.

---

_Label `preview` added by @dylwil3 on 2025-02-10 17:21_

---

_Referenced in [astral-sh/ruff#16083](../../astral-sh/ruff/pulls/16083.md) on 2025-02-10 17:21_

---

_Closed by @dylwil3 on 2025-02-10 17:30_

---

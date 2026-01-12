```yaml
number: 18387
title: UP050 fix should be unsafe when it deletes a comment
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-05-30T17:15:50Z
updated_at: 2025-06-03T13:10:16Z
url: https://github.com/astral-sh/ruff/issues/18387
synced_at: 2026-01-12T15:54:56Z
```

# UP050 fix should be unsafe when it deletes a comment

---

_@dscorbett_

### Summary

When the fix for [`useless-class-metaclass-type` (UP050)](https://docs.astral.sh/ruff/rules/useless-class-metaclass-type/) deletes a comment, it should be marked unsafe.

```console
$ cat >up050.py <<'# EOF'
class C(
    object,  # Explicit is better than implicit.
    metaclass=type,
): ...
# EOF

$ ruff --isolated check up050.py --select UP050 --preview --fix
Found 1 error (1 fixed, 0 remaining).

$ cat up050.py
class C(
    object,
): ...
```

### Version

ruff 0.11.12 (aee3af0f7 2025-05-29)

---

_Label `bug` added by @ntBre on 2025-05-30 17:25_

---

_Label `fixes` added by @ntBre on 2025-05-30 17:25_

---

_Label `help wanted` added by @ntBre on 2025-05-30 17:25_

---

_Comment by @chirizxc on 2025-05-30 17:55_

[useless-object-inheritance (UP004)](https://docs.astral.sh/ruff/rules/useless-object-inheritance/#useless-object-inheritance-up004) have same bug

https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP004.py.snap#L54

---

_Comment by @chirizxc on 2025-05-30 18:07_

ok, im working on this

---

_Assigned to @chirizxc by @ntBre on 2025-05-30 19:07_

---

_Closed by @ntBre on 2025-06-03 13:10_

---

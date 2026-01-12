```yaml
number: 16445
title: UP028 false positive when a target variable is global or nonlocal
type: issue
state: closed
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2025-02-28T21:18:09Z
updated_at: 2025-03-04T10:28:03Z
url: https://github.com/astral-sh/ruff/issues/16445
synced_at: 2026-01-12T15:54:55Z
```

# UP028 false positive when a target variable is global or nonlocal

---

_@dscorbett_

### Summary

The fix for [`yield-in-for-loop` (UP028)](https://docs.astral.sh/ruff/rules/yield-in-for-loop/) is incorrect when the `for` loop’s target list includes a global or nonlocal variable. Rewriting the loop with `yield from` means the variable isn’t updated. This is similar to #15725. The fix is already marked unsafe, but because in this context it is so likely to break the program, no fix should be provided.

```console
$ cat >up028.py <<'# EOF'
CURRENT_ELEMENT = None
def f(iterable):
    global CURRENT_ELEMENT
    for CURRENT_ELEMENT in iterable:
        yield CURRENT_ELEMENT
list(f("xyz"))
print(CURRENT_ELEMENT)
# EOF

$ python up028.py
z

$ ruff --isolated check --select UP028 up028.py --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ cat up028.py
CURRENT_ELEMENT = None
def f(iterable):
    global CURRENT_ELEMENT
    yield from iterable
list(f("xyz"))
print(CURRENT_ELEMENT)

$ python up028.py
None
```

### Version

ruff 0.9.9 (091d0af2a 2025-02-28)

---

_Label `bug` added by @ntBre on 2025-02-28 21:24_

---

_Closed by @MichaReiser on 2025-03-04 10:28_

---

_Closed by @MichaReiser on 2025-03-04 10:28_

---

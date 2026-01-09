---
number: 16537
title: "F841 treats `except` bindings differently from all other bindings"
type: issue
state: open
author: dscorbett
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-03-06T15:25:23Z
updated_at: 2025-05-21T19:05:44Z
url: https://github.com/astral-sh/ruff/issues/16537
synced_at: 2026-01-07T13:12:16-06:00
---

# F841 treats `except` bindings differently from all other bindings

---

_Issue opened by @dscorbett on 2025-03-06 15:25_

### Summary

The implementation of [unused-variable (F841)](https://docs.astral.sh/ruff/rules/unused-variable/) is split: `except` bindings [are handled separately in bindings.rs](https://github.com/astral-sh/ruff/blob/0.9.9/crates/ruff_linter/src/checkers/ast/analyze/bindings.rs#L33), causing false positives. They should be processed consistently with other bindings.

One false positive occurs when the bound variable is not used within the `except` block but is used outside it. The fix can change program behavior by suppressing a runtime error. The fix could be beneficial, but it should be marked unsafe like other unused bindings’ fixes.
```console
$ cat >f841_1.py <<'# EOF'
def f():
    e = None
    try:
        1 / 0
    except ZeroDivisionError as e:
        pass
    print(e)
f()
# EOF

$ python f841_1.py 2>&1 | tail -n 1
UnboundLocalError: cannot access local variable 'e' where it is not associated with a value

$ ruff --isolated check --select F841 f841_1.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat f841_1.py
def f():
    e = None
    try:
        1 / 0
    except ZeroDivisionError:
        pass
    print(e)
f()

$ python f841_1.py
None
```

Another false positive occurs when the binding is not within a function.
```console
$ cat >f841_2.py <<'# EOF'
e = None
try:
    1 / 0
except ZeroDivisionError as e:
    pass
print(e)
# EOF

$ python f841_2.py 2>&1 | tail -n 1
NameError: name 'e' is not defined

$ ruff --isolated check --select F841 f841_2.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat f841_2.py
e = None
try:
    1 / 0
except ZeroDivisionError:
    pass
print(e)

$ python f841_2.py
None
```

There are other false positives, but they are all variations on the same theme. They can probably all be fixed by merging the two F841 implementations.

### Version

ruff 0.9.9 (091d0af2a 2025-02-28)

---

_Label `bug` added by @ntBre on 2025-03-06 16:26_

---

_Label `help wanted` added by @ntBre on 2025-03-06 16:26_

---

_Comment by @teddylear on 2025-05-20 23:47_

@dscorbett I can try to give this a shot, but at little confused at the ask. I can make the edit unsafe that you listed, but not sure what you mean by `except bindings [are handled separately in bindings.rs`. I'm new to repo so trying to get clarification of ask before implementing. 

---

_Comment by @dscorbett on 2025-05-21 19:05_

F841 is triggered in two files, [deferred_scopes.rs](https://github.com/astral-sh/ruff/blob/0.11.10/crates/ruff_linter/src/checkers/ast/analyze/deferred_scopes.rs#L423) and [bindings.rs](https://github.com/astral-sh/ruff/blob/0.11.10/crates/ruff_linter/src/checkers/ast/analyze/bindings.rs#L33). The latter implementation has various false positives. My suggestion is to merge it into the former implementation. However, as long as they are consistent with each other and the false positives are resolved, merging them is not strictly necessary.

---

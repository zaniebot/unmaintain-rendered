```yaml
number: 13126
title: "FURB105 fix removes `sep` arguments with side effects"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - accepted
assignees: []
created_at: 2024-08-27T18:46:26Z
updated_at: 2024-08-30T16:17:48Z
url: https://github.com/astral-sh/ruff/issues/13126
synced_at: 2026-01-12T15:54:52Z
```

# FURB105 fix removes `sep` arguments with side effects

---

_@dscorbett_

The fix for [FURB105](https://docs.astral.sh/ruff/rules/print-empty-string/) removes unneeded `sep` arguments even when they might have side effects. In that case, the fix should be omitted or marked unsafe.

```console
$ ruff --version
ruff 0.6.2
$ cat furb105.py
print(sep=print("sep"))
$ python furb105.py
sep

$ ruff check --isolated --select FURB105 furb105.py --fix
Found 1 error (1 fixed, 0 remaining).
$ cat furb105.py
print()
$ python furb105.py


```

---

_Label `bug` added by @AlexWaygood on 2024-08-27 19:07_

---

_Label `fixes` added by @AlexWaygood on 2024-08-27 19:07_

---

_Comment by @AlexWaygood on 2024-08-27 19:13_

I'd be happy to accept a PR that marked the fix as unsafe if we detected the argument to `sep=` as something that could have a side effect. We could use the `contains_effect` function we have here for that purpose: https://github.com/astral-sh/ruff/blob/483748c188e854370836182c6f08b35fb140ae93/crates/ruff_python_ast/src/helpers.rs#L46.

However, I think we should still offer the fix (though it'll be marked as unsafe), even in these situations. I think it'll honestly be relatively rare for somebody to pass in a function that has a side effect to `sep=` here, and `contains_effect()` errs on the side of assuming that arbitrary functions might have side effects, although in reality they often won't.

---

_Label `accepted` added by @AlexWaygood on 2024-08-27 19:13_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-30 15:44_

---

_Closed by @charliermarsh on 2024-08-30 16:17_

---

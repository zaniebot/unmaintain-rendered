```yaml
number: 18298
title: "PYI041 fixes `None | int | None | float` to `None | None | float`"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-05-25T17:24:51Z
updated_at: 2025-06-20T19:04:52Z
url: https://github.com/astral-sh/ruff/issues/18298
synced_at: 2026-01-12T15:54:56Z
```

# PYI041 fixes `None | int | None | float` to `None | None | float`

---

_@dscorbett_

### Summary

The fix for [`redundant-numeric-union` (PYI041)](https://docs.astral.sh/ruff/rules/redundant-numeric-union/) can produce `None | None`, which can raise an exception.

```console
$ cat >pyi041.py <<'# EOF'
def foo(x: None | int | None | float) -> None: ...
# EOF

$ ruff --isolated check pyi041.py --select PYI041 --fix 
Found 1 error (1 fixed, 0 remaining).

$ python pyi041.py 
Traceback (most recent call last):
  File "pyi041.py", line 1, in <module>
    def foo(x: None | None | float) -> None: ...
               ~~~~~^~~~~~
TypeError: unsupported operand type(s) for |: 'NoneType' and 'NoneType'
```

One possible solution is to move the supertype to the position of the subtype if the subtype preceded it; for example, `None | int | None | float` would become `None | float | None`. `None | float | None | int` would still become `None | float | None` as it already does in Ruff 0.11.11.

### Version

ruff 0.11.11 (0397682f1 2025-05-22)

---

_Label `bug` added by @AlexWaygood on 2025-05-25 18:19_

---

_Label `fixes` added by @AlexWaygood on 2025-05-25 18:19_

---

_Label `help wanted` added by @AlexWaygood on 2025-05-25 18:19_

---

_Comment by @MichaReiser on 2025-05-26 07:07_

I think it also makes sense to simply bail out in this case and not provide a fix.

---

_Comment by @d-cryptic on 2025-05-26 10:43_

@AlexWaygood Can you please assign this issue to me? I will like to work on this

---

_Assigned to @d-cryptic by @MichaReiser on 2025-05-26 11:38_

---

_Comment by @robsdedude on 2025-06-10 20:43_

I'm wondering if `None` is the only type of this kind that makes problems.

Another thought: the type annotation is PEP 563 style  (`from __future import annotations`). The fix is actually fine.



---

_Comment by @AlexWaygood on 2025-06-10 20:49_

> I'm wondering if `None` is the only type of this kind that makes problems.

I'm pretty sure it is

---

_Comment by @robsdedude on 2025-06-10 21:07_

@d-cryptic are you still working on this? If not, I might have a stab at it in the coming days.

---

_Comment by @d-cryptic on 2025-06-11 18:50_

Hey @robsdedude 
Got occupied at work, please feel free to work on this issue

---

_Assigned to @robsdedude by @AlexWaygood on 2025-06-11 18:53_

---

_Unassigned @d-cryptic by @AlexWaygood on 2025-06-11 18:53_

---

_Closed by @ntBre on 2025-06-20 19:04_

---

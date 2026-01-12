```yaml
number: 15742
title: "RUF058 fix is broken when `zip` arguments are starred empty iterables"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
assignees: []
created_at: 2025-01-25T17:04:24Z
updated_at: 2025-01-25T18:42:29Z
url: https://github.com/astral-sh/ruff/issues/15742
synced_at: 2026-01-12T15:54:54Z
```

# RUF058 fix is broken when `zip` arguments are starred empty iterables

---

_@dscorbett_

### Description

The fix for [`starmap-zip` (RUF058)](https://docs.astral.sh/ruff/rules/starmap-zip/) in Ruff 0.9.3 can cause a `TypeError` at runtime when all the arguments to `zip` are starred empty iterables.
```console
$ cat >ruf058.py <<'#EOF'
from itertools import starmap
def f(x):
    for _ in starmap(print, zip(*x)):
        pass
f([])
#EOF

$ python ruf058.py

$ ruff --isolated check --preview --select RUF058 ruf058.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat ruf058.py
from itertools import starmap
def f(x):
    for _ in map(print, *x):
        pass
f([])

$ python ruf058.py
Traceback (most recent call last):
  File "ruf058.py", line 5, in <module>
    f([])
  File "ruf058.py", line 3, in f
    for _ in map(print, *x):
             ^^^^^^^^^^^^^^
TypeError: map() must have at least two arguments.
```


---

_Comment by @AlexWaygood on 2025-01-25 17:42_

Thanks! It seems reasonable to me to add a cautionary note about this to the rule's docs. However, I don't think this potential unsafety is enough to warrant marking the fix as always unsafe. It seems quite unlikely to come up in real code, and there are costs to marking a fix as always unsafe. I'd imagine a lot of our users never run Ruff with `--unsafe-fixes`, and so marking this fix as unsafe would mean that they would never get the benefit of the fix at all. If we have too many stylistic diagnostics that can't be easily autofixed, it can make running Ruff feeling like a burden that leads to needless busywork for our users, rather than something that they appreciate because it helps them write better, more idiomatic code.

---

_Label `documentation` added by @AlexWaygood on 2025-01-25 17:42_

---

_Label `fixes` added by @AlexWaygood on 2025-01-25 17:42_

---

_Comment by @InSyncWithFoo on 2025-01-25 18:02_

In this case, perhaps not emitting a diagnostic at all would be the correct thing to do. The user might be relying on `zip()` to take care of the empty case, and so they shouldn't be asked to change the code.

---

_Label `documentation` removed by @AlexWaygood on 2025-01-25 18:41_

---

_Label `fixes` removed by @AlexWaygood on 2025-01-25 18:41_

---

_Label `bug` added by @AlexWaygood on 2025-01-25 18:41_

---

_Label `rule` added by @AlexWaygood on 2025-01-25 18:41_

---

_Comment by @AlexWaygood on 2025-01-25 18:41_

Yes, fair point. Thanks!

---

_Closed by @AlexWaygood on 2025-01-25 18:42_

---

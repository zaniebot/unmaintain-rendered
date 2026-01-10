```yaml
number: 16457
title: FURB161 fix should be marked unsafe in some contexts
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-03-02T21:53:11Z
updated_at: 2025-04-18T17:46:02Z
url: https://github.com/astral-sh/ruff/issues/16457
synced_at: 2026-01-10T11:09:57Z
```

# FURB161 fix should be marked unsafe in some contexts

---

_Issue opened by @dscorbett on 2025-03-02 21:53_

### Summary

The fix for [bit-count (FURB161)](https://docs.astral.sh/ruff/rules/bit-count/) has a few problems.

When the argument to `bin` is a starred expression, the fix introduces a syntax error.
```console
$ cat >furb161_1.py <<'# EOF'
print(bin(*[123]).count("1"))
# EOF

$ python furb161_1.py
6

$ ruff --isolated check --target-version py310 --preview --select FURB161 furb161_1.py --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

The fix is marked safe, but it is only safe when the argument to `bin` is an instance of certain types, including `int` and `bool`, that have `__index__` and `bit_count` methods. If Ruff can’t determine that the argument is of one of those types, it should mark the fix unsafe. Otherwise, the fix can introduce a runtime error.
```console
$ cat >furb161_2.py <<'# EOF'
from plistlib import UID
print(bin(UID(123)).count("1"))
# EOF

$ python furb161_2.py
6

$ ruff --isolated check --target-version py310 --preview --select FURB161 furb161_2.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat furb161_2.py
from plistlib import UID
print(UID(123).bit_count())

$ python furb161_2.py
Traceback (most recent call last):
  File "furb161_2.py", line 2, in <module>
    print(UID(123).bit_count())
          ^^^^^^^^^^^^^^^^^^
AttributeError: 'UID' object has no attribute 'bit_count'
```

Another way the fix can be unsafe is by changing which exception is raised.
```console
$ cat >furb161_3.py <<'# EOF'
try:
    bin("123").count("1")
except Exception as e:
    print(repr(e))
# EOF

$ python furb161_3.py
TypeError("'str' object cannot be interpreted as an integer")

$ ruff --isolated check --target-version py310 --preview --select FURB161 furb161_3.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat furb161_3.py
try:
    "123".bit_count()
except Exception as e:
    print(repr(e))

$ python furb161_3.py
AttributeError("'str' object has no attribute 'bit_count'")
```

### Version

ruff 0.9.9 (091d0af2a 2025-02-28)

---

_Label `bug` added by @MichaReiser on 2025-03-03 09:05_

---

_Label `fixes` added by @MichaReiser on 2025-03-03 09:05_

---

_Label `help wanted` added by @MichaReiser on 2025-03-03 09:05_

---

_Comment by @VascoSch92 on 2025-03-04 05:11_

This seems pretty interesting. Can I fix it?

---

_Comment by @MichaReiser on 2025-03-04 09:39_

Sure. I suggest fixing the two issues in separate PRs. Marking the fix as unsafe is probably easier than the precedence issue

---

_Closed by @ntBre on 2025-04-18 17:46_

---

_Closed by @ntBre on 2025-04-18 17:46_

---

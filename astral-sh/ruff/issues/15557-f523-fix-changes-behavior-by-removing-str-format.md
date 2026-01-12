```yaml
number: 15557
title: "F523 fix changes behavior by removing `str.format` call"
type: issue
state: open
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-01-17T19:56:52Z
updated_at: 2025-08-04T14:06:52Z
url: https://github.com/astral-sh/ruff/issues/15557
synced_at: 2026-01-12T15:54:54Z
```

# F523 fix changes behavior by removing `str.format` call

---

_@dscorbett_

When the fix for [`string-dot-format-extra-positional-arguments` (F523)](https://docs.astral.sh/ruff/rules/string-dot-format-extra-positional-arguments/) removes a string’s `format` method call, it can change the program’s behavior. If the string contains a double brace like `{{`, it originally represents an escape sequence for a single brace like `{`, but with the method call removed it literally represents a double brace. If the string contains an unset field name, removing the method call suppresses the expected `KeyError`.

```console
$ ruff --version
ruff 0.9.2

$ cat f523.py
print("{{".format("!"))
print("{x}".format("!"))

$ python f523.py
{
Traceback (most recent call last):
  File "f523.py", line 2, in <module>
    print("{x}".format("!"))
          ^^^^^^^^^^^^^^^^^
KeyError: 'x'

$ ruff --isolated check --fix --select F523 f523.py
Found 2 errors (2 fixed, 0 remaining).

$ python f523.py
{{
{x}
```

---

_Label `bug` added by @MichaReiser on 2025-01-17 20:29_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-01-18 02:05_

---

_Label `fixes` added by @dylwil3 on 2025-01-18 02:07_

---

_Comment by @knavdeep152002 on 2025-05-10 12:13_

Hey @dscorbett ,

So, what should be the ideal fix for the example you mentioned? 

Should it be

```python
print("{")
print("{x}".format(x="!"))
```

Or is it expected to be something else? 



---

_Comment by @dscorbett on 2025-05-10 12:54_

The simplest fix would be to revert #15309:
```python
print("{{".format())
print("{x}".format())
```

The ideal fix would be:
```python
print("{")
print("{x}".format())
```

For the first expression, the `format` call can be removed if the double braces are unescaped into a single brace. For the second expression, the unused argument can be removed, but the `format` call can’t because that would suppress the `KeyError`.

---

_Unassigned @dylwil3 by @dylwil3 on 2025-08-04 14:06_

---

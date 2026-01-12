```yaml
number: 998
title: Implement flake8-simplify
type: issue
state: closed
author: charliermarsh
labels:
  - good first issue
  - plugin
assignees: []
created_at: 2022-12-02T15:36:49Z
updated_at: 2024-01-14T16:01:05Z
url: https://github.com/astral-sh/ruff/issues/998
synced_at: 2026-01-12T15:54:40Z
```

# Implement flake8-simplify

---

_@charliermarsh_

Python-specific rules:

- [x] `SIM101` ("Multiple isinstance-calls which can be merged into a single call by using a tuple as a second argument")
- [x] `SIM105` ("Use 'contextlib.suppress(...)' instead of try-except-pass")
- [x] `SIM107` ("Don't use `return` in try/except and finally ")
- [x] `SIM108` ("Use the ternary operator if it's reasonable ")
- [x] `SIM109` ("Use a tuple to compare a value against multiple values")
- [x] `SIM110` ("Use [any(...)](https://docs.python.org/3/library/functions.html#any) ")
- [x] `SIM111` ("Use [all(...)](https://docs.python.org/3/library/functions.html#all)")
- [x] `SIM113` ("Use enumerate instead of manually incrementing a counter")
- [x] `SIM114` ("Combine conditions via a logical or to prevent duplicating code")
- [x] `SIM115` ("Use context handler for opening files")
- [x] `SIM116` ("Use a dictionary instead of many if/else equality checks")
- [x] `SIM117` ("Merge with-statements that use the same scope")

Simplifying Comparisons:

- [x] `SIM201` ("Use 'a != b' instead of 'not a == b'")
- [x] `SIM202` ("Use 'a == b' instead of 'not a != b'")
- [x] `SIM203` ("Use 'a not in b' instead of 'not (a in b)'")
- [x] `SIM208` ("Use 'a' instead of 'not (not a)'")
- [x] `SIM210` ("Use 'bool(a)' instead of 'True if a else False'")
- [x] `SIM211` ("Use 'not a' instead of 'False if a else True'")
- [x] `SIM212` ("Use 'a if a else b' instead of 'b if not a else a'")
- [x] `SIM220` ("Use 'False' instead of 'a and not a'")
- [x] `SIM221` ("Use 'True' instead of 'a or not a'")
- [x] `SIM222` ("Use 'True' instead of '... or True'")
- [x] `SIM223` ("Use 'False' instead of '... and False'")
- [x] `SIM300` ("Use 'age == 42' instead of '42 == age'")

Simplifying usage of dictionaries:

- [x] `SIM401` ("Use 'a_dict.get(key, "default_value")' instead of an if-block")
- [x] `SIM118` ("Use 'key in dict' instead of 'key in dict.keys()'")

General Code Style:

- [x] `SIM102` ("Use a single if-statement instead of nested if-statements")
- [x] `SIM103` ("Return the boolean condition directly")
- [x] `SIM112` ("Use CAPITAL environment variables")


---

_Label `rule` added by @charliermarsh on 2022-12-02 15:36_

---

_Comment by @charliermarsh on 2022-12-02 16:50_

This plugin is particularly useful because we should be able to autofix most of these.

---

_Label `rule` removed by @charliermarsh on 2022-12-31 18:17_

---

_Label `plugin` added by @charliermarsh on 2022-12-31 18:17_

---

_Label `good first issue` added by @charliermarsh on 2023-01-04 01:03_

---

_Comment by @charliermarsh on 2023-01-04 20:44_

I'm working on `SIM110` and `SIM111`.

(These actually require some weird logic whereby we need to be able to peek at the next sibling, so TBD.)


---

_Comment by @danieleades on 2023-01-05 11:01_

looking good! I'll keep an eye on this issue and hopefully be able to remove the `flake8-simplify` plugin from Sphinx soon!

---

_Comment by @charliermarsh on 2023-01-06 00:32_

I'm working on `SIM101`.

---

_Comment by @messense on 2023-01-08 01:51_

> SIM106 ("Handle error-cases first")

`SIM106` was removed in `flake8-simplify` due to too many false-positives: https://github.com/MartinThoma/flake8-simplify/pull/108 

---

_Comment by @charliermarsh on 2023-01-08 03:17_

Removed, thanks!

---

_Comment by @jackklika on 2023-01-20 16:35_

FYI `SIM103` autofix refactored my code to be incorrect:

```
(apps-z6RSF3Uo-py3.10) jack@macbook api % cat /tmp/a.py 
def verify_user(kind):
    if kind != "USER":
        return False
    else:
        return True
(apps-z6RSF3Uo-py3.10) jack@macbook api % ruff /tmp/a.py
/tmp/a.py:2:5: SIM103 Return the condition `kind != "USER"` directly
Found 1 error(s).
1 potentially fixable with the --fix option.
(apps-z6RSF3Uo-py3.10) jack@macbook api % ruff /tmp/a.py --fix
Found 1 error(s) (1 fixed, 0 remaining).
(apps-z6RSF3Uo-py3.10) jack@macbook api % cat /tmp/a.py      
def verify_user(kind):
    return kind != "USER"
(apps-z6RSF3Uo-py3.10) jack@macbook api % 

```

In the first case, the function returns `False` if the kind is not `"USER"`, and True otherwise. In the fixed function, it returns the opposite. In the REPL:

```
>>> def verify_user(kind):
...     if kind != "USER":
...         return False
...     else:
...         return True
>>> verify_user("USER")
True
>>> def verify_user(kind):
...     return kind != "USER"
>>> verify_user("USER")
False
```

---

_Comment by @charliermarsh on 2023-01-20 16:57_

@jackklika - D'oh, thank you. I'll fix that this afternoon. Must be a flipped condition somewhere.

---

_Comment by @harupy on 2023-01-22 05:52_

Can I implement SIM124?

https://github.com/MartinThoma/flake8-simplify#sim909 says:

> The trial period starts on 28th of March and will end on 28th of September 2022.

---

_Comment by @charliermarsh on 2023-01-22 05:54_

Go for it!

---

_Comment by @harupy on 2023-01-22 06:03_

@charliermarsh Is there a way to compare two expressions while ignoring their contexts?

```python
foo = foo
^^^   ^^^
 |     |
 |      ---- Name { id: "foo", ctx: Load }
  ---------- Name { id: "foo", ctx: Store }
```

I tried `ComparableExpr` but it didn't work because the assignment target and value don't have the same context.

---

_Comment by @charliermarsh on 2023-01-22 18:26_

Hmm, no, we don't have anything to support that right now. Although arguably `ComparableExpr` should ignore `ctx`. Trying to think of where that would be incorrect.

---

_Closed by @charliermarsh on 2023-02-20 20:01_

---

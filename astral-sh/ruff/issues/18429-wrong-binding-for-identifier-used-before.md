```yaml
number: 18429
title: Wrong binding for identifier used before definition in class nested in function
type: issue
state: open
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2025-06-02T14:44:51Z
updated_at: 2025-06-04T22:25:44Z
url: https://github.com/astral-sh/ruff/issues/18429
synced_at: 2026-01-12T15:54:56Z
```

# Wrong binding for identifier used before definition in class nested in function

---

_@dscorbett_

### Summary

Ruff tracks bindings incorrectly when a class is defined within a function definition, and the function binds a certain identifier, and the class also binds that identifier, and the class loads that identifier before the binding. That load uses the global/builtin scope in Python, but Ruff treats it as loading from the enclosing function. That would be correct for a nested function, but not a nested class. This causes false positives and false negatives in many rules. I’ve given two examples below, but this is a general problem, not specific to those rules.

False negative:
```console
$ cat >b905.py <<'# EOF'
def f():
    zip = "outer"
    class Nested:
        zip("A", "B")  # False negative: B905 should recommend `strict` here
        zip = "inner"
# EOF

$ ruff --isolated check b905.py --select B905 --target-version py310
All checks passed!
```

False positive with incorrect fix:
```console
$ cat >f401.py <<'# EOF'
from sys import version
def f():
    version = 0
    class Nested:
        print(version)
        version = 1
        print(version)
    print(version)
f()
# EOF

$ python3.10 f401.py
3.10.14 (main, Mar 19 2024, 21:46:16) [Clang 15.0.0 (clang-1500.3.9.4)]
1
0

$ ruff --isolated check f401.py --select F401 --fix
Found 1 error (1 fixed, 0 remaining).

$ python3.10 f401.py 2>&1 | tail -n 1
NameError: name 'version' is not defined
```

### Version

ruff 0.11.12 (aee3af0f7 2025-05-29)

---

_Comment by @ntBre on 2025-06-02 16:12_

Wow that's pretty surprising, but here's the description in the [Python docs](https://docs.python.org/3/reference/executionmodel.html#naming):

> Class definition blocks and arguments to [exec()](https://docs.python.org/3/library/functions.html#exec) and [eval()](https://docs.python.org/3/library/functions.html#eval) are special in the context of name resolution. A class definition is an executable statement that may use and define names. **These references follow the normal rules for name resolution with an exception that unbound local variables are looked up in the global namespace.** The namespace of the class definition becomes the attribute dictionary of the class. The scope of names defined in a class block is limited to the class block; it does not extend to the code blocks of methods. This includes comprehensions and generator expressions, but it does not include [annotation scopes](https://docs.python.org/3/reference/executionmodel.html#annotation-scopes), which have access to their enclosing class scopes. This means that the following will fail:

I wonder if we might do something wrong with the end of that as well, with different behavior in annotations and comprehensions/generators.

Thanks for the report!

---

_Label `bug` added by @ntBre on 2025-06-02 16:13_

---

_Renamed from "Wrong binding for indentifier used before definition in class nested in function" to "Wrong binding for identifier used before definition in class nested in function" by @dscorbett on 2025-06-02 16:23_

---

_Comment by @LaBatata101 on 2025-06-04 22:08_

I can work on this

---

_Assigned to @LaBatata101 by @ntBre on 2025-06-04 22:25_

---

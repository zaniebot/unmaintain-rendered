```yaml
number: 2019
title: Unresolved reference on default function argument that gets a class attribute
type: issue
state: closed
author: benruijl
labels: []
assignees: []
created_at: 2025-12-17T15:45:44Z
updated_at: 2025-12-17T22:02:12Z
url: https://github.com/astral-sh/ty/issues/2019
synced_at: 2026-01-12T15:54:26Z
```

# Unresolved reference on default function argument that gets a class attribute

---

_@benruijl_

### Summary

```python
from enum import Enum

def E(mode: ParseMode = ParseMode.Symbolica):
    pass

class ParseMode(Enum):
    Symbolica = 1
```

or even simpler

```python

def E(mode: int = ParseMode.test):
    pass

class ParseMode:
    test = 1
```

yields 

```
Name `ParseMode` used when not defined (unresolved-reference) [Ln 3, Col 25]
```

The error does not occur when the class definition is moved before the function.

Playground: https://play.ty.dev/cffd74ca-33cc-4568-94fc-a01a0b99cff2

### Version

_No response_

---

_Comment by @charliermarsh on 2025-12-17 15:47_

That diagnostic seems correct -- are you seeing otherwise (at runtime)?

```
Traceback (most recent call last):
  File "/Users/crmarsh/workspace/foo.py", line 3, in <module>
    def E(mode: ParseMode = ParseMode.Symbolica):
                            ^^^^^^^^^
NameError: name 'ParseMode' is not defined
```

```
Traceback (most recent call last):
  File "/Users/crmarsh/workspace/foo.py", line 1, in <module>
    def E(mode: int = ParseMode.test):
                      ^^^^^^^^^
NameError: name 'ParseMode' is not defined
```


---

_Comment by @benruijl on 2025-12-17 15:57_

Aha, the file I am editing is a stub file (`.pyi` file)! Pylance can correctly detect the forward reference in this case.

Pylance for a py file:

<img width="822" height="220" alt="Image" src="https://github.com/user-attachments/assets/46e5a29b-f9c0-4d19-882a-daa99f6f522a" />

Pylance for a pyi file:

<img width="822" height="220" alt="Image" src="https://github.com/user-attachments/assets/28f9d21a-e91d-40d6-99e2-23f75eca717a" />

ty seems to be able to detect the forward reference on the type, as `mode: ParseMode` does not give an error, but it doesn't do it on the default argument.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-17 16:22_

---

_Unassigned @charliermarsh by @charliermarsh on 2025-12-17 16:23_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-17 16:31_

---

_Closed by @carljm on 2025-12-17 22:02_

---

```yaml
number: 15804
title: Improve docs for RUF012
type: issue
state: closed
author: RayBB
labels:
  - documentation
assignees: []
created_at: 2025-01-29T06:36:07Z
updated_at: 2025-02-06T14:01:11Z
url: https://github.com/astral-sh/ruff/issues/15804
synced_at: 2026-01-10T11:09:57Z
```

# Improve docs for RUF012

---

_Issue opened by @RayBB on 2025-01-29 06:36_

### Description

Hi there! I've been working on enabling RUF012 (Mutable class variables should not have default values) for OpenLibrary, and I believe the documentation for this rule could be enhanced to make it more actionable and clear. Specifically, I'd like to propose the following improvements:

## 1. Provide a Suggestion/Example for Immutable Types That Work for dict:
The current documentation for RUF012 highlights the issue of mutable default values in class variables but doesn't provide a concrete example of how to handle dict types immutably. A common solution is to use types.MappingProxyType, which creates an immutable wrapper around a dictionary. Adding an example of this would make the rule more practical and easier to implement.

Example:
```python
from types import MappingProxyType

class MyClass:
    # Instead of a mutable dict
    MY_DICT: dict[str, str] = {}

    # Use an immutable MappingProxyType
    MY_DICT: MappingProxyType[str, str] = MappingProxyType({"key": "value"})
```

## 2. Provide an Example of Moving a Variable to Be an Instance Variable:
Another way to address mutable class variables is to move them to instance variables initialized in `__init__`.

Example:
```python
class MyClass:
    # Instead of a mutable class variable
    MY_LIST: list[int] = []

    # Move it to an instance variable
    def __init__(self):
        self.my_list: list[int] = []
```

These additions would make the documentation more comprehensive and actionable, especially for developers who are new to the rule or Python's best practices for immutability.

I hope this is helpful. Unfortunately, I don't have time now to update the docs myself but wanted to at least create an issue for it. Thanks for your time and consideration!

---

_Label `documentation` added by @dylwil3 on 2025-01-29 10:43_

---

_Comment by @MichaReiser on 2025-02-02 16:14_

Thanks for the detailed write up. Would you be interested in creating a PR to improve the documentation? 

---

_Comment by @RayBB on 2025-02-02 16:16_

Sure I can make a PR. Do you wanna give any feedback on the examples now?

---

_Comment by @MichaReiser on 2025-02-03 09:52_

it looks good to me. I'd probably simplify 2 to have the annotation on the instance attribute 

```python
class A:
    mutable_default: list[int]
    immutable_default: list[int]

    def __init__(self):
        self.mutable_default = []
        self.immutable_default = []
``` 


@AlexWaygood do you have any inputs to the documentation?

---

_Comment by @AlexWaygood on 2025-02-03 11:20_

Improving the examples makes sense to me! These seem like great suggestions overall.

I'm undecided on whether it makes sense to mention `MappingProxyType` specifically. It's true that this is one way to fix the issue:

```diff
+ from types import MappingProxyType
+ from collections.abc import Mapping
+
  class A:
-     instance_dict: dict[str, str] = {"key": "value"}
+     instance_dict: Mapping[str, str] = MappingProxyType({"key": "value"})
```

But I think the more common way to fix the issue (assuming you want to keep it as an instance variable rather than a class variable) would be to do this:

```diff
  class A:
-     instance_dict: dict[str, str] = {"key": "value"}
+
+     def __init__(self) -> None:
+         self.instance_dict: dict[str, str] = {"key": "value"}
```

or this:

```diff
  class A:
-     instance_dict: dict[str, str] = {"key": "value"}
+     instance_dict: dict[str, str] | None = None
```

We could maybe mention `MappingProxyType` towards the bottom of the docs, but there can be costs to making the docs for a rule too long (it becomes overwhelming for users)

---

_Comment by @RayBB on 2025-02-05 21:52_

Hey folks, thanks for the input here.
I opened a PR based on your comments.
I'm not particularly attached to it so feel free to edit it directly and merge as you see fit.

https://github.com/astral-sh/ruff/pull/15982

---

_Closed by @MichaReiser on 2025-02-06 14:01_

---

_Closed by @MichaReiser on 2025-02-06 14:01_

---

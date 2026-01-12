```yaml
number: 15069
title: "Question: why doesn't ruff complain about CapsWords (PascalCase)?"
type: issue
state: open
author: tribunsky-kir
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-12-19T15:11:03Z
updated_at: 2024-12-23T08:22:11Z
url: https://github.com/astral-sh/ruff/issues/15069
synced_at: 2026-01-12T15:54:54Z
```

# Question: why doesn't ruff complain about CapsWords (PascalCase)?

---

_@tribunsky-kir_

[PEP 8](https://peps.python.org/pep-0008/) describes that:

> Class names should normally use the CapWords convention.

and that for [Method Names and Instance Variables](https://peps.python.org/pep-0008/#method-names-and-instance-variables)

> Use the function naming rules: lowercase with words separated by underscores as necessary to improve readability.

---

Short `example.py`:

```python
class MyClass:
    WhatIf: str
    WhySo: str
```

```bash
ruff check example.py --select ALL --ignore D
All checks passed!
```

```
ruff --version
ruff 0.8.3
```

---

_Renamed from "Question: why does ruff don't complain on CapsWords (PascalCase)?" to "Question: why doesn't ruff complain about CapsWords (PascalCase)?" by @tribunsky-kir on 2024-12-19 15:13_

---

_Comment by @Avasam on 2024-12-19 19:40_

Read-as-written, I believe attributes defined at the class-level are not instance variables, but rather class variables (unless you're using a dataclass)

```py
class MyClass:
    class_var = "bar"
    def __init__(self):
        self.instance_var = "foo"
```

That being said, I have no idea if PEP-8 intended to cover class variables here, or type-annotations-only declaractions.

---

_Comment by @tmke8 on 2024-12-19 20:56_

There is [N815](https://docs.astral.sh/ruff/rules/mixed-case-variable-in-class-scope/) which says:

Bad:

```python
class MyClass:
    myVariable = "hello"
    another_variable = "world"
```

Good:

```python
class MyClass:
    my_variable = "hello"
    another_variable = "world"
```

At first glance, it's surprising that it doesn't exclude PascalCase, but maybe there is a reason?

---

_Comment by @tribunsky-kir on 2024-12-20 00:18_

> Read-as-written, I believe attributes defined at the class-level are not instance variables, but rather class variables (unless you're using a dataclass)
> 
> class MyClass:
>     class_var = "bar"
>     def __init__(self):
>         self.instance_var = "foo"
> 
> That being said, I have no idea if PEP-8 intended to cover class variables here, or type-annotations-only declaractions.

Technically, I can't disagree, that it is class attribute (and I am not sure about explicit PEP-8 opinion on that). 

But [N815 ](https://docs.astral.sh/ruff/rules/mixed-case-variable-in-class-scope/) would complain on `mixedCase` in the absolutely same case, as mentioned above. Such behavior looks inconsistent and surprising to me.

For popular case like `pydantic` class attributes are more like _instance variables_.


---

_Comment by @MichaReiser on 2024-12-20 07:39_

These are just my own notes from looking into this issue:

The difference here doesn't come from whether the variable is attributed. It correctly flags `myVariable` but it doesn't flag `MyVariable`. See [playground](https://play.ruff.rs/0b9709d7-90e8-491a-87c2-cac1e144e779)

This sounds very familiar to https://github.com/astral-sh/ruff/issues/14905

Ruff's behavior is consistent with pep8-namiang

```py
class MyClass:
    MyVariable: int
    myVariable: int

```

```shell
❯ uv tool run --with pep8-naming flake8 test.py
test.py:3:6: N815 variable 'myVariable' in class scope should not be mixedCase
```

---

_Comment by @MichaReiser on 2024-12-20 07:52_

I did search the pep8-naming repository for an explanation but couldn't find any. 

My best guess is that CapNames are allowed because flagging them would be in conflict with the PEP8 recommendation for [type variables](https://peps.python.org/pep-0008/#type-variable-names). 



---

_Label `question` added by @MichaReiser on 2024-12-20 07:54_

---

_Comment by @Daverball on 2024-12-20 08:10_

@MichaReiser Another code pattern that could explain this are nested classes, where the nested class is swapped out using an assignment to a different class by a subclass. I've seen this pattern used in a few places over the years.

E.g. `webob.request.Request.ResponseClass` or `wtforms.Form.Config` come to mind, although in the latter case you're generally expected to always define a new nested class, rather than assign one. Although you can think of those as type variables too, since they're referring to a type.

I think we could come up with some heuristics for the given example with `AnnAssign` though. If the annotation doesn't refer to a `type`-like, then using PascalCase is wrong. If it's a regular `Assignment`, then we don't know, without performing type inference.

---

_Comment by @tmke8 on 2024-12-20 14:10_

I also sometimes use this pattern:

```python
from enum import Enum, auto
from typing import TypeAlias

class LoggerOption(Enum):
    INFO = auto()
    WARN = auto()

class Logger:
    Option: TypeAlias = LoggerOption

    def __init__(self, option: Option):  # using the alias
        self.option = option
```

In another file:
```python
from logger import Logger  # only Logger needs to be imported

logger = Logger(Logger.Option.INFO)
```

---

_Label `question` removed by @MichaReiser on 2024-12-23 08:22_

---

_Label `rule` added by @MichaReiser on 2024-12-23 08:22_

---

_Label `needs-decision` added by @MichaReiser on 2024-12-23 08:22_

---

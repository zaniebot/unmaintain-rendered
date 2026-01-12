```yaml
number: 17225
title: "add rule to disallow right-hand side values in `TypedDict`"
type: issue
state: open
author: vpoverennov
labels:
  - rule
assignees: []
created_at: 2025-04-05T20:58:51Z
updated_at: 2025-07-08T07:16:08Z
url: https://github.com/astral-sh/ruff/issues/17225
synced_at: 2026-01-12T15:54:55Z
```

# add rule to disallow right-hand side values in `TypedDict`

---

_@vpoverennov_

### Summary

```
from typing import TypedDict


class A(TypedDict):
    x: int = 42
           ^^^^
```

This is not allowed according to [PEP 589](https://peps.python.org/pep-0589/)
> TypedDict doesn’t support providing a default value type for keys that are not explicitly defined. This would allow arbitrary keys to be used with a TypedDict object, and only explicitly enumerated keys would receive special treatment compared to a normal, uniform dictionary type.

And is indeed ignored at runtime if you instantiate the class
```
>>> A()
{}
```


mypy:
```
y.py:5: error: Right hand side values are not supported in TypedDict  [misc]
```

pyright:
```
y.py:5:5 - error: TypedDict classes can contain only type annotations (reportGeneralTypeIssues)
```

Would be curious to get your thoughts for such a rule, thank you for making the awesome tool

---

_Renamed from "disallow right-hand side values TypedDict" to "disallow right-hand side values in `TypedDict`" by @vpoverennov on 2025-04-05 20:59_

---

_Renamed from "disallow right-hand side values in `TypedDict`" to "add rule to disallow right-hand side values in `TypedDict`" by @vpoverennov on 2025-04-05 20:59_

---

_Label `rule` added by @ntBre on 2025-04-06 00:28_

---

_Comment by @karthikeya9999 on 2025-04-06 06:06_

from typing import TypedDict

class A(TypedDict, total=False):
    x: int

a: A = {}  # x is optional
b: A = {'x': 42}  # okay too


---

_Comment by @MichaReiser on 2025-04-07 08:13_

@karthikeya9999 

I'm not sure I understand your comment. Is it in favor of the rule or an argument against it?

---

_Comment by @Sriketk on 2025-07-07 21:37_

I second this.

```py
class State(MessagesState):
    """Extended MessagesState with medical case information."""

    context: str
    question: str
    answer: str
    userAnswer: str = ""
    questionAnswered: bool = False

    # Optional medical information
    medications: list[str] = []
    allergies: list[str] = []
    familyHistory: list[str] = []
    labResults: list[str] = []
    options: list[str] = []
    bloodPresure: str = ""
    respirations: str = ""
    pulse: str = ""
    physicalExamination: str = ""
    temperature: str = ""
    history: str = ""
    demographics: str = ""


graph_builder = StateGraph(State)
```

linter is giving me error here 

```
src/agent/graph.py:117: error: Right hand side values are not supported in TypedDict  [misc]
```


---

_Comment by @MichaReiser on 2025-07-08 07:16_

Given that mypy and pyright already support this. I feel like it's less urgent for ruff to add this rule now (as it also depends on multifile analysis to resolve if the class extends a typed dict)

---

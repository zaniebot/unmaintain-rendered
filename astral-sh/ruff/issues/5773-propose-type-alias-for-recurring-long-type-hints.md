```yaml
number: 5773
title: Propose type alias for recurring long type hints
type: issue
state: open
author: sbrugman
labels:
  - needs-decision
assignees: []
created_at: 2023-07-15T09:01:17Z
updated_at: 2023-08-03T15:30:02Z
url: https://github.com/astral-sh/ruff/issues/5773
synced_at: 2026-01-10T11:09:48Z
```

# Propose type alias for recurring long type hints

---

_Issue opened by @sbrugman on 2023-07-15 09:01_

There is a pattern when using long type annotations multiple times that might make a useful linting rule. I've searched, but not found it (yet). (flake8-pyi #848, or flake-annotations might be candidates, but this might be just out of scope)

Motivating example:
```python
from typing import Literal


def apply_strategy(
   data: list,
   strategy: Literal["first", "cross_product", "exhaustive", "another_strategy"]
) -> None:
   """Apply the strategy."""
   ...

def suggest_strategy(
   data: list,
   strategy: Literal["first", "cross_product", "exhaustive", "another_strategy"]
) -> None:
   """Suggest the strategy."""
   ...

```

In cases of long, recurring, type hints a linter can suggest usage of type aliases:

```python
from typing import Literal

Strategy =  Literal["first", "cross_product", "exhaustive", "another_strategy"]

def apply_strategy(data: list, strategy: Strategy) -> None:
   """Apply the strategy."""
   ...

def suggest_strategy(data: list, strategy: Strategy) -> None:
   """Suggest the strategy."""
   ...

```

Apart from the naming of the type alias this would be autofixable. Perhaps only autofix if in addition to the type hints the argument/keyword arguments are the same.

Would this be something to include in `ruff`? 
I think the rule may have large coverage since `Literal` is an elementary type hint, but could extend to any recurring pattern.

---

_Label `needs-decision` added by @zanieb on 2023-08-03 15:30_

---

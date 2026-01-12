```yaml
number: 4887
title: Some rules should be disabled for typing protocols
type: issue
state: open
author: gaborbernat
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-06-06T01:17:53Z
updated_at: 2025-04-29T17:26:05Z
url: https://github.com/astral-sh/ruff/issues/4887
synced_at: 2026-01-12T15:54:45Z
```

# Some rules should be disabled for typing protocols

---

_@gaborbernat_

I can think of ANN101 and D102:

Given:
```python
class Message(Protocol):
    """Message on the event queue."""

    @property
    def message_id(self) -> str:
        ...

    def delete(self) -> None:
        ...
```

---

_Label `question` added by @charliermarsh on 2023-06-06 14:31_

---

_Comment by @charliermarsh on 2023-06-06 14:31_

Not necessarily disagreeing, but could you expand on the reasoning for omitting ANN101 and D102 here vs. in other classes? (E.g., I almost think ANN101 and ANN102 should be removed altogether.)

---

_Comment by @gaborbernat on 2023-06-06 15:07_

I'm happy to remove ANN101. As for D102 on a second think I can see both sides. On one hand protocols tend to be glue code, so less important to have docstrings, on other side might not be bad to have documentation for those methods 🤔 

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:11_

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:12_

---

_Label `rule` added by @charliermarsh on 2023-07-10 01:12_

---

_Comment by @tlambert03 on 2025-04-29 17:19_

here's a slightly interesting conflict between ruff and pyright:

ruff dislikes this:

```python
    @property
    def message_id(self) -> str: ...
```

`Missing docstring in public method Ruff(D102)`

whereas  pyright dislikes this:

```python
    @property
    def message_id(self) -> str:
        """Return the message ID."""
```

`Function with declared return type "str" must return value on all code paths
  "None" is not assignable to "str" Pylance(reportReturnType)`


meaning we essentially ~*must* now add an unnecessary `raise NotImplementedError` in order to satisfy both, or ignore one of them.~  *(edit ... silly me:  the ellipses suggested below is better)*

---

_Comment by @carljm on 2025-04-29 17:22_

Pyright is happier if you include the `...` along with the docstring:

```py
    @property
    def message_id(self) -> str:
        """Return the message ID."""
        ...
```

I guess the rationale here is that the `...` makes it explicit that you are intentionally omitting the implementation.

---

_Comment by @tlambert03 on 2025-04-29 17:24_

oh how silly of me 🤦‍♂️ ... yes of course.  much better than the raise

---

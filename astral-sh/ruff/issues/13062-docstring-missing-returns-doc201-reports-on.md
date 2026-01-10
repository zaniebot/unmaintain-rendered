```yaml
number: 13062
title: "docstring-missing-returns (DOC201) reports on explicit `None` returns in bodies that return `None` only"
type: issue
state: closed
author: tjkuson
labels:
  - docstring
  - accepted
assignees: []
created_at: 2024-08-22T18:35:46Z
updated_at: 2024-08-27T16:00:20Z
url: https://github.com/astral-sh/ruff/issues/13062
synced_at: 2026-01-10T11:09:55Z
```

# docstring-missing-returns (DOC201) reports on explicit `None` returns in bodies that return `None` only

---

_Issue opened by @tjkuson on 2024-08-22 18:35_

Running `ruff check --select DOC201 --preview --isolated` on

```python
def foo(obj: object) -> None:
    """A very helpful description."""
    if obj is None:
        return None
    print(obj)
```

reports a docstring-missing-returns (DOC201) diagnostic. Making the `None` return implicit means the diagnostic is raised no longer.

```python
def foo(obj: object) -> None:
    """A very helpful description."""
    if obj is None:
        return
    print(obj)
```

The expected behaviour is that the diagnostic is raised in neither situation, as the function returns `None` on all paths and the explicit return operates as an early exit.

I don't like the explicit `None` return and unnecessary-return-none (RET501) would catch this, but it still seems like the incorrect behaviour for this specific rule.

Reproduced on Ruff version 0.6.1.

Search terms: DOC201, docstring-missing-returns

---

_Label `docstring` added by @AlexWaygood on 2024-08-22 19:02_

---

_Label `accepted` added by @AlexWaygood on 2024-08-22 19:02_

---

_Closed by @AlexWaygood on 2024-08-27 16:00_

---

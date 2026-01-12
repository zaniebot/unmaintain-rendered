```yaml
number: 13079
title: "docstring-missing-returns (`DOC201`) reports on `__new__`"
type: issue
state: closed
author: tjkuson
labels:
  - rule
  - docstring
  - needs-decision
assignees: []
created_at: 2024-08-23T13:20:26Z
updated_at: 2024-09-10T17:25:40Z
url: https://github.com/astral-sh/ruff/issues/13079
synced_at: 2026-01-12T15:54:52Z
```

# docstring-missing-returns (`DOC201`) reports on `__new__`

---

_@tjkuson_

Running `ruff check --select DOC201 --preview --isolated` on

```python
class Foo:
    def __new__(cls, x: int) -> "Foo":
        """A very helpful docstring.

        Args:
            x: An integer.
        """
        ...  # Do something with x.
        return super().__new__(cls)
```

reports a docstring-missing-returns (DOC201) diagnostic.

The expected behaviour is that the return value of a `__new__` method is not expected to be documented as it simply returns an instance of the class.

Related to #8085

Search terms: DOC201, docstring-missing-returns, `__new__`


---

_Label `rule` added by @MichaReiser on 2024-09-02 06:24_

---

_Label `docstring` added by @MichaReiser on 2024-09-02 06:24_

---

_Label `needs-decision` added by @MichaReiser on 2024-09-02 06:24_

---

_Closed by @AlexWaygood on 2024-09-10 17:25_

---

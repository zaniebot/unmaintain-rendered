```yaml
number: 4465
title: "False positive `B030` when using `*exc_types or ValueError`"
type: issue
state: closed
author: jamesbraza
labels:
  - question
assignees: []
created_at: 2023-05-17T06:28:52Z
updated_at: 2023-05-18T00:54:10Z
url: https://github.com/astral-sh/ruff/issues/4465
synced_at: 2026-01-12T15:54:44Z
```

# False positive `B030` when using `*exc_types or ValueError`

---

_@jamesbraza_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

`ruff==0.0.265` has a false positive `B030`:

```python
def foo(msg: str, *exc_types: type[Exception]) -> None:
    try:
        raise ValueError(msg)
    except exc_types or ValueError as exc:
        print(exc)


foo("BOOM")
foo("BOOM2", ValueError)
foo("BOOM3", ValueError, IndexError)
```

```bash
> ruff --select=B030 a.py
a.py:4:12: B030 `except` handlers should only be exception classes or tuples of exception classes
Found 1 error.
```

---

_Comment by @tjkuson on 2023-05-17 08:30_

I don't think this is a false positive; binary operations shouldn't be used in `except` clauses, as, in your example, only the first exceptions (`exc_types`) will be caught.

Doing this instead passes the check and should work as you intend.

```python
def foo(msg: str, *exc_types: type[Exception]) -> None:
    try:
        raise ValueError(msg)
    except (*exc_types, ValueError) as exc:
        print(exc)


foo("BOOM")
foo("BOOM2", ValueError)
foo("BOOM3", ValueError, IndexError)
```

---

_Comment by @charliermarsh on 2023-05-17 13:22_

Yeah this looks like a true positive to me. What behavior are you seeing, and what are you expecting?

---

_Label `question` added by @charliermarsh on 2023-05-17 13:22_

---

_Comment by @jamesbraza on 2023-05-17 19:08_

Yeah thanks for asking for clarification!

The `or ValueError` is a valid use case, it's basically the fallback if no `exc_types` were passed.

<details>
<summary> Original Unclear Snippet's Extension</summary>

The below code (paired with the `foo` in the OP) might also help, if you want to play around with something.

```python
import pytest

def foo(msg: str, *exc_types: type[Exception]) -> None:
    try:
        raise ValueError(msg)
    except exc_types or ValueError as exc:
        print(exc)

foo("BOOM")  # Rely on `or ValueError` fallback
foo("BOOM2", ValueError)  # Manually passing
foo("BOOM3", ValueError, IndexError)  # Manually passing multiple

# Here we don't catch the ValueError as we don't include in `exc_types`
with pytest.raises(ValueError, match="BOOM4"):
    foo("BOOM4", KeyError, IndexError)
with pytest.raises(ValueError, match="BOOM5"):
    foo("BOOM5", KeyError)
```

</details>

---

Let's start anew with a better snippet:

```python
# I can contain 0+ Exceptions
# When I contain 1+ Exceptions and don't include ValueError,
# it overrides the default behavior
exc_types: tuple[type[Exception], ...] = ()

try:
    pass  # Some code
except exc_types or ValueError as exc:
    pass  # Some handling code
```


---

_Comment by @charliermarsh on 2023-05-18 00:54_

Ahhh I see! Thanks for sharing. That makes sense.

I see how this could be considered a false positive, but to be honest, I think this is the rule working as intended. The pattern that's being used here, even if it has the correct runtime behavior, is confusing -- as evidence, the two readers on this issue thought the intended behavior was different from the author's intended behavior. I'd argue that it's correct for the rule to flag this case, even if the runtime behavior is correct.

My humble suggestion would be to assign `exc_types or ValueError` to a temporary variable and match on that, which IMO is clearer.


---

_Closed by @charliermarsh on 2023-05-18 00:54_

---

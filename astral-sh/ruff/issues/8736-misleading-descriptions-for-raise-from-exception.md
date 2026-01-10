```yaml
number: 8736
title: Misleading descriptions for raise-from exception chaining rules (B904 and TRY200)
type: issue
state: closed
author: theemathas
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2023-11-17T04:13:00Z
updated_at: 2024-02-01T19:35:06Z
url: https://github.com/astral-sh/ruff/issues/8736
synced_at: 2026-01-10T11:09:51Z
```

# Misleading descriptions for raise-from exception chaining rules (B904 and TRY200)

---

_Issue opened by @theemathas on 2023-11-17 04:13_

In my understanding, the description pages for rules [B904](https://docs.astral.sh/ruff/rules/raise-without-from-inside-except/) and [TRY200](https://docs.astral.sh/ruff/rules/reraise-no-cause/) say (paraphrased): "Re-raising an exception (i.e., catching an exception and then raising a different exception) without specifying a cause with the `from` keyword will make the raised exception not be chained, which means we won't have the full stack trace information."

This is incorrect. As per the [official documentation](https://docs.python.org/3.12/tutorial/errors.html#exception-chaining), if an exception is re-raised without explicit chaining, it will be implicitly chained with the caught exception. Therefore, a re-raised exception without explicit chaining (which the linter rule says is bad) has similar behavior as explicit chaining (which the linter rule recommends). The main difference seems to be a slight difference in the wording of the stack trace. This behavior was not as well-documented in previous python versions, but it has been like this since at least [version 3.5.9](https://docs.python.org/3.5/reference/simple_stmts.html#the-raise-statement), if not older.

Side note: the fact that these two lint rules are duplicates is a known issue as per #2186 

---

_Comment by @zanieb on 2023-11-17 14:25_

For those who are unfamiliar, here are the various outputs:

Implicit chaining
```python
try:
    raise ValueError(1)
except Exception:
    raise ValueError(2)
```
```
Traceback (most recent call last):
  File "example.py", line 2, in <module>
    raise ValueError(1)
ValueError: 1

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "example.py", line 4, in <module>
    raise ValueError(2)
ValueError: 2
```

Explicit chaining

```python
try:
    raise ValueError(1)
except Exception as exc:
    raise ValueError(2) from exc
```
```
Traceback (most recent call last):
  File "example.py", line 19, in <module>
    raise ValueError(1)
ValueError: 1

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "example.py", line 21, in <module>
    raise ValueError(2) from exc
ValueError: 2
```

Explicit break of chain
```python
try:
    raise ValueError(1)
except Exception:
    raise ValueError(2) from None
```
```
Traceback (most recent call last):
  File "example.py", line 39, in <module>
    raise ValueError(2) from None
ValueError: 2
```

I agree the rule documentation is confusing as written. However "During handling of the above exception, another exception occurred" and "The above exception was the direct cause of the following exception" signal different things to a user. Intentionally raising an exception in an `except` block should always use `from` for clarity. It'd be great to update the documentation for these rules and add some examples


---

_Label `documentation` added by @zanieb on 2023-11-17 14:25_

---

_Added to milestone `v0.2.0` by @MichaReiser on 2024-01-19 14:21_

---

_Label `good first issue` added by @zanieb on 2024-01-30 03:08_

---

_Closed by @zanieb on 2024-02-01 19:35_

---

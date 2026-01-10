```yaml
number: 10356
title: "FBT003: Ignore by default django.db.models.Value"
type: issue
state: closed
author: 9128305
labels:
  - rule
  - configuration
assignees: []
created_at: 2024-03-12T10:03:40Z
updated_at: 2024-03-30T00:26:14Z
url: https://github.com/astral-sh/ruff/issues/10356
synced_at: 2026-01-10T11:09:52Z
```

# FBT003: Ignore by default django.db.models.Value

---

_Issue opened by @9128305 on 2024-03-12 10:03_

Hi, there is a common usage of `Value(False)` | `Value(True)`  in [Django](https://docs.djangoproject.com/en/5.0/ref/models/expressions/#value-expressions) projects. Ruff does not provide any configuration to extend the FBT003 default whitelist. So my request would be 1) Ignore `Value` by default like https://github.com/astral-sh/ruff/issues/6711 2) settings to extend FBT003 whitelist.
<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
```python
from django.db.models import Case, Q, Value, When
...
qs.annotate(
    is_foo_or_bar=Case(
        When(Q(is_foo=True) | Q(is_bar=True)),
        then=Value(True),  # noqa: FBT003
    ),
    default=Value(False),  # noqa: FBT003
)
```

---

_Label `rule` added by @zanieb on 2024-03-12 14:26_

---

_Label `configuration` added by @zanieb on 2024-03-12 14:26_

---

_Comment by @zanieb on 2024-03-12 14:27_

An allow-list for this rule does seems reasonable.

---

_Comment by @ngnpope on 2024-03-26 09:52_

@9128305 It should be possible to use `then=True` and `default=False` directly without wrapping with `Value()`, and that goes for most basic types, not just booleans. The exception is where strings are used, as `then="string"` would be interpreted as `then=F("string")`, i.e. a column name, and so `then=Value("string")` is required explicitly.

---

_Closed by @charliermarsh on 2024-03-30 00:26_

---

_Closed by @charliermarsh on 2024-03-30 00:26_

---

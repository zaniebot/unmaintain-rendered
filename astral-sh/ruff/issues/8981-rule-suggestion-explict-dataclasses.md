```yaml
number: 8981
title: "Rule Suggestion: explict-dataclasses"
type: issue
state: open
author: sbdchd
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-12-03T16:27:50Z
updated_at: 2024-01-08T03:43:57Z
url: https://github.com/astral-sh/ruff/issues/8981
synced_at: 2026-01-12T15:54:48Z
```

# Rule Suggestion: explict-dataclasses

---

_@sbdchd_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Basically I want dataclasses to default to `frozen=True` and `slots=True` with the option to opt out.

The rule would require passing the `frozen` and `slots` warts (if the python version is new enough to have `slots`)

Not sure about the naming of the rule


Or maybe the rule should be more general, like `dataclass-require-params` and the params are part of a config?

```python
from dataclasses import dataclass

# error, missing frozen and slot params
@dataclass
class Data: ...

# ok, unless on a python version with slots
@dataclass(frozen=True)
class Data: ...

# ok
@dataclass(frozen=True, slots=True)
class Data: ...

# ok
@dataclass(frozen=False, slots=False)
class Data: ...

# ok
@dataclass(frozen=True, slots=False)
class Data: ...
```


---

_Comment by @Avasam on 2023-12-03 18:46_

I've been thinking about wanting a way to enforce using slots on dataclasses. slots are the default with the `attr` package!
idk about frozen though.

---

_Label `rule` added by @charliermarsh on 2024-01-08 03:43_

---

_Label `needs-decision` added by @charliermarsh on 2024-01-08 03:43_

---

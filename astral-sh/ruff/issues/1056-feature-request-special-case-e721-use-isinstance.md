```yaml
number: 1056
title: "[feature-request] Special case E721 (use isinstance instead of type comparison) for `type(None)`"
type: issue
state: closed
author: smackesey
labels: []
assignees: []
created_at: 2022-12-05T12:40:54Z
updated_at: 2022-12-24T09:45:41Z
url: https://github.com/astral-sh/ruff/issues/1056
synced_at: 2026-01-10T12:05:22Z
```

# [feature-request] Special case E721 (use isinstance instead of type comparison) for `type(None)`

---

_Issue opened by @smackesey on 2022-12-05 12:40_

Comparison to `type(None)` occasionally comes up in our codebase:

```
if context.dagster_type.typing_type == type(None):
    return None
```

This raises a ruff/flake8 error: `E721 Do not compare types, use isinstance()`.

IMO this rule should not apply for comparisons to `type(None)`, since the "correct" form implied by the rule looks very odd:

```
if isinstance(None, context.dagster_type.typing_type):
    return None
```

I think this weirdness is unique to `type(None)`.


---

_Renamed from "Special case E721 for `type(None)`" to "Special case E721 (use isinstance instead of type comparison) for `type(None)`" by @smackesey on 2022-12-05 12:42_

---

_Label `enhancement` added by @charliermarsh on 2022-12-05 14:06_

---

_Renamed from "Special case E721 (use isinstance instead of type comparison) for `type(None)`" to "[feature-request] Special case E721 (use isinstance instead of type comparison) for `type(None)`" by @smackesey on 2022-12-05 14:29_

---

_Closed by @charliermarsh on 2022-12-24 09:45_

---

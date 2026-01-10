```yaml
number: 199
title: "Consider `__all__` for re-export convention"
type: issue
state: closed
author: dhruvmanila
labels: []
assignees: []
created_at: 2025-02-14T09:55:10Z
updated_at: 2025-05-07T16:12:44Z
url: https://github.com/astral-sh/ty/issues/199
synced_at: 2026-01-10T02:34:09Z
```

# Consider `__all__` for re-export convention

---

_Issue opened by @dhruvmanila on 2025-02-14 09:55_

### Description

Follow-up from https://github.com/astral-sh/ruff/issues/14099, the current implementation does not consider the values from `__all__` variable as seen in the [test document](https://github.com/astral-sh/ruff/blob/60b3ef2c985dba405db58a72bc6f22ff64c249fa/crates/red_knot_python_semantic/resources/mdtest/import/conventions.md#exported-using-__all__).

This requires support for `__all__` in general so ideally that should be done first and then a separate PR which considers that values in `__all__` for re-export conventions.

---

_Comment by @AlexWaygood on 2025-02-14 13:25_

This will also be required for the second stage of https://github.com/astral-sh/ruff/issues/14169 (the first stage that I'm implementing will pretend none of the semantics around `__all__` exist for wildcard imports)

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-05-01 13:48_

---

_Comment by @dhruvmanila on 2025-05-01 13:58_

(This is blocked on https://github.com/astral-sh/ty/issues/106.)

---

_Renamed from "[red-knot] Consider `__all__` for re-export convention" to "Consider `__all__` for re-export convention" by @MichaReiser on 2025-05-07 15:26_

---

_Added to milestone `alpha` by @MichaReiser on 2025-05-07 15:53_

---

_Closed by @dhruvmanila on 2025-05-07 16:12_

---

_Closed by @dhruvmanila on 2025-05-07 16:12_

---

```yaml
number: 19402
title: Reciprocal to rules like UP007 and UP045 for preserving native type annotation syntax
type: issue
state: open
author: smroels
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-07-17T15:00:14Z
updated_at: 2025-07-17T15:28:36Z
url: https://github.com/astral-sh/ruff/issues/19402
synced_at: 2026-01-12T15:54:56Z
```

# Reciprocal to rules like UP007 and UP045 for preserving native type annotation syntax

---

_@smroels_

### Summary

In a py3.9 context with `from __future__ import annotations`, we are using `keep-runtime-typing = true`
to avoid application of rules like UP007 and UP045 that suggest use of X | None or X | Y syntax instead of Optional[X] and Union{X|Y].
This is to avoid issues with Pydantic while nevertheless permitting us to address forward referencing issues.

It would be valuable to actually have reciprocal rules that flag the use of non-native py3.9 type annotation syntax with `from __future__ import annotations`. That is, not only would X | None not be suggested, it would get flagged as an issue if observed.  This would presumably be non-default behavior, but something that could be opted into.

---

_Comment by @ntBre on 2025-07-17 15:28_

This sounds like a "pydowngrade" rule set tracked in https://github.com/astral-sh/ruff/issues/2501. I'm not sure this is quite a duplicate, though. Wanting this behavior even in the presence of `from __future__ import annotations` seems a bit different but makes sense in this context I think.

---

_Label `rule` added by @ntBre on 2025-07-17 15:28_

---

_Label `needs-decision` added by @ntBre on 2025-07-17 15:28_

---

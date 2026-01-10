```yaml
number: 841
title: Stricter Ignore
type: issue
state: closed
author: carelvniekerk
labels:
  - question
  - suppression
assignees: []
created_at: 2025-07-17T13:34:20Z
updated_at: 2025-07-18T19:15:32Z
url: https://github.com/astral-sh/ty/issues/841
synced_at: 2026-01-10T02:07:36Z
```

# Stricter Ignore

---

_Issue opened by @carelvniekerk on 2025-07-17 13:34_

### Question

When using a comment to type:ignore [] mypy is strict and forces you to mention which type error to ignore making the comment transparent but ty currently is happy with just # type:ignore

### Version

_No response_

---

_Label `question` added by @carelvniekerk on 2025-07-17 13:34_

---

_Label `suppression` added by @AlexWaygood on 2025-07-17 13:38_

---

_Comment by @zanieb on 2025-07-17 13:53_

See https://docs.astral.sh/ty/suppression/

`# type: ignore` is a generic suppression comment so we prefer not to use tool specific codes there.

---

_Closed by @carljm on 2025-07-18 19:15_

---

```yaml
number: 121
title: Add more tests for inference of async comprehensions with invalid syntax
type: issue
state: open
author: ntBre
labels:
  - testing
  - internal
assignees: []
created_at: 2025-04-21T13:02:54Z
updated_at: 2025-11-18T16:10:23Z
url: https://github.com/astral-sh/ty/issues/121
synced_at: 2026-01-12T15:54:22Z
```

# Add more tests for inference of async comprehensions with invalid syntax

---

_@ntBre_

Similarly here, I don't think the intent here was for this to test our resilience to invalid syntax (the test case is titled "Basic" 😄), so I'd be inclined to do this to avoid the syntax error:

```suggestion
async def _():
    [reveal_type(x) async for x in AsyncIterable()]
```

although it probably _is_ a good idea for us to add a _separate_ test case that demonstrates that we still infer types correctly even if there is invalid syntax!

_Originally posted by @AlexWaygood in https://github.com/astral-sh/ruff/pull/17463#discussion_r2050872442_
            

---

_Renamed from "[red-knot] Add tests for type checking with invalid syntax" to "[red-knot] Add more tests for inference of async comprehensions with invalid syntax" by @AlexWaygood on 2025-04-21 13:10_

---

_Renamed from "[red-knot] Add more tests for inference of async comprehensions with invalid syntax" to "Add more tests for inference of async comprehensions with invalid syntax" by @MichaReiser on 2025-05-07 15:25_

---

_Label `testing` added by @AlexWaygood on 2025-05-10 18:06_

---

_Label `internal` added by @AlexWaygood on 2025-05-10 18:06_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 14:32_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---

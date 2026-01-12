```yaml
number: 18467
title: "[ty] Exclude members starting with `_abc_` from a protocol interface"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/protocol-members-abc
created_at: 2025-06-04T18:30:37Z
updated_at: 2025-06-04T19:34:10Z
url: https://github.com/astral-sh/ruff/pull/18467
synced_at: 2026-01-12T15:56:19Z
```

# [ty] Exclude members starting with `_abc_` from a protocol interface

---

_@AlexWaygood_

## Summary

As well as excluding a hardcoded set of special attributes, CPython at runtime also excludes any attributes or declarations starting with `_abc_` from the set of members that make up a protocol interface. I missed this in my initial implementation.

This is a bit of a CPython implementation detail, but I do think it's important that we try to model the runtime as best we can here. The closer we are to the runtime behaviour, the closer we come to sound behaviour when narrowing types from `isinstance()` checks against runtime-checkable protocols (for example)

## Test Plan

Extended an existing mdtest


---

_Review requested from @carljm by @AlexWaygood on 2025-06-04 18:30_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-04 18:30_

---

_Label `ty` added by @AlexWaygood on 2025-06-04 18:30_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-04 18:30_

---

_Comment by @github-actions[bot] on 2025-06-04 18:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-06-04 19:29_

Neither mypy nor pyright implement this, and I'm not sure how much the _static typing_ understanding of protocols should be built around implementation details of runtime-checkable protocols.

But I guess this is not likely to hurt compatibility, since such attributes should be unusual. So it seems fine to do this and re-evaluate if it causes any actual issues.

---

_Merged by @AlexWaygood on 2025-06-04 19:34_

---

_Closed by @AlexWaygood on 2025-06-04 19:34_

---

_Branch deleted on 2025-06-04 19:34_

---

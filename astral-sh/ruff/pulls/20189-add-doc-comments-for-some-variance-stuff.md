```yaml
number: 20189
title: add doc-comments for some variance stuff
type: pull_request
state: merged
author: ericmarkmartin
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: variance-comments
created_at: 2025-09-01T04:36:49Z
updated_at: 2025-09-05T22:46:21Z
url: https://github.com/astral-sh/ruff/pull/20189
synced_at: 2026-01-10T17:46:21Z
```

# add doc-comments for some variance stuff

---

_Pull request opened by @ericmarkmartin on 2025-09-01 04:36_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Add some comments to variance.rs machinery

## Test Plan

<!-- How was it tested? -->
N/a


---

_Review requested from @carljm by @ericmarkmartin on 2025-09-01 04:36_

---

_Review requested from @AlexWaygood by @ericmarkmartin on 2025-09-01 04:36_

---

_Review requested from @sharkdp by @ericmarkmartin on 2025-09-01 04:36_

---

_Review requested from @dcreager by @ericmarkmartin on 2025-09-01 04:36_

---

_Comment by @github-actions[bot] on 2025-09-01 04:38_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-01 04:41_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@dhruvmanila reviewed on 2025-09-01 08:26_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/variance.rs`:117 on 2025-09-01 08:26_

I think this is incomplete

---

_Label `ty` added by @ntBre on 2025-09-01 14:52_

---

_@carljm had review dismissed on 2025-09-02 18:14_

Looks good, thank you! Just need to complete the incomplete sentence

---

_Label `internal` added by @dhruvmanila on 2025-09-03 04:19_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/variance.rs`:121 on 2025-09-04 14:53_

```suggestion
    /// Creates a `VarianceInferable` that applies `polarity` (see
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/variance.rs`:126 on 2025-09-04 14:53_

```suggestion
    /// In some cases, we need to apply a polarity to the recursive call.
    /// You can do this with `ty.with_polarity(polarity).variance_of(typevar)`.
```

---

_@AlexWaygood reviewed on 2025-09-04 14:55_

thank you!

---

_Merged by @AlexWaygood on 2025-09-05 14:03_

---

_Closed by @AlexWaygood on 2025-09-05 14:03_

---

_Branch deleted on 2025-09-05 22:46_

---

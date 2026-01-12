```yaml
number: 21944
title: "[ty] Classify `cls` as class parameter"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/cls-semantic-tokens
created_at: 2025-12-12T12:31:47Z
updated_at: 2025-12-12T12:54:39Z
url: https://github.com/astral-sh/ruff/pull/21944
synced_at: 2026-01-12T15:57:37Z
```

# [ty] Classify `cls` as class parameter

---

_@MichaReiser_

## Summary

Classify usages of the `cls` parameter as `ClsParameter`.

## Test Plan

I updated the tests to have `cls` usages in the body and verified that they were incorrectly classified as `Parameter`. 

Now, with the changes in this PR, they're correctly classified as `ClsParameter`


---

_Review requested from @carljm by @MichaReiser on 2025-12-12 12:31_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-12 12:31_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-12 12:31_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-12 12:31_

---

_Label `server` added by @MichaReiser on 2025-12-12 12:31_

---

_Label `ty` added by @MichaReiser on 2025-12-12 12:31_

---

_Review requested from @ibraheemdev by @MichaReiser on 2025-12-12 12:31_

---

_Comment by @astral-sh-bot[bot] on 2025-12-12 12:34_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-12 12:35_

---

_Comment by @astral-sh-bot[bot] on 2025-12-12 12:35_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5133 diagnostics
+ Found 5132 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review request for @carljm removed by @MichaReiser on 2025-12-12 12:40_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-12-12 12:40_

---

_Review request for @dcreager removed by @MichaReiser on 2025-12-12 12:40_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-12-12 12:40_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-12-12 12:40_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-12 12:40_

---

_@BurntSushi approved on 2025-12-12 12:49_

---

_@dhruvmanila approved on 2025-12-12 12:53_

---

_Merged by @MichaReiser on 2025-12-12 12:54_

---

_Closed by @MichaReiser on 2025-12-12 12:54_

---

_Branch deleted on 2025-12-12 12:54_

---

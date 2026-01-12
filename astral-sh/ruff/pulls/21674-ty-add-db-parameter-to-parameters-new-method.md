```yaml
number: 21674
title: "[ty] Add `db` parameter to `Parameters::new` method"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dhruv/add-db-param
created_at: 2025-11-28T12:22:57Z
updated_at: 2025-11-28T12:29:59Z
url: https://github.com/astral-sh/ruff/pull/21674
synced_at: 2026-01-12T15:57:30Z
```

# [ty] Add `db` parameter to `Parameters::new` method

---

_@dhruvmanila_

## Summary

This PR adds a new `db` parameter to `Parameters::new` for https://github.com/astral-sh/ruff/pull/21445. This change creates a large diff so thought to split it out as it's just a mechanical change.

The `Parameters::new` method not only creates the `Parameters` but also analyses the parameters to check what kind it is. For `ParamSpec` support, it's going to require the `db` to check whether the annotated type is `ParamSpec` or not. For the current set of parameters that isn't required because it's only checking whether it's dynamic or not which doesn't require `db`.


---

_Label `internal` added by @dhruvmanila on 2025-11-28 12:22_

---

_Review requested from @carljm by @dhruvmanila on 2025-11-28 12:22_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-11-28 12:22_

---

_Label `ty` added by @dhruvmanila on 2025-11-28 12:22_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-11-28 12:22_

---

_Review requested from @dcreager by @dhruvmanila on 2025-11-28 12:22_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-11-28 12:22_

---

_Comment by @dhruvmanila on 2025-11-28 12:23_

This is a mechanical change, so I'm going to merge this without a review.

---

_Comment by @astral-sh-bot[bot] on 2025-11-28 12:26_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-28 12:28_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 500 diagnostics
+ Found 498 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Merged by @dhruvmanila on 2025-11-28 12:29_

---

_Closed by @dhruvmanila on 2025-11-28 12:29_

---

_Branch deleted on 2025-11-28 12:29_

---

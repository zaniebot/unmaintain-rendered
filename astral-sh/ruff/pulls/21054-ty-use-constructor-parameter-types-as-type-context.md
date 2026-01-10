```yaml
number: 21054
title: "[ty] Use constructor parameter types as type context"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/bidi-constructors
created_at: 2025-10-24T01:35:35Z
updated_at: 2025-10-24T20:14:19Z
url: https://github.com/astral-sh/ruff/pull/21054
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Use constructor parameter types as type context

---

_Pull request opened by @ibraheemdev on 2025-10-24 01:35_

## Summary

Resolves https://github.com/astral-sh/ty/issues/1408.

---

_Review requested from @carljm by @ibraheemdev on 2025-10-24 01:35_

---

_Label `ty` added by @ibraheemdev on 2025-10-24 01:35_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-10-24 01:35_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-10-24 01:35_

---

_Review requested from @dcreager by @ibraheemdev on 2025-10-24 01:35_

---

_Comment by @github-actions[bot] on 2025-10-24 01:39_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-24 01:41_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
-     struct fields = ~36MB
+     struct fields = ~38MB
-     memo metadata = ~108MB
+     memo metadata = ~113MB

```
</details>


---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-24 03:06_

---

_Comment by @github-actions[bot] on 2025-10-24 03:12_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://ibraheem-bidi-constructors.ecosystem-663.pages.dev/diff)** ([timing results](https://ibraheem-bidi-constructors.ecosystem-663.pages.dev/timing))


---

_Comment by @ibraheemdev on 2025-10-24 03:39_

The mypy-primer diff changes are due to a known issue with duplicated diagnostics (see https://github.com/astral-sh/ty/issues/1428), there are no actual new diagnostics (somewhat surprisingly).

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/bidirectional.md`:213 on 2025-10-24 13:00_

Is it important that these are separate `TypedDict` types? If so, can you call out why in a comment or in the markdown documentation?

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/bidirectional.md`:156 on 2025-10-24 13:02_

The examples in this section look like they're not exercising bidi typing to their fullest, since the expressions will already be inferred as `Literal["x"]` even without the type context. Would it be possible to update these so that the type context is different from what we'd normally infer for the expression? (Maybe a list literal and a more specific `list[something]` type context?) That might overlap with some of the tests elsewhere in the file, but I think it would make this section clearer.

(And ideally, it would be nice to be able to then show that the bidi-inferred type is different, by placing this alongside the same expression without a type context. But I think that would require `hover` annotations #20786)


---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:6057 on 2025-10-24 13:19_

We use `self_type` [down below](https://github.com/astral-sh/ruff/pull/21054/files?w=1#diff-8049ab5af787dba29daa389bbe2b691560c15461ef536f122b1beab112a4b48aR6089) when we bind the `cls` parameter when calling `__new__`. Can we use that here too?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:6073 on 2025-10-24 13:21_

ditto here

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:5980 on 2025-10-24 13:24_

Can you update the docs for this method to explain why this is a closure? (So that we can wait to infer types for arguments until we know the signatures of `__new__` and `__init__`, so that we can use their parameter annotations as type context for bidi inference)

---

_@dcreager approved on 2025-10-24 13:25_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-10-24 13:37_

---

_@ibraheemdev reviewed on 2025-10-24 20:09_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/bidirectional.md`:156 on 2025-10-24 20:09_

Sorry, that was an oversight, fixed. `hover` tests would nice :smile: 

---

_@ibraheemdev reviewed on 2025-10-24 20:10_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/bidirectional.md`:213 on 2025-10-24 20:10_

I realized this was actually incorrect because of structural subtyping. The idea was that both type contexts would be required for a valid inference, I updated the test to use a generic call instead.

---

_Merged by @ibraheemdev on 2025-10-24 20:14_

---

_Closed by @ibraheemdev on 2025-10-24 20:14_

---

_Branch deleted on 2025-10-24 20:14_

---

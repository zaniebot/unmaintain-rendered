```yaml
number: 19139
title: "[ty] detect cycles in Type::is_disjoint_from"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/disjoint-recurse
created_at: 2025-07-04T06:18:37Z
updated_at: 2025-07-04T13:31:46Z
url: https://github.com/astral-sh/ruff/pull/19139
synced_at: 2026-01-10T18:33:12Z
```

# [ty] detect cycles in Type::is_disjoint_from

---

_Pull request opened by @carljm on 2025-07-04 06:18_

## Summary

Detect and prevent cycles in `Type::is_disjoint_from`, which can now occur with recursive protocols.

## Test Plan

Added mdtest.


---

_Review requested from @AlexWaygood by @carljm on 2025-07-04 06:18_

---

_Label `ty` added by @carljm on 2025-07-04 06:18_

---

_Review requested from @sharkdp by @carljm on 2025-07-04 06:18_

---

_Review requested from @dcreager by @carljm on 2025-07-04 06:18_

---

_Comment by @github-actions[bot] on 2025-07-04 06:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~80MB

pydantic (https://github.com/pydantic/pydantic)
- TOTAL MEMORY USAGE: ~142MB
+ TOTAL MEMORY USAGE: ~156MB

mkosi (https://github.com/systemd/mkosi)
-     memo fields = ~106MB
+     memo fields = ~97MB

tornado (https://github.com/tornadoweb/tornado)
- TOTAL MEMORY USAGE: ~171MB
+ TOTAL MEMORY USAGE: ~156MB

psycopg (https://github.com/psycopg/psycopg)
- TOTAL MEMORY USAGE: ~228MB
+ TOTAL MEMORY USAGE: ~207MB

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- TOTAL MEMORY USAGE: ~276MB
+ TOTAL MEMORY USAGE: ~304MB

paasta (https://github.com/yelp/paasta)
- TOTAL MEMORY USAGE: ~189MB
+ TOTAL MEMORY USAGE: ~207MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~304MB
+ TOTAL MEMORY USAGE: ~276MB

scipy (https://github.com/scipy/scipy)
- TOTAL MEMORY USAGE: ~1271MB
+ TOTAL MEMORY USAGE: ~1156MB

```
</details>


---

_@sharkdp approved on 2025-07-04 06:40_

Nice!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/cyclic.rs`:16 on 2025-07-04 10:42_

Implementing `Default` for a type alias like this means that you can do this -- but falling back to `Any` might not be what you want for an arbitrary `CycleDetector` instance; it only really makes sense for type transformers:

```rs
fn foo() {
    let x = CycleDetector::<Type, Type>::default();
}
```

Would it be better to implement `TypeTransformer` as a newtype rather than a type alias and implement `Deref`/`DerefMut`/`Default` for the newtype?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/cyclic.rs`:21 on 2025-07-04 10:44_

you can provide the bounds on the `impl` block without specifying them for the struct itself -- it's less noisy in terms of the syntax and also makes the struct definition more flexible (it would allow us to add `impl` blocks for instances of `CycleDetector` where `T: !Hash`, for example, if there was ever a need for those instances).

```suggestion
pub(crate) struct CycleDetector<T, R> {
```

---

_@AlexWaygood approved on 2025-07-04 10:45_

---

_@carljm reviewed on 2025-07-04 13:21_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/cyclic.rs`:16 on 2025-07-04 13:21_

A CycleDetector that visits single types and returns a single type _is_ inherently a type transformer, simply by filling in the type parameters that way. So I think a type alias is better here (and a lot simpler).

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/cyclic.rs`:16 on 2025-07-04 13:23_

OK!

---

_@AlexWaygood reviewed on 2025-07-04 13:23_

---

_@carljm reviewed on 2025-07-04 13:23_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/cyclic.rs`:21 on 2025-07-04 13:23_

Since a cycle detector contains a hashset of T, I don't think there's a realistic flexibility argument here (if we had a cycle detector with `T: !Hash` it could never populate `seen` at all.)

But I don't mind removing these for brevity anyway. 

---

_Merged by @carljm on 2025-07-04 13:31_

---

_Closed by @carljm on 2025-07-04 13:31_

---

_Branch deleted on 2025-07-04 13:31_

---

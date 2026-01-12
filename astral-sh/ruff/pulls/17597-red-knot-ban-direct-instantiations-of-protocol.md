```yaml
number: 17597
title: "[red-knot] Ban direct instantiations of `Protocol` classes"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/ban-protocol-instantiation
created_at: 2025-04-23T23:27:03Z
updated_at: 2025-04-24T09:32:38Z
url: https://github.com/astral-sh/ruff/pull/17597
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Ban direct instantiations of `Protocol` classes

---

_@AlexWaygood_

## Summary

Protocol classes are abstract and thus cannot be directly instantiated (although concrete subclasses of protocols can be!). This PR adds logic to detect attempts to instantiate them, and report such attempts as diagnostics.

I also factored out some common code from `diagnostics.rs` into `class.rs`.

## Test Plan

Existing mdtests updated, and snapshots added.

Local screenshot to show what it looks like in the terminal:

![image](https://github.com/user-attachments/assets/a21bfdba-df47-4d4c-a6f2-50e0e8158354)



---

_Label `red-knot` added by @AlexWaygood on 2025-04-23 23:27_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-23 23:27_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-23 23:27_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-23 23:27_

---

_Comment by @github-actions[bot] on 2025-04-23 23:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:1734 on 2025-04-23 23:39_

```suggestion
    /// ```ignore
```

---

_@AlexWaygood reviewed on 2025-04-23 23:39_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:1417 on 2025-04-24 00:35_

Are you sure that it's clearest to use the term "abstract" for this? I can see why it's not unreasonable, but both mypy and pyright just say "Cannot instantiate protocol class 'MyProtocol'". It seems like introducing the term "abstract" might be unnecessary complexity, and evoke only-sort-of-related things like ABCMeta? It just forces us to explain below that inheriting Protocol makes a class "implicitly abstract", which seems unnecessary when we could just as well say "it inherits Protocol, making it a protocol class, and protocol classes can't be instantiated"

---

_@carljm approved on 2025-04-24 00:36_

Looks great!

---

_@AlexWaygood reviewed on 2025-04-24 09:15_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:1417 on 2025-04-24 09:15_

Hmm, fair point. In my mental model, protocol classes are banned from being instantiated for exactly the same reason that ABCs with abstract methods are banned from being instantiated: both classes represent abstract interfaces to some extent (with the former really just being an extension of the latter in terms of how far you take that idea). But using the term "abstract" probably doesn't help our users, who won't necessary have the same mental model!

---

_Merged by @AlexWaygood on 2025-04-24 09:31_

---

_Closed by @AlexWaygood on 2025-04-24 09:31_

---

_Branch deleted on 2025-04-24 09:31_

---

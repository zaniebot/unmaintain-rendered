```yaml
number: 20217
title: "[ty] Add functions for revealing assignability/subtyping constraints"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/reveal-when-constraints
created_at: 2025-09-03T18:34:14Z
updated_at: 2025-09-03T20:44:37Z
url: https://github.com/astral-sh/ruff/pull/20217
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Add functions for revealing assignability/subtyping constraints

---

_Pull request opened by @dcreager on 2025-09-03 18:34_

This PR adds two new `ty_extensions` functions, `reveal_when_assignable_to` and `reveal_when_subtype_of`. These are closely related to the existing `is_assignable_to` and `is_subtype_of`, but instead of returning when the property (always) holds, it produces a diagnostic that describes _when_ the property holds. (This will let us construct mdtests that print out constraints that are not always true or always false — though we don't currently have any instances of those.)

I did not replace _every_ occurrence of the `is_property` variants in the mdtest suite, instead focusing on the generics-related tests where it will be important to see the full detail of the constraint sets.

As part of this, I also updated the mdtest harness to accept the shorter `# revealed:` assertion format for more than just `reveal_type`, and updated the existing uses of `reveal_protocol_interface` to take advantage of this.

---

_Label `ty` added by @dcreager on 2025-09-03 18:34_

---

_Comment by @github-actions[bot] on 2025-09-03 18:36_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Review comment by @dcreager on `crates/ty_test/src/matcher.rs`:309 on 2025-09-03 18:42_

This makes sure that we have a consistent type in both debug and release modes, so that we can compare with `expected_type` down below

---

_@dcreager reviewed on 2025-09-03 18:42_

---

_Marked ready for review by @dcreager on 2025-09-03 18:42_

---

_Review requested from @carljm by @dcreager on 2025-09-03 18:42_

---

_Review requested from @AlexWaygood by @dcreager on 2025-09-03 18:42_

---

_Review requested from @sharkdp by @dcreager on 2025-09-03 18:42_

---

_Review requested from @MichaReiser by @dcreager on 2025-09-03 18:42_

---

_Comment by @github-actions[bot] on 2025-09-03 18:43_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `internal` added by @AlexWaygood on 2025-09-03 19:08_

---

_Review comment by @sharkdp on `crates/ty_vendored/ty_extensions/ty_extensions.pyi`:58 on 2025-09-03 19:12_

Source and target seem to be swapped here? (and also above, in the existing function)

It's `is_assignable_to(source, target)` or `is_assignable_to(from, to)` or `is_assignable_to(rhs_of_assignment, lhs_of_assignment)`. https://play.ty.dev/47621237-8fec-475e-b668-56905b91252b

---

_@sharkdp reviewed on 2025-09-03 19:12_

---

_@sharkdp approved on 2025-09-03 19:15_

Cool, thank you!

---

_@dcreager reviewed on 2025-09-03 20:32_

---

_Review comment by @dcreager on `crates/ty_vendored/ty_extensions/ty_extensions.pyi`:58 on 2025-09-03 20:32_

Yes you're right!  One of the other functions used the less opinionated `type_a` and `type_b`, so I standardized all of the signatures on that. (And cleaned up a couple that were using `type` as a parameter name, which even if it's not a syntax error is confusing as it conflicts with the new type alias keyword)

---

_Merged by @dcreager on 2025-09-03 20:44_

---

_Closed by @dcreager on 2025-09-03 20:44_

---

_Branch deleted on 2025-09-03 20:44_

---

```yaml
number: 13170
title: "[red-knot] introduce basic parameter annotation expression inference"
type: pull_request
state: closed
author: chriskrycho
labels: []
assignees: []
draft: true
base: main
head: rk-basic-param-annotations
created_at: 2024-08-30T22:39:23Z
updated_at: 2024-09-04T21:57:14Z
url: https://github.com/astral-sh/ruff/pull/13170
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] introduce basic parameter annotation expression inference

---

_@chriskrycho_

## Summary

Function definitions now have the types for parameters lazily available, just like return types. This only supports basic explicit type annotations, no special forms, in line with the rest of the existing annotation inference.

## Test Plan

Tests that normal function declaration statements have the type annotations correctly attached to their parameters.

---

_@chriskrycho reviewed on 2024-08-30 22:42_

This presently results in a failure on `types::infer::tests::ellipsis_type`. The expectation is that the type inferred in `x = ...` should be `Unknown | Literal[EllipsisType]` (as it was prior to now), but it is resolving as `Unknown | Literal[EllipsisType] | Literal[ellipsis]`. It is unclear why!

---

_@carljm reviewed on 2024-08-30 22:49_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:381 on 2024-08-30 22:49_

I think this should just be `self.node(db)` -- we don't have to avoid calling `self.definition(db)` twice.

---

_@chriskrycho reviewed on 2024-08-30 22:56_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types.rs`:381 on 2024-08-30 22:56_

Ah, good point; then we can simplify the call below to just be `self.definition(db)` inline as well.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:389 on 2024-08-30 22:57_

Let's give this a doc comment to clarify its expected semantics.

I think the current semantics here are a little odd, given the name `params`, since it will iterate over all parameter annotations, skipping un-annotated parameters, and with no way to associate the iterated types with parameter names. I think at the very least this should probably iterate all parameters, and return pairs of `(param-node, annotated-type-or-unknown)`? Though I'm not sure that iterating all params is the right model here at all, given how this will be used. We may want a full-fledged `Signature` struct, that carries full information about all parameters and their types.

...I guess what I'm saying is maybe we'll need to design this API along with building out our initial use of it in checking call validity, to see what shape it should have?

---

_@carljm reviewed on 2024-08-30 22:57_

---

_@carljm reviewed on 2024-08-30 23:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:465 on 2024-08-30 23:01_

It's not correct to defer _all_ of parameter inference, only annotations (not defaults). So I think we can't just move `infer_parameters` in here, it'll need to get split up. We could just have `infer_parameter_defaults` and `infer_parameter_annotations`. Or we can call `infer_parameters` in both non-deferred and deferred cases, and do the `is_stub` checking down in `infer_parameter_with_default` and `infer_parameter`. Either could be OK, not sure which will work out nicer.

---

_Closed by @chriskrycho on 2024-09-04 21:57_

---

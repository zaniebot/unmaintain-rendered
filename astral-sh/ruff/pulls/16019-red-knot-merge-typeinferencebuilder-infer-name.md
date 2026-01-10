```yaml
number: 16019
title: "[red-knot] Merge `TypeInferenceBuilder::infer_name_load` and `TypeInferenceBuilder::lookup_name`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/simplify-lookup-name
created_at: 2025-02-07T11:34:38Z
updated_at: 2025-02-08T19:42:16Z
url: https://github.com/astral-sh/ruff/pull/16019
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Merge `TypeInferenceBuilder::infer_name_load` and `TypeInferenceBuilder::lookup_name`

---

_Pull request opened by @AlexWaygood on 2025-02-07 11:34_

## Summary

No functional change here; this is another simplification split out from my outcome-refactor branch to reduce the diff there. This merges `TypeInferenceBuilder::infer_name_load` and `TypeInferenceBuilder::lookup_name`. This removes the need to have extensive doc-comments about the purpose of `TypeInferenceBuilder::lookup_name`, since the method only makes sense when called from the specific context of `TypeInferenceBuilder::infer_name_load`.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2025-02-07 11:34_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-07 11:34_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-07 11:34_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-07 11:34_

---

_@AlexWaygood reviewed on 2025-02-07 11:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3302 on 2025-02-07 11:35_

@carljm, I'd appreciate it if you could confirm that my understanding here is correct...!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3302 on 2025-02-07 17:32_

My only quibble is that I think it'd be clearer to say "where we currently are in control flow" rather than "where we currently are in the AST", but this looks right.

FWIW I'm not sure if this is ideal, though -- I think the split between what we do in `infer_name_load` and `lookup_name` doesn't make a ton of sense, and the code might be clearer if we moved all the logic into `lookup_name` such that it is called for every name load? But not 100% sure without trying it.

---

_@carljm approved on 2025-02-07 17:36_

---

_Renamed from "[red-knot] Further simplify `TypeInferenceBuilder::lookup_name`" to "[red-knot] Merge `TypeInferenceBuilder::infer_name_load` and `TypeInferenceBuilder::lookup_name`" by @AlexWaygood on 2025-02-08 12:41_

---

_@AlexWaygood reviewed on 2025-02-08 12:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3302 on 2025-02-08 12:42_

> FWIW I'm not sure if this is ideal, though -- I think the split between what we do in `infer_name_load` and `lookup_name` doesn't make a ton of sense, and the code might be clearer if we moved all the logic into `lookup_name` such that it is called for every name load? But not 100% sure without trying it.

Excellent idea. I tried it, and I think it helps quite a lot!

---

_Review requested from @carljm by @AlexWaygood on 2025-02-08 12:42_

---

_@carljm approved on 2025-02-08 19:41_

Love it!

---

_Merged by @AlexWaygood on 2025-02-08 19:42_

---

_Closed by @AlexWaygood on 2025-02-08 19:42_

---

_Branch deleted on 2025-02-08 19:42_

---

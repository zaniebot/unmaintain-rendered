```yaml
number: 13320
title: "[red-knot] Fix type inference for `except*` definitions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/async-except
created_at: 2024-09-10T22:09:00Z
updated_at: 2024-09-11T19:05:42Z
url: https://github.com/astral-sh/ruff/pull/13320
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] Fix type inference for `except*` definitions

---

_Pull request opened by @AlexWaygood on 2024-09-10 22:09_

Once again, async Python requires different inference rules to sync Python!

A new `DefinitionNodeRef::ExceptStarHandler` variant is introduced, so that we can differentiate between except-handler definitions in `StmtTry { is_star: true }` nodes and those in `StmtTry { is_star: false }` nodes. The logic for adding these definitions is also moved in `builder.rs` so that we can know whether the `try` block is catching exceptions or exception groups. (We'll need to move the logic there anyway when adding control flow for these definitions.)

Lots of TODOs here, but I _think_ inferring `Instance(BaseExceptionGroup)` is better than just inferring `Unknown` for these definitions.

The PR is stacked on top of #13319

---

_Label `red-knot` added by @AlexWaygood on 2024-09-10 22:09_

---

_Review requested from @carljm by @AlexWaygood on 2024-09-10 22:09_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-09-10 22:09_

---

_Comment by @github-actions[bot] on 2024-09-10 22:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-09-10 22:31_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:704 on 2024-09-10 22:31_

I don't mind having different nodes, but I am interested in understanding your motivation for two variants over a boolean flag. 

---

_@AlexWaygood reviewed on 2024-09-10 22:34_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:704 on 2024-09-10 22:34_

There's not much in it. Carl and I thought it seemed marginally simpler to have two different variants rather than creating a dedicated struct with an `is_star: bool` flag inside the `ExceptHandler` variant. It also reinforces semantically that these are different kinds of definitions that create quite different types. But I'm very happy to do a dedicated struct if you prefer it!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:433 on 2024-09-10 22:39_

I think if we are going to just collapse back down to a boolean flag on the inference end, then we should just pass it through the definition as a boolean flag also.

(I know this is opposite to what I was leaning toward before, but that was on the assumption that the inference end would be more separated. But it's all the same node, so I think that was wrong.)

---

_@carljm reviewed on 2024-09-10 22:39_

---

_@AlexWaygood reviewed on 2024-09-10 22:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:433 on 2024-09-10 22:42_

Yeah, it worked out differently to how I was expecting as well... I'll switch it to be just a boolean flag!

---

_@MichaReiser reviewed on 2024-09-10 22:51_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:704 on 2024-09-10 22:51_

Nah that's fine. I was just interested in the reasoning and I'm sure if it's for the best if that was the conclusion you two came to.

---

_Review requested from @carljm by @AlexWaygood on 2024-09-11 13:50_

---

_@MichaReiser approved on 2024-09-11 19:04_

---

_@carljm approved on 2024-09-11 19:05_

🎉 

---

_Merged by @AlexWaygood on 2024-09-11 19:05_

---

_Closed by @AlexWaygood on 2024-09-11 19:05_

---

_Branch deleted on 2024-09-11 19:05_

---

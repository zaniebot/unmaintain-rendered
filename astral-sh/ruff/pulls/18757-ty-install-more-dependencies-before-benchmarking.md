```yaml
number: 18757
title: "[ty] Install more dependencies before benchmarking hydra-zen"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
base: main
head: alex/more-hydra-deps
created_at: 2025-06-18T14:54:39Z
updated_at: 2025-06-18T15:14:02Z
url: https://github.com/astral-sh/ruff/pull/18757
synced_at: 2026-01-12T15:56:25Z
```

# [ty] Install more dependencies before benchmarking hydra-zen

---

_@AlexWaygood_

## Summary

This makes the number of `unresolved-import` diagnostics on hydra-zen go down from 136 to 16. The overall number of diagnostics goes down from 703 to 588.

That seems worth the cost of installing them considering that hydra-zen would likely have these imports installed if they were running ty in CI, and we want the benchmark to accurately mimic the experience users would have if they ran ty on these projects in CI. (In CI, gathering suggestions for an `unresolved-import` diagnostic would usually be a very cold path, for example, because there would usually be a very small number of diagnostics like this.)

## Test Plan

I:
1. Cloned [hydra-zen](https://github.com/mit-ll-responsible-ai/hydra-zen)
2. Created a virtual environment at `<hydra-zen repo root>/.venv`
3. Ran `uv pip install pydantic beartype hydra-core
4. Ran `path/to/ty/binary check` and observed that there were 136 `unresolved-import diagnostics
5. Ran `uv pip install pytest hypothesis`
6. Ran `path/to/ty/binary check` and observed that there were now only 16 `unresolved-import diagnostics


---

_Review requested from @MichaReiser by @AlexWaygood on 2025-06-18 14:54_

---

_Label `testing` added by @AlexWaygood on 2025-06-18 14:54_

---

_Label `ty` added by @AlexWaygood on 2025-06-18 14:54_

---

_Comment by @MichaReiser on 2025-06-18 14:56_

You might want to update mypy primer too 😅 

Hydra is already one of the slowest to install. I'm curious by how much performance degrades due to adding the new dependencies.

---

_Comment by @AlexWaygood on 2025-06-18 14:57_

> You might want to update mypy primer too 😅

I will!

---

_Comment by @github-actions[bot] on 2025-06-18 15:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/ty.rs`:437 on 2025-06-18 15:06_

I can't comment but you might want to lower the number of the max expected diagnostics on line 441. 

That makes me realise that we may want to enable all rules, including possibly-* rules

---

_@MichaReiser approved on 2025-06-18 15:06_

---

_@AlexWaygood reviewed on 2025-06-18 15:07_

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/ty.rs`:437 on 2025-06-18 15:07_

> That makes me realise that we may want to enable all rules, including possibly-* rules

that makes sense but I'll leave it out of this PR

---

_@MichaReiser reviewed on 2025-06-18 15:09_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/ty.rs`:437 on 2025-06-18 15:09_

Oh yeah, that was more a note for myself

---

_Comment by @AlexWaygood on 2025-06-18 15:13_

I realised that the benchmarks only run ty on the `src/` directory, and that pytest/hypothesis are not needed for checking those directories. So this PR isn't necessary.

---

_Closed by @AlexWaygood on 2025-06-18 15:13_

---

_Branch deleted on 2025-06-18 15:14_

---

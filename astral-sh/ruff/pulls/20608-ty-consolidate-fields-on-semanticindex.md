```yaml
number: 20608
title: "[ty] Consolidate fields on `SemanticIndex`"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - performance
  - ty
assignees: []
base: main
head: alex/semantic-index-memory
created_at: 2025-09-28T15:21:23Z
updated_at: 2025-10-02T06:54:33Z
url: https://github.com/astral-sh/ruff/pull/20608
synced_at: 2026-01-12T15:57:06Z
```

# [ty] Consolidate fields on `SemanticIndex`

---

_@AlexWaygood_

## Summary

This PR consolidates various fields on the `SemanticIndex` struct so that instead of having five `IndexVec` fields that are indexed by `FileScopeId`, we only have one.

The original motivation behind this PR was to see if this change would reduce memory usage at all. I'm unable to find any evidence that this makes any significant difference to ty's memory usage -- and this probably makes sense, because we only create one `SemanticIndex` struct for each file. However, I think this PR is probably worth doing anyway, as it looks like it leads to a 3% speedup on our `colour` benchmark, and speedups of 1-2% on all our other walltime benchmarks: https://codspeed.io/astral-sh/ruff/branches/alex%2Fsemantic-index-memory?runnerMode=WallTime. These reported speedups have been consistent across multiple runs on this PR, so I think they're real. I assume this PR speeds things up because we're now doing less work in `SemanticIndexBuilder::new()` (rather than having to allocate five `IndexVec`s, we now only need three for each builder), and `SemanticIndexBuilder::build()` (rather than having five different `.shrink_to_fit()` calls and three different `.collect()` calls, we now only need one of each). Conceptually, it's also quite nice to have all per-scope information consolidated in the `Scope` struct as well.

Best reviewed commit by commit.

## Test Plan

Existing tests


---

_Label `ty` added by @AlexWaygood on 2025-09-28 15:21_

---

_Renamed from "[ty] Reduce memory usage of `SemanticIndex`'" to "[ty] Reduce memory usage of `SemanticIndex`" by @AlexWaygood on 2025-09-28 15:21_

---

_Comment by @github-actions[bot] on 2025-09-28 15:23_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-28 15:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 52 diagnostics
+ Found 53 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Renamed from "[ty] Reduce memory usage of `SemanticIndex`" to "[ty] Consolidate fields on `SemanticIndex`" by @AlexWaygood on 2025-09-28 15:46_

---

_Label `performance` added by @AlexWaygood on 2025-09-28 17:20_

---

_Marked ready for review by @AlexWaygood on 2025-09-28 17:24_

---

_Review requested from @carljm by @AlexWaygood on 2025-09-28 17:24_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-28 17:24_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-28 17:24_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/semantic_index/builder.rs`:117 on 2025-09-29 14:26_

Is there something that prevents us from moving `place_tables` and `use_def_maps` into the new `ScopeBuilder` as well? (If there is, that deserves a comment here calling out why)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/semantic_index/scope.rs`:138 on 2025-09-29 14:38_

optional nit: I think it's a minor code smell that callers now have to import `ScopeLike` to get access to some of the methods of `Scope`. You can get around that by keeping the definitions of `kind` and `parent` here in addition to the new definitions down in `ScopeLike`.

And you can change the definitions below to 

```rust
fn kind(&self) -> ScopeKind {
    Scope::kind(self)
}
```

to avoid duplicating the method body and without introducing an infinite loop

---

_@dcreager approved on 2025-09-29 14:38_

---

_@AlexWaygood reviewed on 2025-09-29 14:41_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/builder.rs`:117 on 2025-09-29 14:41_

It's _basically_ just that the borrow checker gets quite angry if you try to borrow the `scopes` `IndexVec` mutably and immutably at the same time -- having separate `IndexVec`s for `place_tables` and `use_def_maps` works around this, because both `place_tables` and `use_def_maps` frequently need to be borrowed mutably from `builder.rs`. There are ways of working around the borrow checker complaints if we store the use-def and place-table state on `ScopeBuilder` too, but from what I could tell they all undid the performance benefits of this change.

And yes, this is _definitely_ worth a comment -- thank you! I'll add one.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/scope.rs`:138 on 2025-09-29 14:43_

Yes, I was in two minds about this! I guess `ScopeLike` is really an implementation detail of the `semantic_index` submodule, so it's probably better to make it private and expose these methods publicly on `Scope`, as you suggest

---

_@AlexWaygood reviewed on 2025-09-29 14:43_

---

_Comment by @AlexWaygood on 2025-09-29 16:52_

Hmm, well, after the latest rebase Codspeed is no longer claiming any speedup as a result of this, even though it pretty consistently said that there was one previously. So this PR now just seems to produce more code, for maybe not that much benefit :/ There is still this as a motivation for the change:

> Conceptually, it's also quite nice to have all per-scope information consolidated in the `Scope` struct as well.

But I don't know if it's a worthwhile change just on that basis.

---

_Comment by @MichaReiser on 2025-09-29 17:51_

My only worry with this is that it might get more challenging to satisfy the borrow checker in the future (similar to place table). But I don't feel stronlgy about this

---

_Comment by @ibraheemdev on 2025-09-30 21:10_

Did you run the detailed memory report (`TY_MEMORY_REPORT=full`) locally to see the diff? I'd be interested if this is a net-positive or net-negative to memory usage (even if minimal), because the struct of arrays representation should theoretically minimize padding, though I'm not sure if it's affected in this case because we already have an array of `Scope`s.

It can also be more cache-efficient when iterating over a single array, but the opposite can also be true if zipping over multiple.

---

_Comment by @AlexWaygood on 2025-09-30 21:15_

> Did you run the detailed memory report (`TY_MEMORY_REPORT=full`) locally to see the diff?

I ran with `TY_MEMORY_REPORT=mypy_primer` on a large project locally and couldn't see any difference in the topline numbers relative to `main`. I can run again with `TY_MEMORY_REPORT=full`

---

_Comment by @ibraheemdev on 2025-09-30 21:20_

The mypy-primer rounds the numbers to avoid excessive diffs (though maybe we should change that). Also make sure to run with `TY_MAX_PARALLELISM=1` for deterministic numbers locally.

---

_Comment by @dcreager on 2025-10-02 00:34_

If we're not seeing clear evidence of memory or CPU improvements, I would lean towards tabling this.

---

_Closed by @AlexWaygood on 2025-10-02 06:53_

---

_Comment by @AlexWaygood on 2025-10-02 06:54_

Yeah, I think we've probably spent enough time looking at this now given our priorities and deadlines :-)

Thanks all!

---

_Branch deleted on 2025-10-02 06:54_

---

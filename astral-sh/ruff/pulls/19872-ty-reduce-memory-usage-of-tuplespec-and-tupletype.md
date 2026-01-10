```yaml
number: 19872
title: "[ty] Reduce memory usage of `TupleSpec` and `TupleType`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/smaller-tuples
created_at: 2025-08-11T21:44:09Z
updated_at: 2025-08-12T11:51:18Z
url: https://github.com/astral-sh/ruff/pull/19872
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Reduce memory usage of `TupleSpec` and `TupleType`

---

_Pull request opened by @AlexWaygood on 2025-08-11 21:44_

## Summary

Fixes https://github.com/astral-sh/ty/issues/956. Use boxed slices rather than Vecs where possible for types interned in the salsa database, to save on memory.

## Test Plan

Existing tests


---

_Label `ty` added by @AlexWaygood on 2025-08-11 21:45_

---

_Comment by @github-actions[bot] on 2025-08-11 21:46_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-11 21:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
sphinx (https://github.com/sphinx-doc/sphinx)
-     struct fields = ~17MB
+     struct fields = ~16MB

```
</details>


---

_Comment by @AlexWaygood on 2025-08-11 22:15_

Not sure if this is worth it — CI reports no significant memory-usage reductions in the [memory.txt projects](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/primer/memory.txt), and codspeed reports 1% regressions on several benchmarks.

Tomorrow I'll try getting some memory-usage reports for projects that we know have some complicated tuple types in them, such as colour-science and static-frame. Opening up for review now anyway, to see what folks think of the approach!

---

_Marked ready for review by @AlexWaygood on 2025-08-11 22:16_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-11 22:16_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-11 22:16_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-11 22:16_

---

_Comment by @MichaReiser on 2025-08-12 06:12_

You probably want to wait for https://github.com/astral-sh/ruff/pull/19790 (or rebase on top) to get a full picture. The memory report today only accounts for queries (heap and stack size) and the stack size of tracked struct, but it doesn't account for the heap size of tracked structs. That means, the only memory reduction you'd see is from reducing the stack memory from  48 + `stack_size::<T>` to 32 + `stack_size::<T>`. That on its own might still be significant but only if we create a large number of tuple specs (and intern them)

---

_Comment by @MichaReiser on 2025-08-12 06:21_

This is the full memory report comparison from running on a large project. You can see how the tuple type's stack size has become much smaller. It's just not significant in overall terms because there aren't many enough (or put differently, some other queries use up so much space that the tuple type improvement is negligible). I suggest re-running the analysis with #19790 

https://www.diffchecker.com/ZkHpFHCN/

Edit: The reason why we have different counts for some ingredients is because ty still panics when analysing some files and that leads to non-deterministic results.

---

_Comment by @MichaReiser on 2025-08-12 11:23_

I just ran main with the salsa struct memory usage information and the total memory usage for tuples is only:

```
`TupleType`                                        metadata=6.65MB   fields=13.88MB  count=118827
```

So i don't think this will lead to a very significant improvement

---

_Comment by @AlexWaygood on 2025-08-12 11:45_

I tried running memory reports on colour-science and static-frame, which were codebases I thought might be interesting because we know operations on tuple types are quite hot when typechecking those codebases. The top-level memory usage for both those codebases is the same on `main` and with this PR. The memory taken up by `TupleType` does go down, but it's a small fraction of the overall memory usage

<table>
<tr>
 <td>Project
 <td>TupleType memory usage (main)
 <td>TupleType memory usage (this PR)
 <td>Total memory usage (main and this PR)
<tr>
 <td>static-frame
 <td>metadata: 0.98MB<br>fields: 1.91MB
 <td>metadata: 0.98MB<br>fields: 1.58MB
 <td>348MB
<tr>
 <td>colour
 <td>metadata: 0.12MB<br>fields: 0.44MB
 <td>metadata: 0.12MB<br>fields: 0.39MB
 <td>298MB
</table>

(All measurements done by executing `TY_MEMORY_REPORT=full cargo run --release --manifest-path ../ruff/Cargo.toml -p ty check --quiet`)

That does seem like a pretty decent reduction in memory usage on tuples in `static-frame`, even if it's a small slice of the overall pie. Using boxed slices is also consistent with what we do elsewhere for our Salsa-interned structs, and is more expressive of the fact that types are immutable once they're stored in the Salsa cache. I think the main question is whether this is worth the increase in code complexity -- I could go either way on that.

Performance is in the noise -- some benchmarks are showing tiny speedups, other ones tiny slowdowns.

---

_@MichaReiser approved on 2025-08-12 11:49_

I think the immutability of `TupleSpec` is a nice side effect that I value more than the tiny memory improvement :)

---

_Comment by @MichaReiser on 2025-08-12 11:49_

Eh, sphinx has a memory reduction 😆 

---

_Merged by @AlexWaygood on 2025-08-12 11:51_

---

_Closed by @AlexWaygood on 2025-08-12 11:51_

---

_Branch deleted on 2025-08-12 11:51_

---

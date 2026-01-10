```yaml
number: 14116
title: Use accumulator for diagnostics
type: pull_request
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
base: main
head: micha/accumulator-diagnostics
created_at: 2024-11-05T19:31:27Z
updated_at: 2024-11-06T09:04:42Z
url: https://github.com/astral-sh/ruff/pull/14116
synced_at: 2026-01-10T20:50:57Z
```

# Use accumulator for diagnostics

---

_Pull request opened by @MichaReiser on 2024-11-05 19:31_

## Summary

Use a Salsa accumulator for diagnostics to avoid double-emitting diagnostics for unpack assignments.

We now use a single `CompileDiagnostic` accumulator that stores all diagnostics. That includes IO errors, parse errors, and type check errors. I decided that we're beyond where `String` annotations are fun. That's why I introduced a `Diagnostic` trait with 5 implementations:

* `ParseDiagnostic` for parse errors
* `SourceTextDiagnostic` for IO errors
* `TypeCheckDiagnostic` for type inference errors
* `RevealedTypeDiagnostic` for `reveal_type` (info severity)
* `CompileDiagnostic` which is a cheap cloneable *any* diagnostic wrapper that implements salsa accumulator. 

Using a structured diagnostic has the advantage of no longer needing manual parsing in the LSP. 

A nice side effect of this is that mdtests now support tests with syntax errors because parse-errors are just another diagnostic :)

## Performance regression

I expect this to hit performance pretty bad, especially the incremental benchmark. The problem is that there's currently no way to run an accumulator concurrently. That's why I had to resort to a hack where we run type checking in parallel, and then collect the diagnostics. This has the downside that Salsa first runs all queries (in parallel) but then has to iterate over all of them again to collect the diagnostics (even if there are none!). The queries are all cached, but it is still expensive because the query dependency tree is somewhat large.

This overhead is especially noticeable in the cache case. We'll need https://github.com/salsa-rs/salsa/pull/568 to do the accumulation and checking in one go. 

## Test Plan

`cargo test`


---

_@MichaReiser reviewed on 2024-11-05 19:32_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:40 on 2024-11-05 19:32_

This has to be a salsa query so that the mdtest framework can get the diagnostics (accumulator require a query)

---

_@MichaReiser reviewed on 2024-11-05 19:34_

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/edit/range.rs`:1 on 2024-11-05 19:34_

All the code here is copied over from `ruff_server`.

---

_@MichaReiser reviewed on 2024-11-05 19:36_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/parsed.rs`:45 on 2024-11-05 19:36_

TODO

---

_Comment by @codspeed-hq[bot] on 2024-11-05 19:37_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha/accumulator-diagnostics)

### Merging #14116 will **degrade performances by 32.2%**

<sub>Comparing <code>micha/accumulator-diagnostics</code> (371e5fc) with <code>main</code> (4ece8e5)</sub>



### Summary

`❌ 1` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha/accumulator-diagnostics)._

### Benchmarks breakdown

|     | Benchmark | `main` | `micha/accumulator-diagnostics` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `red_knot_check_file[incremental]` | 4.2 ms | 6.3 ms | -32.2% |


---

_Marked ready for review by @MichaReiser on 2024-11-05 19:51_

---

_Review requested from @carljm by @MichaReiser on 2024-11-05 19:51_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-05 19:51_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-05 19:51_

---

_Label `red-knot` added by @MichaReiser on 2024-11-05 19:52_

---

_Comment by @github-actions[bot] on 2024-11-05 20:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @carljm on `crates/ruff_db/src/diagnostic.rs`:2 on 2024-11-05 22:26_

What is the benefit of the `as _` here? It seems to be just as happy for me locally without it.
```suggestion
use salsa::Accumulator;
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:5457 on 2024-11-05 22:36_

oh, great catch! I think now that the accidental syntax error is fixed here, it would now be trivial to port this to an mdtest.

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/lib.rs`:89 on 2024-11-05 22:38_

```suggestion
                // The accumulator returns all diagnostics from all files. We're only interested in
```

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/diagnostic.rs`:57 on 2024-11-05 22:52_

What's the benefit of implementing these methods in the `impl` block here, and then again in the trait `impl` directly below? Couldn't we just inline these definitions in the trait `impl`? I remember you telling me off for doing something like this back in https://github.com/astral-sh/ruff/pull/11564#discussion_r1615996481 ;)

---

_Review comment by @AlexWaygood on `crates/red_knot_workspace/src/workspace.rs`:232 on 2024-11-05 22:56_

I find the formatting here makes it quite hard to read. Just importing `TextRange` into the namespace would make it much more readable, in my opinion:

```diff
--- a/crates/red_knot_workspace/src/workspace.rs
+++ b/crates/red_knot_workspace/src/workspace.rs
@@ -14,6 +14,7 @@ use ruff_db::{
     system::{walk_directory::WalkState, SystemPath, SystemPathBuf},
 };
 use ruff_python_ast::{name::Name, PySourceType};
+use ruff_text_size::TextRange;
 
 use crate::db::Db;
 use crate::db::RootDatabase;
@@ -215,11 +216,8 @@ impl Workspace {
         workspace_diagnostics.sort_unstable_by(|a, b| {
             a.file().cmp(&b.file()).then_with(|| {
                 a.range()
-                    .map_or(TextSize::default(), ruff_text_size::TextRange::start)
-                    .cmp(
-                        &b.range()
-                            .map_or(TextSize::default(), ruff_text_size::TextRange::start),
-                    )
+                    .map_or(TextSize::default(), TextRange::start)
+                    .cmp(&b.range().map_or(TextSize::default(), TextRange::start))
             })
         });
```

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/red_knot.rs`:29 on 2024-11-05 22:58_

nit: could we have a space in between "error" and the code in square brackets? e.g.

```suggestion
    "error [possibly-unresolved-reference] /src/tomllib/_parser.py:66:18 Name `s` used when possibly not defined",
```

---

_@AlexWaygood reviewed on 2024-11-05 22:59_

Just a few comments from skimming. I'll try to look at this more in-depth tomorrow!

---

_Review comment by @carljm on `crates/red_knot_test/src/lib.rs`:90 on 2024-11-05 23:21_

Should we be doing a workspace check once and then splitting the diagnostics by file, instead of re-doing the check for each file and filtering the diagnostics?

Doesn't need to be done in this PR, regardless. And maybe tests are small enough that it really doesn't matter.

---

_Review comment by @carljm on `crates/red_knot_test/src/diagnostic.rs`:6 on 2024-11-05 23:25_

It's not a problem, but I'm curious why we stopped using the `Ranged` trait here; the reason isn't obvious to me.

---

_@carljm approved on 2024-11-05 23:39_

Looks great to me, thank you! I think this is definitely an improvement in terms of reliable handling of diagnostics (to avoid duplicating or accidentally missing them); hopefully parallel-db brings back most of the regression.

---

_@MichaReiser reviewed on 2024-11-06 07:06_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic.rs`:2 on 2024-11-06 07:06_

Both work, for as long as there's no other type named `Accumulator`. Using `as _` imports that type "unnamed". You can't reference the type itself. That's often sufficient for traits because all that is needed is to bring the trait into the scope to call that trait's functions.

---

_@MichaReiser reviewed on 2024-11-06 07:06_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/red_knot.rs`:29 on 2024-11-06 07:06_

I prefer the non-space version. Otherwise there are too many free-floating parts

---

_@MichaReiser reviewed on 2024-11-06 07:18_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/diagnostic.rs`:6 on 2024-11-06 07:18_

Oh, I should have reverted that change. It's because `Diagnostic` returns `Option<TextRange>` which wasn't compatible. Anyway, I went ahead and deleted the `mdtest` Diagnostic and replaced it with `Diagnostic`. 

---

_Comment by @MichaReiser on 2024-11-06 07:21_

I just noticed that we can now support invalid-syntax in mdtests. I ported the exception handler with invalid syntax test case to demonstrate it

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:90 on 2024-11-06 07:25_

The method to check all files is defined in `red_knot_workspace` on which this crate doesn't depend. It also requires creating a workspace, which is currently somewhat awkward and it does concurrent checking, which we don't want. That's why I think the current approach is still the right approach for testing

---

_@MichaReiser reviewed on 2024-11-06 07:25_

---

_@MichaReiser reviewed on 2024-11-06 07:38_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic.rs`:57 on 2024-11-06 07:38_

I've to be careful with you. You never forget lol.... 

I don't have a good answer. I was mainly annoyed that I had to bring the `Diagnostic` trait into scope everywhere were I sued the `CompileDiagnostic`. Looking at it now, it's exactly two places where this avoids importing the trait. I think it's fine to remove this for now

---

_Comment by @MichaReiser on 2024-11-06 09:04_

I don't think this is the way to go. The problem is that salsa's accumulator are `O(tree)` and not `O(changes)` which introduces a significant cost. They are very convenient but I'm concerned about the incremental performance in very large mono repository... 

I have a fix specific for unpacking and I'll look into extracting some of the improvements I made in this PR but I'm more convinced now that accumulators, as they are today, are not a good fit with our very deeply nested queries.

---

_Closed by @MichaReiser on 2024-11-06 09:04_

---

```yaml
number: 13251
title: "[red-knot] Add type inference for loop variables inside comprehension scopes"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/comprehension-inference
created_at: 2024-09-05T11:23:52Z
updated_at: 2024-09-09T20:30:55Z
url: https://github.com/astral-sh/ruff/pull/13251
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] Add type inference for loop variables inside comprehension scopes

---

_Pull request opened by @AlexWaygood on 2024-09-05 11:23_

## Summary

This adds type inference for loop variables inside comprehension scopes. The type-inference tests are passing, but it seems to currently trigger a panic in the corpus tests.

Async comprehensions will require different logic, so for now loop variables in these are inferred as `Unknown` still. In order to distinguish between async comprehension definitions and synchronous comprehensions definitions, a new `is_async` field is added to `ComprehensionDefinitionKind` and various other structs for tracking comprehension definitions.

## Test Plan

`cargo test -p red_knot_python_semantic` and `cargo test -p red_knot_workspace`


---

_Label `red-knot` added by @AlexWaygood on 2024-09-05 11:23_

---

_Renamed from "[WIP] Add type inference for loop variables inside comprehension scopes" to "[WIP] [red-knot] Add type inference for loop variables inside comprehension scopes" by @AlexWaygood on 2024-09-05 11:25_

---

_@dhruvmanila reviewed on 2024-09-05 11:26_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1721 on 2024-09-05 11:26_

It might not be the first parent in case there are nested comprehensions like `[[x for x in iter1] for y in iter2]`, you might need to traverse up until there's a non-comprehension scope.

---

_Comment by @codspeed-hq[bot] on 2024-09-05 11:33_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex/comprehension-inference)

### Merging #13251 will **not alter performance**

<sub>Comparing <code>alex/comprehension-inference</code> (86aacad) with <code>main</code> (ac720cd)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_@AlexWaygood reviewed on 2024-09-05 11:37_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1721 on 2024-09-05 11:37_

I just pushed a test for that. It seems to work fine :)

I think the nonlocal name lookup happens automatically now, following https://github.com/astral-sh/ruff/commit/3c4ec82aee18c8438a5f807853edcf703e0816a0. We need to look it up in the parent scope because it is _not_ valid for the iterable to reference any variables defined in _this_ scope. But looking it up in the parent scope doesn't mean that it will stop at the parent scope; it will continue iterating up through the parent scopes until it finds a definition (or infer `Unbound` if it can't)

---

_Comment by @MichaReiser on 2024-09-05 11:41_

Just FYI: I think the benchmark is panicking now. I see some `std::io::Write` show up in the benchmarks.

---

_Comment by @AlexWaygood on 2024-09-05 11:54_

lots of things are currently broken here 😆 I just figured I'd make a draft PR to show where I'm at (and in case it's obvious to anybody else what the cause of the panics is)

---

_@dhruvmanila reviewed on 2024-09-05 12:02_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1721 on 2024-09-05 12:02_

Oh interesting, thanks for clarifying

---

_Comment by @MichaReiser on 2024-09-05 12:20_

> lots of things are currently broken here 😆 I just figured I'd make a draft PR to show where I'm at (and in case it's obvious to anybody else what the cause of the panics is)

Haha, okay. I was shocked by the performance regression, so I had to take a quick look at why it was that bad. I was relieved to see that it was probably caused by the benchmark crashing. 

---

_Comment by @AlexWaygood on 2024-09-05 12:22_

> I was shocked by the performance regression, so I had to take a quick look at why it was that bad. I was relieved to see that it was probably caused by the benchmark crashing.

Oh the PR also currently adds a `dbg!()` call in quite a hot location so that I can see what's going on more clearly. I assume that doesn't help 😄

---

_Renamed from "[WIP] [red-knot] Add type inference for loop variables inside comprehension scopes" to "[red-knot] Add type inference for loop variables inside comprehension scopes" by @AlexWaygood on 2024-09-09 19:22_

---

_Marked ready for review by @AlexWaygood on 2024-09-09 19:22_

---

_Review requested from @carljm by @AlexWaygood on 2024-09-09 19:22_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-09-09 19:22_

---

_@AlexWaygood reviewed on 2024-09-09 19:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1448 on 2024-09-09 19:26_

(This gets you a much better error message as it tells you what `previous` was in the error message)

---

_Comment by @github-actions[bot] on 2024-09-09 19:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-09-09 19:39_

> Haha, okay. I was shocked by the performance regression, so I had to take a quick look at why it was that bad. I was relieved to see that it was probably caused by the benchmark crashing.

All gone now 🎉

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1764 on 2024-09-09 19:42_

```suggestion
        // Two things are different if it's the first comprehension:
        // (1) We must lookup the `ScopedExpressionId` of the iterable expression in the outer scope,
        //     because that's the scope we visit it in in the semantic index builder
        // (2) We must *not* call `self.extend()` on the result of the type inference,
        //     because `ScopedExpressionId` are only meaningful within their own scope, so
        //     we'd add types for random wrong expressions in the current scope
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4387 on 2024-09-09 19:47_

Should we also have a test for the case where the inner comprehension actually does iterate over a name defined by the outer comprehension, instead of a name defined in the outer outer scope?

---

_@carljm approved on 2024-09-09 19:56_

Looks great!!

---

_@AlexWaygood reviewed on 2024-09-09 19:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4387 on 2024-09-09 19:58_

Sure!

---

_@carljm approved on 2024-09-09 20:20_

All of the tests! Looks excellent, merge away

---

_Comment by @AlexWaygood on 2024-09-09 20:20_

Thanks very much for the help debugging this!

---

_Merged by @AlexWaygood on 2024-09-09 20:22_

---

_Closed by @AlexWaygood on 2024-09-09 20:22_

---

_Branch deleted on 2024-09-09 20:22_

---

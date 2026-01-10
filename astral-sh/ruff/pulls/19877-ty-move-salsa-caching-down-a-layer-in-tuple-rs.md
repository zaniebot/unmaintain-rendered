```yaml
number: 19877
title: "[ty] Move Salsa caching \"down a layer\" in `tuple.rs`"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
base: main
head: alex/tuple-salsa-cache
created_at: 2025-08-12T12:56:50Z
updated_at: 2025-08-13T15:17:08Z
url: https://github.com/astral-sh/ruff/pull/19877
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Move Salsa caching "down a layer" in `tuple.rs`

---

_Pull request opened by @AlexWaygood on 2025-08-12 12:56_

## Summary

This PR moves the Salsa caching "down a layer" in `tuple.rs`:
- `TupleType` is no longer a Salsa-tracked struct; instead, it's just a thin wrapper around a Salsa-tracked struct, similar to `NominalInstanceType`, `ProtocolInstanceType` and others.
- `TupleSpec` (the type wrapped by `TupleType`) is no longer a type alias; instead, it's now a Salsa-tracked struct.

The benefits of this are that we need to use many fewer `Cow`s across the ty codebase (which were previously required due to `TupleSpec` not being `Copy`), and we can avoid some very complicated trait bounds that we currently have in the signature of `TupleType::new()` on `main`.

## Test Plan

Existing tests


---

_Label `internal` added by @AlexWaygood on 2025-08-12 12:56_

---

_Label `ty` added by @AlexWaygood on 2025-08-12 12:56_

---

_Comment by @github-actions[bot] on 2025-08-12 12:59_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-12 16:21:25.297441346 +0000
+++ new-output.txt	2025-08-12 16:21:25.364441662 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1f113)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(5f5e)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-12 13:00_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-08-12 13:22_

~~Performance is in the noise: some [microbenchmarks](https://codspeed.io/astral-sh/ruff/branches/alex%2Ftuple-salsa-cache?runnerMode=Instrumentation) are showing a 2% slowdown, but some [walltime](https://codspeed.io/astral-sh/ruff/branches/alex%2Ftuple-salsa-cache?runnerMode=WallTime) benchmarks are showing a 2% speedup.~~

The latest version of the PR shows small but consistent improvements of 1-2% on quite a few benchmarks.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/tuple.rs`:212 on 2025-08-12 13:26_

Can't we implement `to_class_type` on the `TupleSpec` instead to avoid needing to intern `TupleType`? 

Or leave the function here but change the signoature to take a `TupleSpec` instead

---

_@MichaReiser reviewed on 2025-08-12 13:26_

---

_@AlexWaygood reviewed on 2025-08-12 14:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:212 on 2025-08-12 14:24_

> Or leave the function here but change the signoature to take a `TupleSpec` instead

great idea! I applied that

---

_Marked ready for review by @AlexWaygood on 2025-08-12 14:29_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-12 14:29_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-12 14:29_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-12 14:29_

---

_@AlexWaygood reviewed on 2025-08-12 14:30_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:167 on 2025-08-12 14:30_

These are moved to `instance.rs` because we can do the same things with less indirection from that module (we can access private implementation details of `NominalInstanceType` from that module)

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-08-12 14:30_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/tuple.rs`:174 on 2025-08-12 14:54_

These bounds were only here to allow callers to pass in a reference to a `TupleSpec` or an owned one. If you pass in a reference, we could avoid the clone if the result ended up being `None`, or if there was already an interned `TupleType` with an equivalent `TupleSpec`.

When I first added this code, there were some hot path calls where that seemed to make a noticeable difference in performance. But looking again now, that no longer seems to be the case — in most places we're already calling with an owned `TupleSpec`. So, given that, you could also get rid of these bounds by just taking in an owned `TupleSpec`, and requiring the caller to `clone` before calling if they have a reference.

---

_Comment by @AlexWaygood on 2025-08-12 15:00_

(CI failures are all due to the [GitHub outage](https://www.githubstatus.com/))

---

_@dcreager reviewed on 2025-08-12 15:21_

As an alternative, we might consider just making `Tuple` (and therefore `TupleSpec` in the world where it's still an alias) easier/cheaper to clone. On the assumption that most tuple specs are small, we could use `SmallVec`s inside of `Tuple` and friends, and just `clone` the `TupleSpec`s where we need to. Then you'd get rid of the `Cow`s by returning owned `TupleSpec`s everywhere.

> we can avoid some very complicated trait bounds that we currently have in the signature of `TupleType::new()` on `main`.

See below; this was only there to let us call the salsa interning logic with a reference, but if `TupleSpec` becomes cheap to clone, that's no longer needed.

---

_Comment by @AlexWaygood on 2025-08-12 15:39_

> As an alternative, we might consider just making `Tuple` (and therefore `TupleSpec` in the world where it's still an alias) easier/cheaper to clone. On the assumption that most tuple specs are small, we could use `SmallVec`s inside of `Tuple` and friends, and just `clone` the `TupleSpec`s where we need to. Then you'd get rid of the `Cow`s by returning owned `TupleSpec`s everywhere.

Hmm... we _could_, but I'm not sure I see the advantages of doing it that way. If we did this we'd have to keep `TupleType` as a Salsa-interned struct, or `NominalInstanceType` would no longer be `Copy` (and if `NominalInstanceType` were no longer `Copy`, `Type` would no longer be `Copy`). Our size constraints mean we have to intern either one or both of `TupleType` and `TupleSpec`.

What are the benefits of your proposal relative to my current PR?

---

_Comment by @dcreager on 2025-08-12 18:45_

> Hmm... we _could_, but I'm not sure I see the advantages of doing it that way. If we did this we'd have to keep `TupleType` as a Salsa-interned struct, or `NominalInstanceType` would no longer be `Copy` (and if `NominalInstanceType` were no longer `Copy`, `Type` would no longer be `Copy`). Our size constraints mean we have to intern either one or both of `TupleType` and `TupleSpec`.
> 
> What are the benefits of your proposal relative to my current PR?

Yep, with my suggestion, `TupleType` would remain interned, like it was before. We've found (e.g. in https://github.com/astral-sh/ruff/pull/19867) that interning fewer things is generally a good tradeoff, since interning requires cross-thread synchronization. My hunch is that we'll intern more values if we do the caching at the `TupleSpec` level. `try_iterate` is an example: it returns a `TupleSpec`, and there are call sites where we do not wrap that in a `TupleType`. So with this PR, we now have to intern the result of that function in places where we didn't have to before.

---

_Comment by @AlexWaygood on 2025-08-12 19:15_

> Yep, with my suggestion, `TupleType` would remain interned, like it was before. We've found (e.g. in #19867) that interning fewer things is generally a good tradeoff, since interning requires cross-thread synchronization. My hunch is that we'll intern more values if we do the caching at the `TupleSpec` level. `try_iterate` is an example: it returns a `TupleSpec`, and there are call sites where we do not wrap that in a `TupleType`. So with this PR, we now have to intern the result of that function in places where we didn't have to before.

Hmmmmmm, I see, fair enough. I'm _slightly_ sceptical that #19867 is indicative of a broader problem when it comes to lock contention -- my instinct is that that issue is quite sui generis. But I do agree that we should probably try to intern the minimum number of objects in the Salsa cache, since it leads to higher memory usage overall; and I think you're right that this PR in its current form probably leads to us interning a number of `TupleSpec`s that don't really need to be interned at the end of the day.

---

_Comment by @MichaReiser on 2025-08-13 06:53_

> Hmmmmmm, I see, fair enough. I'm slightly sceptical that https://github.com/astral-sh/ruff/pull/19867 is indicative of a broader problem when it comes to lock contention -- my instinct is that that issue is quite sui generis.

I guess that depends on the perspective :) It is specific in the sense that it is only an issue when the same interned values (of a specific interned struct) are used in a very hot code path. It isn't an issue if many threads intern many different interned structs because Salsa shards the locks by value, meaning most threads will end up acquiring different locks. 

I don't understand `try_iterate` well enough to understand if this falls into the former (we often call it on very few tuples) or the latter (we call it on many different tuples) bucket.

The other part that makes #19867 somewhat unique is that it's a very hot methods. I'm not sure if `try_iterate` is equally hot. Either way. It's something you can test easily yourself. Build ty with the `profiling` profile (`cargo build --bin ty --profile profiling`) and run `samply record ./ruff/target/profiling/ty check ...` on a large repository. Then invert the stack frames and verify if there are now more frames attributed to the mutex lock cold path (see the PR for an example) 

---

_Comment by @AlexWaygood on 2025-08-13 09:28_

I find @dcreager's point pretty convincing: most of the paths in `Type::try_iterate` end up creating a fresh `TupleSpec` rather than reusing an (already borrowed one), because it returns a `TupleSpec` for all iterable types (not just tuples) in order to describe the behaviour these types have when they're unpacked. That means we'll end up interning (possibly many) `TupleSpec`s unnecessarily in many cases with the design proposed in this PR.

I don't _love_ how viral our current design makes all the `Cow` types, but I'm persuaded that it's probably the least-bad option for now. I'll split out some of the other simplifications/improvements in this PR into a standalone PR.

Thanks @dcreager!

---

_Closed by @AlexWaygood on 2025-08-13 09:28_

---

_Branch deleted on 2025-08-13 09:28_

---

_Comment by @dcreager on 2025-08-13 15:13_

> I don't _love_ how viral our current design makes all the `Cow` types

We can still try to get rid of the `Cow`s, if we want — we'd just have to make `Tuple` (and therefore `TupleSpec`) cheap to clone, so that we can return owned values all the time. They're just a couple of boxed slices of `Type`s, so they might be cheap enough to copy already!

---

_Comment by @AlexWaygood on 2025-08-13 15:17_

Right! But I consider the price of a few ugly 🐄s worth it to avoid some unnecessary clones, even if those clones wouldn't actually be _that_ expensive 😆

---

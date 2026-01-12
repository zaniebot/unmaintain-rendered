```yaml
number: 14956
title: "Introduce `InferContext`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/type-context
created_at: 2024-12-13T11:05:24Z
updated_at: 2024-12-18T12:26:16Z
url: https://github.com/astral-sh/ruff/pull/14956
synced_at: 2026-01-12T15:55:49Z
```

# Introduce `InferContext`

---

_@MichaReiser_

## Summary

I'm currently on the fence about landing the #14760 PR because it's unclear how we'd support tracking used and unused suppression comments in a performant way:
* Salsa adds an "untracked" dependency to every query reading accumulated values. This has the effect that the query re-runs on every revision. For example, a possible future query `unused_suppression_comments(db, file)` would re-run on every incremental change and for every file. I don't expect the operation itself to be expensive, but it all adds up in a project with 100k+ files
* Salsa collects the accumulated values by traversing the entire query dependency graph. It can skip over sub-graphs if it is known that they contain no accumulated values. This makes accumulators a great tool for when they are rare; diagnostics are a good example. Unfortunately, suppressions are more common, and they often appear in many different files, making the "skip over subgraphs" optimization less effective. 

Because of that, I want to wait to adopt salsa accumulators for type check diagnostics (we could start using them for other diagnostics) until we have very specific reasons that justify regressing incremental check performance. 

This  PR does a "small" refactor that brings us closer to what I have in #14760 but without using accumulators. To emit a diagnostic, a method needs:

* Access to the db
* Access to the currently checked file

This PR introduces a new `InferContext` that holds on to the db, the current file, and the reported diagnostics. It replaces the `TypeCheckDiagnosticsBuilder`. We pass the `InferContext` instead of the `db` to methods that *might* emit diagnostics. This simplifies some of the `Outcome` methods, which can now be called with a context instead of a `db` and the diagnostics builder. Having the `db` and the file on a single type like this would also be useful when using accumulators. 

This PR doesn't solve the issue that the `Outcome` types feel somewhat complicated nor that it can be annoying when you need to report a `Diagnostic,` but you don't have access to an `InferContext` (or the file). However, I also believe that accumulators won't solve these problems because:

* Even with accumulators, it's necessary to have a reference to the file that's being checked. The struggle would be to get a reference to that file rather than getting a reference to `InferContext`.
* Users of the `HasTy` trait (e.g., a linter) don't want to bother getting the `File` when calling `Type::return_ty` because they aren't interested in the created diagnostics. They just want to know what calling the current expression would return (and if it even is a callable). This is what the different methods of `Outcome` enable today. I can ask for the return type without needing extra data that's only relevant for emitting a diagnostic.

A shortcoming of this approach is that it is now a bit confusing when to pass `db` and when an `InferContext`. An option is that we'd make the `file` on `InferContext` optional (it won't collect any diagnostics if `None`) and change all methods on `Type` to take `InferContext` as the first argument instead of a `db`. I'm interested in your opinion on this.

Accumulators are definitely harder to use incorrectly because they remove the need to merge the diagnostics explicitly and there's no risk that we accidentally merge the diagnostics twice, resulting in duplicated diagnostics. I still value performance more over making our life slightly easier.

---

_Comment by @github-actions[bot] on 2024-12-13 11:14_

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

_Label `red-knot` added by @AlexWaygood on 2024-12-13 12:11_

---

_Renamed from "Introduce `TyContext`" to "Introduce `InferContext`" by @MichaReiser on 2024-12-13 12:40_

---

_Marked ready for review by @MichaReiser on 2024-12-13 13:03_

---

_Review requested from @carljm by @MichaReiser on 2024-12-13 13:03_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-13 13:03_

---

_Review requested from @sharkdp by @MichaReiser on 2024-12-13 13:03_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/context.rs`:43 on 2024-12-13 19:02_

```suggestion
            defused: DropBomb::new("`InferContext` need to be explicitly consumed by calling `::finish` to prevent accidental loss of diagnostics."),
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/context.rs`:34 on 2024-12-13 19:03_

minor nit: the name `defused` confused me at first, because initially this is not defused! It's only defused after `finish()`. Would have been clearer to me if it were named `bomb` or `drop_bomb`

---

_@carljm approved on 2024-12-13 19:07_

Thank you, this looks great to me! Even though it doesn't solve the problem of reporting diagnostics across the Type / TypeInferenceBuilder boundary, because of needing a node/location, it still looks like a clear improvement in API, and the DropBomb will help prevent dropping diagnostics by accident.

---

_Comment by @carljm on 2024-12-13 19:13_

> A shortcoming of this approach is that it is now a bit confusing when to pass `db` and when an `InferContext`. An option is that we'd make the `file` on `InferContext` optional (it won't collect any diagnostics if `None`) and change all methods on `Type` to take `InferContext` as the first argument instead of a `db`. I'm interested in your opinion on this.

I feel like the direction we were heading in the discussion this morning is to continue returning outcome/result types from `Type` operations, and keep nodes/locations in `TypeInferenceBuilder` (and sometimes passed into methods on those outcome/result types, which can then emit diagnostics.)

I think this direction makes sense. I don't want to be passing, or to require, nodes/locations  everywhere in `Type` methods. This means we can't emit diagnostics in arbitrary places in `Type` methods, either.

Given that approach, I don't think it's too hard to understand where to pass Db vs InferContext. `Type` methods should only require a `Db`. `InferContext` should only be used where we need to emit diagnostics, which will generally just be in a few methods on result/outcome types.

---

_Merged by @MichaReiser on 2024-12-18 12:22_

---

_Closed by @MichaReiser on 2024-12-18 12:22_

---

_Branch deleted on 2024-12-18 12:22_

---

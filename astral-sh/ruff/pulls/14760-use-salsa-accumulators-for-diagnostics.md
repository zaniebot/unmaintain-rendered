```yaml
number: 14760
title: Use salsa accumulators for diagnostics
type: pull_request
state: closed
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
draft: true
base: main
head: micha/salsa-accumulators
created_at: 2024-12-03T17:47:23Z
updated_at: 2024-12-30T09:09:48Z
url: https://github.com/astral-sh/ruff/pull/14760
synced_at: 2026-01-10T20:42:27Z
```

# Use salsa accumulators for diagnostics

---

_Pull request opened by @MichaReiser on 2024-12-03 17:47_

## Summary

This is a reimplementation of https://github.com/astral-sh/ruff/pull/14116 now that Salsa [salsa accumulators are cheaper](https://github.com/salsa-rs/salsa/pull/615) (close to "free" for queries without diagnostics). 

The main benefit of salsa accumulators is that they're an easy way to emit a diagnostic from anywhere inside a salsa query, and salsa takes care to deduplicate diagnostics if the same query is called multiple times.

## Performance

This still significantly regresses the incremental performance. I created another [salsa PR](https://github.com/salsa-rs/salsa/pull/622) to reduce the regression to 9%. I don't think we can do much better except reducing the number of inputs by using [coarse-grained](https://github.com/salsa-rs/salsa/issues/598) salsa dependencies because:

* Salsa currently tracks reads on a per "field" level for salsa structs. This very fine-grained tracking costs us here because the accumulator has to iterate and resolve all of them to know if they have any accumulated values. The coarse-grained salsa feature will help with this (and reduce the overhead in other places as well)
* We only check 4 files (plus standard library files that are all unchanged and have no diagnostics). The regression would be less proportionally if there were more files without diagnostics.
* The diagnostics are from the largest file and, to make it worse, from the module scope, which has the most definitions.

Considering this, I'm still leaning towards migrating to salsa accumulators because of what they unlock: We can now emit diagnostics from any part of the code. This includes the module resolver (e.g. emit a warning if we find an invalid `pth` file?) 

## Other changes

This PR upgrades Salsa to a version with "cheap" accumulators. The new salsa version now requires that `Db` structs implement `Clone` for its parallel db support (which doesn't seem to work yet). 

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2024-12-03 17:47_

---

_Label `red-knot` added by @MichaReiser on 2024-12-03 17:47_

---

_Comment by @codspeed-hq[bot] on 2024-12-03 17:55_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fsalsa-accumulators)

### Merging #14760 will **degrade performances by 10.43%**

<sub>Comparing <code>micha/salsa-accumulators</code> (35f3815) with <code>main</code> (1685d95)</sub>



### Summary

`❌ 1 (👁 1)` regressions  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `main` | `micha/salsa-accumulators` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `red_knot_check_file[incremental]` | 4 ms | 4.4 ms | -10.43% |


---

_Comment by @github-actions[bot] on 2024-12-03 18:03_

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

_Marked ready for review by @MichaReiser on 2024-12-04 10:15_

---

_Review requested from @carljm by @MichaReiser on 2024-12-04 10:15_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-04 10:15_

---

_Review requested from @sharkdp by @MichaReiser on 2024-12-04 10:15_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/diagnostic.rs`:81 on 2024-12-04 11:43_

is `report` the best verb here? It feels to me like we're adding a diagnostic to be reported later, rather than reporting it to the user immediately. Maybe this could be called `CompileDiagnostic::add()`?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:238 on 2024-12-04 11:44_

similarly here, `add_type_diagnostic` would be a more natural name for me

---

_@AlexWaygood approved on 2024-12-04 11:47_

The use of `report_` everywhere makes me feel like I'm reading the pyright codebase haha. But in most cases it seems like it probably is the best verb to use. Still, there's a couple of places where I think we could use some better names:

---

_Comment by @MichaReiser on 2024-12-04 13:36_

Hmm, I'm looking into suppressions right now, specifically how we'd recognize unused suppression comments. 
We could ignore inline and file-level suppression comments when deciding whether or not to emit a diagnostic and then filter them out as part of a post-processing step. The diagnostics would then allow us to "account" for the seen suppressions and list comments without any suppressions. However, this has the downside that more files have accumulated values, meaning the performance regression remains relevant for more files. 

Another alternative to emitting the diagnostic is to emit a `Suppressed` accumulated value. That's cheaper because it can be a more lightweight value (and suppressing a diagnostic can be used to work-around a bug in the diagnostic generation), but it has the same downside as using the diagnostic accumulator: Collecting all values requires an extra traversal

---

_Review comment by @sharkdp on `crates/red_knot_test/src/lib.rs`:105 on 2024-12-04 15:02_

Only tangentially related: do we have a feeling for how many unrelated-file diagnostics we generate typically? If this would be a large fraction of all diagnostics, we should probably detect that earlier?

---

_@sharkdp reviewed on 2024-12-04 15:02_

---

_@MichaReiser reviewed on 2024-12-04 15:17_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:105 on 2024-12-04 15:17_

This could be any number of diagnostics and salsa doesn't provide a way to detect this earlier. 

Salsa accumulators work by traversing the entire dependency tree of a query. In our case, this is `check_file`. `check_file` can branch out into arbitrary files when resolving imports (and checking those files in turn). The only way for us to truncate this earlier is by not using accumulators because salsa doesn't know about the file boundaries. 

I don't think this is very terrible because the CLI uses `check_workspace` to get *all* diagnostics. The LSP uses `check_file` today but I don't think it should. The wasm playground uses `check_file`... this is probably fine

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/string_annotation.rs`:11 on 2024-12-04 15:32_

Minor: Something like

```suggestion
struct StringAnnotationParseError;
type AnnotationParseResult = Result<Parsed<ModExpression>, StringAnnotationParseError>;
```

would maybe make `match`es at the call sites of this function a bit easier to understand

---

_@sharkdp reviewed on 2024-12-04 15:32_

---

_@sharkdp reviewed on 2024-12-04 15:46_

---

_Review comment by @sharkdp on `crates/red_knot_workspace/src/workspace.rs`:207 on 2024-12-04 15:46_

A few questions:

- Do we really want an unstable sort here? Two different diagnostics could compare equal according to the ordering defined here if they refer to the same range, or even just the same range start.
- Why do we order by file and then by the path of the file? Can multiple files refer to the same path?
- If performance is not a major concern here, we could potentially do something like
  ```rs
  diagnostics.sort_unstable_by_key(|diagnostic| {
      let file = diagnostic.file();
      let path = file.path(db).as_str();
      let range_start = diagnostic.range().unwrap_or_default().start();
      (file, path, range_start)
  });
  ```

---

_@MichaReiser reviewed on 2024-12-04 16:07_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/workspace.rs`:207 on 2024-12-04 16:07_

> Why do we order by file and then by the path of the file? Can multiple files refer to the same path?

Oh, that's just wrong. The idea was to short-cut if the files are identical. 

> Do we really want an unstable sort here? Two different diagnostics could compare equal according to the ordering defined here if they refer to the same range, or even just the same range start.

I don't think it's important for us to preserve the original order if there are multiple diagnostics at the same line, for as long as the order is deterministic (Running Red Knot multiple times should give you the same result). We should probably include the rule code as well as disambiguator 



---

_@sharkdp reviewed on 2024-12-04 16:20_

---

_Review comment by @sharkdp on `crates/red_knot_workspace/src/workspace.rs`:207 on 2024-12-04 16:20_

> for as long as the order is deterministic

Exactly, that was my concern.

> We should probably include the rule code as well as disambiguator

:+1: 

---

_@dhruvmanila approved on 2024-12-05 04:20_

---

_Converted to draft by @MichaReiser on 2024-12-10 13:02_

---

_Closed by @MichaReiser on 2024-12-30 09:09_

---

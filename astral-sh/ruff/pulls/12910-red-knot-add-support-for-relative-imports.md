```yaml
number: 12910
title: "[red-knot] Add support for relative imports"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/relative-imports
created_at: 2024-08-15T21:29:03Z
updated_at: 2024-08-16T15:22:32Z
url: https://github.com/astral-sh/ruff/pull/12910
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] Add support for relative imports

---

_Pull request opened by @AlexWaygood on 2024-08-15 21:29_

## Summary

This PR adds support for relative import statements, e.g. `from .foo import bar`, `from . import bar`, or `from ...foo import bar`.

## Test Plan

Several tests added to `infer.rs`.

Co-authored-by: Carl Meyer <carl@astral.sh>

---

_Label `red-knot` added by @AlexWaygood on 2024-08-15 21:29_

---

_Review requested from @carljm by @AlexWaygood on 2024-08-15 21:29_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-15 21:29_

---

_Comment by @AlexWaygood on 2024-08-15 21:30_

(I'm expecting this to have a significant impact on the `tomllib` benchmark, since lots of relative imports in `tomllib` that we previously couldn't understand at all will now be understood)

---

_Comment by @github-actions[bot] on 2024-08-15 21:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @carljm on 2024-08-16 05:00_

I'll let @MichaReiser review this, since I helped write it :)

You'll have to update the expected number of errors in the benchmark before we'll get to see benchmark results.

And it looks like Clippy has a complaint.

---

_Review request for @carljm removed by @carljm on 2024-08-16 05:00_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_name.rs`:188 on 2024-08-16 06:01_

We could avoid the need to return an `Error` if we make the `Argument` a `ModuleName`. 

Nit: I think `push` is sufficient because we don't allow pushing at the front and it better aligns with the `str` API. 
```suggestion
    pub fn join(&mut self, other &ModuleName {
    	self.0.push(&other.0);
    }
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:840 on 2024-08-16 06:03_

Nit: 

```suggestion
        let mut level = level.get();
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:887 on 2024-08-16 06:04_

I think we should add a TODO here to raise an error if the module name is not a valid identifier or how do we do this for non-relative paths?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1828 on 2024-08-16 06:05_

Can we add a TODO here?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1873 on 2024-08-16 06:05_

Can we add a test for a file that's not a module and verify what pyright's behavior is in that case?

---

_@MichaReiser approved on 2024-08-16 06:06_

This looks great. Again, much simpler than I expected and nice that the `file_to_module` provides everything that we need.

---

_@AlexWaygood reviewed on 2024-08-16 07:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/module_name.rs`:188 on 2024-08-16 07:43_

> We could avoid the need to return an `Error` if we make the `Argument` a `ModuleName`.

Great idea!

> Nit: I think `push` is sufficient because we don't allow pushing at the front

Hmm, I don't think my current name is great but I don't like `push`, because I'd expect a `push` method to only accept a single component (like `Vec::push`), which isn't what this method does (the string it accepts could be a single component, but it could also be a string representing multiple components).

> it better aligns with the `str` API.

I don't think this is a great reason because the fact that this type wraps a string currently is an implementation detail. Conceptually, an absolute module name is a list of identifier strings separated by dots, so in terms of the semantics of the type it could just as easily wrap a `Vec` (or `SmallVec`, etc)

---

_@MichaReiser reviewed on 2024-08-16 07:50_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_name.rs`:188 on 2024-08-16 07:50_

How about `extend` or `join`. Because your concerns about `push` also apply to `push_tail`

---

_@AlexWaygood reviewed on 2024-08-16 09:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:887 on 2024-08-16 09:18_

Other than `from foo import *` imports (which will anyway require their own branch of logic), I don't think it's possible for any components of the module name here to not be a valid identifier. The source code would fail to parse, so we'd never get here. So I'm not sure how to add a test for this.

Ideally the `ruff_python_ast::nodes::Identifier` node would have invariants around it that ensured that it never wrapped a string that wasn't a valid Python identifier: if so, we could also implement a more efficient conversion from `ruff_python_ast::nodes::Identifier` to `ModuleName`, that skips some of the checks performed by the `ModuleName` constructor.

---

_Comment by @codspeed-hq[bot] on 2024-08-16 09:35_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex/relative-imports)

### Merging #12910 will **degrade performances by 29.01%**

<sub>Comparing <code>alex/relative-imports</code> (79c1853) with <code>main</code> (9b73532)</sub>



### Summary

`⚡ 1` improvements
`❌ 2 (👁 2)` regressions
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `alex/relative-imports` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 779.1 µs | 727.8 µs | +7.05% |
| 👁 | `red_knot_check_file[cold]` | 43.4 ms | 51 ms | -14.9% |
| 👁 | `red_knot_check_file[incremental]` | 1.9 ms | 2.7 ms | -29.01% |


---

_@MichaReiser reviewed on 2024-08-16 09:59_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:887 on 2024-08-16 09:59_

Fair, although this makes assumptions about what the parser does. The parser could decide to recover from `a.5.b` because that gives a better overall parse result than stopping after `a.5`. 

I think it would be good to at least log a debug or warning because it would be an unexpected outcome.

---

_@MichaReiser reviewed on 2024-08-16 10:00_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/red_knot.rs`:92 on 2024-08-16 10:00_

Woohoo nice

---

_@AlexWaygood reviewed on 2024-08-16 10:04_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:887 on 2024-08-16 10:04_

> Fair, although this makes assumptions about what the parser does. The parser could decide to recover from `a.5.b` because that gives a better overall parse result than stopping after `a.5`.

Okay, but even if it decides to do error recovery, the parser should still mark the node as invalid somehow, correct? And if so, we shouldn't *ever* get here; it would be fruitless to attempt to do type analysis on a module that's known to contain invalid syntax (which is what this would be)

> I think it would be good to at least log a debug or warning because it would be an unexpected outcome.

fair enough

---

_@MichaReiser reviewed on 2024-08-16 10:08_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:887 on 2024-08-16 10:08_

> Okay, but even if it decides to do error recovery, the parser should still mark the node as invalid somehow

Ideally yes, and we should handle it here. Adding a log should help us find this if we ever forget.

>  it would be fruitless to attempt to do type analysis on a module that's known to contain invalid syntax (which is what this would be)

I don't agree with this. Doing type inference on programs with invalid syntax is a primary goal and we should do the best we can or Red Knot won't be suited for editors.

---

_@AlexWaygood reviewed on 2024-08-16 10:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:887 on 2024-08-16 10:17_

> I don't agree with this. Doing type inference on programs with invalid syntax is a primary goal and we should do the best we can or Red Knot won't be suited for editors.

Interesting. From our previous discussions I understood that having an error-recoverable parser that could offer helpful error messages in cases of invalid syntax was a primary goal, but that being able to offer diagnostics and perform semantic analysis on modules with invalid syntax was not a goal that we expected to achieve in the foreseeable future. Was my understanding incorrect? Or are you saying that it's only a non-goal for _Ruff_, and that we plan on supporting modules with invalid syntax from the very start with red-knot?

---

_@MichaReiser reviewed on 2024-08-16 10:20_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:887 on 2024-08-16 10:20_

We de-prioritized the recoverable semantic analysis work in Ruff, but Red Knot should support it from the very start. We may want to spend some more time to think about how we want to do that. @carljm general idea is to just return `Unknown` or `Unbound` in these cases but I start to think that it may involve more than just that. For example, we may want to avoid raising exceptions for calls to a function where the function's declaration contains syntax errors because that likely only contributes noise. 

---

_@AlexWaygood reviewed on 2024-08-16 10:40_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:897 on 2024-08-16 10:40_

Are these tracing calls the kind of thing you were thinking of @MichaReiser? I also added a `tracing::trace!()` call to `infer_import_from_definition`.

---

_@MichaReiser reviewed on 2024-08-16 11:34_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:897 on 2024-08-16 11:34_

LGTM. Thanks

---

_Merged by @AlexWaygood on 2024-08-16 11:35_

---

_Closed by @AlexWaygood on 2024-08-16 11:35_

---

_Branch deleted on 2024-08-16 11:35_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/module_name.rs`:185 on 2024-08-16 15:19_

Nice, I also had a thought late last night that this is the name we should have used for this method, so glad you all settled on the same thing 😆 

---

_@carljm reviewed on 2024-08-16 15:22_

---

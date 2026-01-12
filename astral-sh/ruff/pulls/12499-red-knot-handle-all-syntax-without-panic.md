```yaml
number: 12499
title: "[red-knot] handle all syntax without panic"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/no-panic
created_at: 2024-07-25T02:18:10Z
updated_at: 2024-07-26T00:38:11Z
url: https://github.com/astral-sh/ruff/pull/12499
synced_at: 2026-01-12T15:55:41Z
```

# [red-knot] handle all syntax without panic

---

_@carljm_

Extend red-knot type inference to cover all syntax, so that inferring types for a scope gives all expressions a type. This means we can run the red-knot semantic lint on all Python code without panics. It also means we can infer types for `builtins.pyi` without panics.

To keep things simple, this PR intentionally doesn't add any new type inference capabilities: the expanded coverage is all achieved with `Type::Unknown`. But this puts the skeleton in place for adding better inference of all these language features.

I also had to add basic Salsa cycle recovery (with just `Type::Unknown` for now), because some `builtins.pyi` definitions are cyclic.

To test this, I added a comprehensive corpus of test snippets sourced from Cinder under [MIT license](https://github.com/facebookincubator/cinder/blob/cinder/3.10/cinderx/LICENSE), which matches Ruff's license. I also added to this corpus some additional snippets for newer language features: all the `27_func_generic_*` and `73_class_generic_*` files, as well as `20_lambda_default_arg.py`, and added a test which runs semantic-lint over all these files. (The test doesn't assert the test-corpus files are lint-free; just that they are able to lint without a panic.)

---

_Label `red-knot` added by @carljm on 2024-07-25 02:18_

---

_Review requested from @MichaReiser by @carljm on 2024-07-25 02:18_

---

_Review requested from @AlexWaygood by @carljm on 2024-07-25 02:18_

---

_Comment by @github-actions[bot] on 2024-07-25 02:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @carljm on 2024-07-25 03:38_

@DinoV you might be interested in the files I added to the test corpus here (listed in the PR description); they might be worth adding to the py-compiler test suite in CinderX as well.

---

_Review comment by @MichaReiser on `crates/red_knot/resources/test/corpus/00_const.py`:1 on 2024-07-25 06:31_

We should add cinder's license to the folder to correctly attribute where the files are from. 



---

_Review comment by @MichaReiser on `crates/red_knot/resources/test/corpus/00_empty.py`:1 on 2024-07-25 06:33_

To me it's not clear what tests should be added under `test/corpus`. Should I add our own tests under this directory? What's the coverage of these tests? It seems these are at best, smoke tests because they don't assert anything. 

That's why I would rename the folder to `cinder` (to make the attribution very clear) or `smoke` to at least express the intend fo the tests.

---

_Review comment by @MichaReiser on `crates/red_knot/src/lint.rs`:385 on 2024-07-25 06:35_

I'm leaning towards making these integration tests (move the test to its own file in the `tests` directory instead of in `src`. 


https://doc.rust-lang.org/book/ch11-03-test-organization.html

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:230 on 2024-07-25 06:43_

I recommend using explicit ignores here so that adding a new field to the AST node triggers a compile error
```suggestion
                        range: _
```

Same in other places where we match on AST nodes 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:294 on 2024-07-25 06:45_

Nit
```suggestion
        let type_params = class.type_params.as_deref().expect("class type params scope without type params");
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:424 on 2024-07-25 06:46_

Nit: I think `parameters` implements `Iterator` which returns all parameters in the right order. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:447 on 2024-07-25 06:47_

Nit: Maybe introduce a `infer_optional_expression(default.as_deref())`  method to reduce the `if let Some` 

---

_@MichaReiser approved on 2024-07-25 06:48_

Nice!

---

_Review comment by @AlexWaygood on `.pre-commit-config.yaml`:64 on 2024-07-25 07:22_

I think this should just be added to the "exclude these files from all hooks" regex at the top of this file. I doubt we want any hooks to run on them in CI.

---

_@AlexWaygood approved on 2024-07-25 07:34_

The overall approach looks great to me.

I wonder if this corpus of Python code should go in a redknot crate at all. It could also be a useful resource for testing the parser.

Additionally, we already have a Python script that fuzzes the parser on randomly generated (but guaranteed to be syntactically valid) Python source code (in the `scripts/fuzz-parser` directory), which we run as a CI job. It might be interesting to adapt that script so that it also runs redknot on the generated source code. I don't think it would be too hard to do.

---

_@carljm reviewed on 2024-07-25 14:48_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:447 on 2024-07-25 14:48_

Yeah I thought about this, I'll add it. 

---

_@carljm reviewed on 2024-07-25 14:51_

---

_Review comment by @carljm on `crates/red_knot/resources/test/corpus/00_const.py`:1 on 2024-07-25 14:51_

The actual attribution history is more complicated than that; Cinder originally took these from another Python compiler project and added to them over time. And the Cinder license is MIT license, same as Ruff, and MIT license doesn't require attribution as a condition. So I don't think anything else is really needed here; there's no need or value in having two different copies of the same MIT license in two different places. I can add a readme that clarifies what I know about the history of this corpus, if you think that's useful. 

---

_@carljm reviewed on 2024-07-25 14:53_

---

_Review comment by @carljm on `crates/red_knot/resources/test/corpus/00_empty.py`:1 on 2024-07-25 14:53_

I think we should name these files for what they are, not how they are used. What they _are_ is a body of snippets of Python code aiming to cover the syntax of the language. Anything that fits that description and covers a gap makes sense to add here. I think "corpus" is a good name for that, but I'm open to other suggestions. 

I don't think they should be named for where they came from -- that isn't really relevant to anything. And I don't think they should be named for how we use them in one particular test; as Alex already pointed out, there are more potential uses for them. 

---

_@carljm reviewed on 2024-07-25 15:01_

---

_Review comment by @carljm on `crates/red_knot/resources/test/corpus/00_const.py`:1 on 2024-07-25 15:01_

Oh, I guess Cinder's LICENSE claims copyright of the files, and that is supposed to be preserved with the MIT license. I think that claim is not really correct (since most of the files weren't written as part of Cinder project but were previously borrowed from elsewhere) but we can preserve it anyway.

---

_Comment by @carljm on 2024-07-25 15:14_

Now that I look, I think this test corpus is very similar to what already exists in `crates/ruff_python_parser/resources/valid`. I could just drop the corpus from this PR and use that. I don't know if I want to take the time to verify that it covers all the same things this corpus does, though 😆 

---

_@MichaReiser reviewed on 2024-07-25 16:45_

---

_Review comment by @MichaReiser on `crates/red_knot/resources/test/corpus/00_const.py`:1 on 2024-07-25 16:45_

Yeah, I would prefer preserving. We do it with many other files. 

---

_@MichaReiser reviewed on 2024-07-25 16:48_

---

_Review comment by @MichaReiser on `crates/red_knot/resources/test/corpus/00_empty.py`:1 on 2024-07-25 16:48_

Yeah. I think the main challenge that I see with this is where should I add a red knot specific test? The test may not make sense for the parser (or at least, it wouldn't give us a lot extra coverage so running a dedicated snapshot test for it doesn't make sense). Same for the formatter. 

What's important to me is that contributors will know where to add a test and that it avoids creating unnecessary tests for parts that don't need running or where it doesn't provide any added value.  

---

_@carljm reviewed on 2024-07-25 22:04_

---

_Review comment by @carljm on `.pre-commit-config.yaml`:64 on 2024-07-25 22:04_

Good call! I took the opportunity to also remove the already-redundant exclude entry on ruff, since everything listed there is already listed in the global exclude

---

_Comment by @carljm on 2024-07-26 00:26_

After offline discussion, going to go ahead and merge this with the corpus as and where it is for now; we can explore merging with the parser test corpus if we want to later.

---

_Merged by @carljm on 2024-07-26 00:38_

---

_Closed by @carljm on 2024-07-26 00:38_

---

_Branch deleted on 2024-07-26 00:38_

---

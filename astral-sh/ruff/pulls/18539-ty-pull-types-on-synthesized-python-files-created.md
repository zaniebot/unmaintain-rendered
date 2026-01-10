```yaml
number: 18539
title: "[ty] Pull types on synthesized Python files created by mdtest"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/pulltypes-mdtest
created_at: 2025-06-07T18:33:04Z
updated_at: 2025-06-12T09:32:19Z
url: https://github.com/astral-sh/ruff/pull/18539
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Pull types on synthesized Python files created by mdtest

---

_Pull request opened by @AlexWaygood on 2025-06-07 18:33_

## Summary

Our "corpus tests" run an AST visitor that attempts to exhaustively check that each sub-expression in an AST has a type inferred for it. This is a great way to catch potential panics that might happen when you're hovering over an expression in an IDE, for example, since ty will try to retrieve the type of that node so that the IDE can show a nice tooltip to the user.

The corpus tests have a problem, though. There are a number of typing symbols to which ty applies heavy special casing. [It's](https://github.com/astral-sh/ruff/commit/72552f31e40577f32195624279ead0a5364a82e0) [quite](https://github.com/astral-sh/ruff/commit/95497ffaaba5bbceebda7c58c50a47292f9bdd39) [common](https://github.com/astral-sh/ruff/pull/18534) for us to forget to store a type for a subexpression when inferring types for these heavily special-cased symbols, especially in cases where these symbols are being used invalidly. Our corpus of Python snippets that we run the "corpus tests" over doesn't provide great coverage for these symbols, so it's easy for these mistakes to go unnnoticed.

We _do_ have a corpus of Python snippets that _do_ have good coverage for these symbols, however, and which even have pretty good coverage of these symbols being used invalidly: the Python files embedded inside our mdtests! Running the `PullTypesVisitor` over all snippets in our Markdown files should give us a much higher level of confidence that we're not forgetting to store types for any subexpressions in any files.

This PR adds a step to mdtest that runs the `PullTypesVisitor` over every synthesized file created by the mdtest framework. There are six existing mdtest suites that fail this new step; these Markdown files have been added to a `KNOWN_PULL_TYPES_FAILURES` list in the `ty_test` crate.

## Test Plan

`cargo test`


---

_Label `testing` added by @AlexWaygood on 2025-06-07 18:33_

---

_Label `ty` added by @AlexWaygood on 2025-06-07 18:33_

---

_@AlexWaygood reviewed on 2025-06-07 18:38_

---

_Review comment by @AlexWaygood on `crates/ty_test/src/lib.rs`:300 on 2025-06-07 18:38_

I tried for quite a while to use `catch_unwind` like we already do in `ty_project`:

https://github.com/astral-sh/ruff/blob/86e5a311f05943095f8e7480dea1a69fdc8c787e/crates/ty_project/tests/check.rs#L132-L151

The advantage is that if a file starts passing, we're forced to remove it from `KNOWN_PULL_TYPES_FAILURES`.

I couldn't get similar logic to work here, however, and I couldn't figure out why. Even when the `pull_types` function was panicking on a given file, the surrounding `catch_unwind` call appeared to be returning an `Ok()` variant for some reason.

---

_Comment by @github-actions[bot] on 2025-06-07 18:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-06-07 18:48_

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

_Marked ready for review by @AlexWaygood on 2025-06-07 18:50_

---

_Review requested from @carljm by @AlexWaygood on 2025-06-07 18:50_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-07 18:50_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-07 18:50_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-06-07 18:50_

---

_Review comment by @MichaReiser on `crates/ty_project/src/pull_types_visitor.rs`:5 on 2025-06-09 14:44_

I would probably move this to the semantic crate. It doesn't depend on any project related behavior. In fact, it's very tight to how our semantic model works 

---

_Review comment by @MichaReiser on `crates/ty_test/src/lib.rs`:300 on 2025-06-09 14:46_

I'd have to debug this. Is there any inner or outer catch unwind? 

---

_Review comment by @MichaReiser on `crates/ty_test/src/lib.rs`:473 on 2025-06-09 14:48_

It would be nice if we had a specific syntax in mdtests instead of maintaining this list (of tests that this crate doesn't depend on).

It has the advantage that I can see that this test isn't fully functioning when editing the md test file

---

_@MichaReiser reviewed on 2025-06-09 14:48_

This is a great idea!

Let me know if I should take a look at the catch unwind thingy

---

_@MichaReiser approved on 2025-06-09 14:48_

---

_@AlexWaygood reviewed on 2025-06-09 15:58_

---

_Review comment by @AlexWaygood on `crates/ty_test/src/lib.rs`:473 on 2025-06-09 15:58_

That's a great idea. It would enable us to more easily narrow down which specific subsection of the Markdown file is causing panics when types are pulled, too!

---

_@AlexWaygood reviewed on 2025-06-09 15:59_

---

_Review comment by @AlexWaygood on `crates/ty_project/src/pull_types_visitor.rs`:5 on 2025-06-09 15:59_

makes sense

---

_@AlexWaygood reviewed on 2025-06-09 17:22_

---

_Review comment by @AlexWaygood on `crates/ty_project/src/pull_types_visitor.rs`:5 on 2025-06-09 17:22_

Should we just move the whole corpus test (and the existing corpus of Python code itself) into the `ty_python_semantic` crate? It's usually changes to `ty_python_semantic` that break those tests. I've been annoyed more than once that running `cargo test -p ty_python_semantic` has passed locally for a PR branch, only for tests in `ty_project` (a crate my PR didn't touch!) to fail in CI.

---

_@carljm reviewed on 2025-06-09 17:26_

---

_Review comment by @carljm on `crates/ty_project/src/pull_types_visitor.rs`:5 on 2025-06-09 17:26_

If we are moving the visitor anyway, and there aren't other dependencies that make this move difficult, then I would say yes, let's move it all into `ty_python_semantic`.

---

_@MichaReiser reviewed on 2025-06-09 17:34_

---

_Review comment by @MichaReiser on `crates/ty_project/src/pull_types_visitor.rs`:5 on 2025-06-09 17:34_

I think the only reason why they're in `ty_project` is because the tests use the `ProjectDatabase`. 

---

_@AlexWaygood reviewed on 2025-06-10 09:57_

---

_Review comment by @AlexWaygood on `crates/ty_project/src/pull_types_visitor.rs`:5 on 2025-06-10 09:57_

Yes, it looks like moving the test to `ty_python_semantic` would at least require a little refactoring (I gave it a quick try yesterday). For now I'll leave the test where it is so as not to increase the scope of this PR any further, but I can open a followup issue for moving the test and the corpus itself to `ty_python_semantic`

---

_Review comment by @MichaReiser on `crates/ty_test/src/lib.rs`:300 on 2025-06-10 10:38_

I think the problem here is that `KNOWN_PULL_TYPES_FAILURE` is at the mdtest file level, but `filter_map` runs for every test inside the MDTEST file (but pull types doesn't panic for all tests in that file)

---

_@MichaReiser reviewed on 2025-06-10 10:38_

---

_@AlexWaygood reviewed on 2025-06-10 11:17_

---

_Review comment by @AlexWaygood on `crates/ty_test/src/lib.rs`:300 on 2025-06-10 11:17_

Ahhh, that makes sense. So https://github.com/astral-sh/ruff/pull/18539#discussion_r2135867389 might fix this too

---

_Converted to draft by @AlexWaygood on 2025-06-11 17:55_

---

_Comment by @AlexWaygood on 2025-06-11 19:53_

Okay, this has now been reimplemented as a "proper mdtest feature", that can be disabled on a per-test basis by adding the `<!-- pull-types:skip -->` directive to a test.

This is what it looks like if the step fails with an unexpected error:

![image](https://github.com/user-attachments/assets/3cfdfdb8-5a8f-4ce6-9825-9d9c39df70d9)

And this is what an unexpected success looks like:

![image](https://github.com/user-attachments/assets/47d01bd4-454f-4860-b89e-a189aa14302c)




---

_Marked ready for review by @AlexWaygood on 2025-06-11 19:53_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-06-11 19:53_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/Cargo.toml`:53 on 2025-06-12 06:00_

Huh, I'm surprised that a crate is allowed to depend on itself. 

---

_@MichaReiser reviewed on 2025-06-12 06:00_

---

_@MichaReiser approved on 2025-06-12 06:03_

---

_Merged by @AlexWaygood on 2025-06-12 09:32_

---

_Closed by @AlexWaygood on 2025-06-12 09:32_

---

_Branch deleted on 2025-06-12 09:32_

---

```yaml
number: 13746
title: "[red-knot] feat: Inference for `BytesLiteral` comparisons"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/infer-bytes-literal-comparisons
created_at: 2024-10-14T10:23:17Z
updated_at: 2024-10-14T18:26:23Z
url: https://github.com/astral-sh/ruff/pull/13746
synced_at: 2026-01-12T15:55:45Z
```

# [red-knot] feat: Inference for `BytesLiteral` comparisons

---

_@sharkdp_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implements inference for `BytesLiteral` comparisons along the lines of https://github.com/astral-sh/ruff/pull/13634. A refactoring that unifies some code with what we do for `StringLiteral` (and possibly other cases, see also #13712) is planned for a later PR.

closes #13687

## Test Plan

Added tests using the new Markdown framework. I added the tests in `mdtest/comparison/byte_literals.md`, following the convention proposed in [this comment](https://github.com/astral-sh/ruff/pull/13712#discussion_r1797197213). This is somewhat in conflict with the proposed structure in #13719 which puts the `StringLiteral` tests under `mdtest/boolean.md`, so this will have to be sorted out after we agreed on a naming/grouping convention for these tests.

---

_Label `red-knot` added by @sharkdp on 2024-10-14 10:23_

---

_Review requested from @carljm by @sharkdp on 2024-10-14 10:23_

---

_Review requested from @MichaReiser by @sharkdp on 2024-10-14 10:23_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-10-14 10:23_

---

_Comment by @github-actions[bot] on 2024-10-14 10:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/byte_literals.md`:1 on 2024-10-14 10:39_

nit: it would be nice to have maybe a sentence of documentation here that explains what the purpose of theses tests is. Even though for this file in particular it's kind of self-evident, I think it's useful to establish a convention here:

```suggestion
### Comparison: Byte literals

These tests assert that we infer precise `Literal` types for comparisons between objects
inferred as having `Literal` bytes types:
```

---

_@AlexWaygood approved on 2024-10-14 10:39_

Nice!

---

_Comment by @AlexWaygood on 2024-10-14 10:40_

> ℹ️ ecosystem check **detected linter changes**. (+0 -1693 violations, +0 -0 fixes in 5 projects; 49 projects unchanged)

Um, you can ignore that. It reports some spurious results sometimes when a PR branch is a bit behind `main` (we should probably fix it at some point 😄)

There's no way this PR could have caused any of those changes, so it can be safely disregarded here!

---

_@T-256 reviewed on 2024-10-14 11:15_

---

_Review comment by @T-256 on `crates/red_knot_python_semantic/resources/mdtest/comparison/byte_literals.md`:8 on 2024-10-14 11:15_

Does it work without `reveal_type`?
```suggestion
b"abc" == b"abc"  # revealed: Literal[True]
b"abc" == b"ab"   # revealed: Literal[False]
```
Is comment aligning part of mdtest's design guide? (is it allowed or preferred?)

---

_@AlexWaygood reviewed on 2024-10-14 11:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comparison/byte_literals.md`:8 on 2024-10-14 11:19_

> Does it work without `reveal_type`?

no, the `reveal_type` call is necessary to trigger a "Revealed" assertion when using `mdtest`-based tests. This is documented [here](https://github.com/astral-sh/ruff/tree/main/crates/red_knot_test#assertions).

> Is comment aligning part of mdtest's design guide? (is it allowed or preferred?)

The comment alignment seems fine to me. At some point maybe we might start running our `scripts/check_docs_formatted.py` CI script on our `mdtest` test files. But until we do that, or introduce autoformatting of these Python snippets some other way, I don't think we should be too picky about the formatting of the Python snippets in our test files.

---

_Merged by @sharkdp on 2024-10-14 12:01_

---

_Closed by @sharkdp on 2024-10-14 12:01_

---

_Branch deleted on 2024-10-14 12:01_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/comparison/byte_literals.md`:8 on 2024-10-14 12:12_

This is probably something we have to align on. I recommend to not rely on comment alignment because it's something that will break if we ever decide to format our example code using ruff (which I think would be a nice addition). 



---

_@MichaReiser reviewed on 2024-10-14 12:12_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:2675 on 2024-10-14 12:14_

Nit: You could have used `&**salsa_b1.value(self.db)` to aovid the `as_ref`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:2664 on 2024-10-14 12:18_

Nit: We could consider using https://docs.rs/memchr/latest/memchr/memmem/fn.find.html

---

_@MichaReiser reviewed on 2024-10-14 12:18_

This is great! I've only two nits. Up to you if you want to follow up on them or not.

---

_@sharkdp reviewed on 2024-10-14 12:44_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:2664 on 2024-10-14 12:44_

Yes, thanks. Did a grep for `memmem` , didn't find anything, and wasn't sure if it warranted adding a new dependency. Should have looked for `memchr`.

==> #13750 

---

_Comment by @carljm on 2024-10-14 18:26_

This is excellent, thank you!!

---

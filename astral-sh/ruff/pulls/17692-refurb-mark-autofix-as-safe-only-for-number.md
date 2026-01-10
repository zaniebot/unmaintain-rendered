```yaml
number: 17692
title: "[`refurb`] Mark autofix as safe only for number literals in `FURB116`"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-FURB116pt2
created_at: 2025-04-28T21:31:15Z
updated_at: 2025-05-12T20:22:14Z
url: https://github.com/astral-sh/ruff/pull/17692
synced_at: 2026-01-10T18:51:01Z
```

# [`refurb`] Mark autofix as safe only for number literals in `FURB116`

---

_Pull request opened by @LaBatata101 on 2025-04-28 21:31_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
We can only guarantee the safety of the autofix for number literals, all other cases may change the runtime behaviour of the program or introduce a syntax error. For the cases reported in the issue that would result in a syntax error,  I disabled the autofix.

Follow-up of #17661. 

Fixes #16472.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Snapshot tests.
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-04-28 21:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dscorbett on 2025-04-28 21:44_

For `bin({0: 1}[0].numerator)[2:]`, it would be better to insert a space before the brace in the unsafe fix, as in `f"{ {0: 1}[0].numerator:b}"`, which is how `ruff format` formats it, instead of disabling the fix.

---

_Comment by @LaBatata101 on 2025-04-28 21:51_

> For `bin({0: 1}[0].numerator)[2:]`, it would be better to insert a space before the brace in the unsafe fix, as in `f"{ {0: 1}[0].numerator:b}"`, which is how `ruff format` formats it, instead of disabling the fix.

Good idea, thanks!

---

_@dscorbett reviewed on 2025-04-28 22:21_

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:161 on 2025-04-28 22:21_

To check for multiple lines, `find_newline` is better because it also detects `'\r'`.

---

_@LaBatata101 reviewed on 2025-04-29 12:55_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:161 on 2025-04-29 12:55_

Thanks for the remainder!

---

_Label `bug` added by @ntBre on 2025-05-07 21:06_

---

_Label `fixes` added by @ntBre on 2025-05-07 21:06_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:32 on 2025-05-07 21:13_

We actually have a third `Applicability` level that I haven't seen used much but might be relevant here called `DisplayOnly`:

https://github.com/astral-sh/ruff/blob/33b75e6c6d0d149b8d09625b715f28fde7055573/crates/ruff_diagnostics/src/fix.rs#L13-L16

We might need to use that if there's a good chance we're going to introduce a syntax error.


---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:130 on 2025-05-07 21:15_

We might be able to use `ResolvedPythonType` like in this related `FURB161` PR instead of comparing against number literals precisely: https://github.com/astral-sh/ruff/pull/17240.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:187 on 2025-05-07 21:27_

I can't think of a precise counterexample right now, but this is reminding me of some subtle issue from when I was working on quoting a couple of months ago. Maybe this can cause problems if `arg` also contains a single quote? You could try a test case with something wild like this:

```python
""" ''' " ' x ' " ''' """
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:191 on 2025-05-07 21:29_

Does this need to be a `contains` check, or is it only a problem if it starts with a `{`? It seems like braces could cause other problems when injected into an f-string, but I guess that issue might already be present in the current rule.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB116_FURB116.py.snap`:15 on 2025-05-07 21:35_

Yeah I think if we're able to resolve the type, this can still be a safe fix.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB116_FURB116.py.snap`:189 on 2025-05-07 21:38_

It looks like this comment is outdated since we get a fix here

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB116_FURB116.py.snap`:210 on 2025-05-07 21:39_

We might want to add a variant of this test with a pre-3.11 version to capture this.

---

_@ntBre reviewed on 2025-05-07 21:44_

Thanks! This looks reasonable to me, just a few fairly minor suggestions.

I think it might be good for @MichaReiser to take a look at the `try_create_replacement` code too, especially the quoting and f-string handling since he helped find a bunch of issues in my handling of those recently.

Do we need to have a check for negative numbers? That seems like the only remaining part of the issue not covered, unless I missed it.

---

_@LaBatata101 reviewed on 2025-05-07 22:36_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:191 on 2025-05-07 22:36_

It was supposed to be `starts_with`, thanks!

---

_@LaBatata101 reviewed on 2025-05-07 22:46_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:130 on 2025-05-07 22:46_

`ResolvedPythonType` resolves the type of identifiers to `Unknown`, so comparing against number literals or using `ResolvedPythonType` is the same thing.

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB116_FURB116.py.snap`:210 on 2025-05-07 22:50_

How do I make a test for a specific version?

---

_@LaBatata101 reviewed on 2025-05-07 22:50_

---

_@LaBatata101 reviewed on 2025-05-07 23:00_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:187 on 2025-05-07 23:00_

Yeah, with your example, I got a syntax error in Python 3.11. So, what is the solution here? Disabling the fix if `arg` has the same quote as the f-string?

---

_@dscorbett reviewed on 2025-05-08 03:04_

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:130 on 2025-05-08 03:04_

Also, the fix is only safe for non-negative numbers. Resolving the expression to a `ResolvedPythonType::Atom(PythonType::Number(NumberLike::Integer))` wouldn’t be enough, because the value could be negative. Resolving it to a `ResolvedPythonType::Atom(PythonType::Number(NumberLike::Bool))` _would_ be enough to prove the fix is safe, though that case is so rare it may not be worth handling.

---

_@LaBatata101 reviewed on 2025-05-08 20:19_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:130 on 2025-05-08 20:19_

Negative numbers are represented by the `UnaryOp` AST node, so we don't need to use `ResolvedPythonType` here. That check is enough to mark the fix as unsafe for negative numbers.  I'll just have to handle numbers with the `+` unary operator.

---

_@LaBatata101 reviewed on 2025-05-08 20:21_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:32 on 2025-05-08 20:21_

I think using `DisplayOnly` makes sense here.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:187 on 2025-05-08 21:02_

That could be one option. I think we could also use the `Generator`, which should handle escaping quotes for us. I'm not sure how difficult it would be to convert  this to use the generator though.

---

_@ntBre reviewed on 2025-05-08 21:02_

---

_@ntBre reviewed on 2025-05-08 21:04_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB116_FURB116.py.snap`:210 on 2025-05-08 21:04_

You'd probably have to copy this test and override the `unresolved_target_version` setting in `LinterSettings`:

https://github.com/astral-sh/ruff/blob/981bd70d393fe29324c07c3ffce5ecddd40e6bd6/crates/ruff_linter/src/rules/refurb/mod.rs#L54-L62

---

_@ntBre reviewed on 2025-05-08 21:07_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:130 on 2025-05-08 21:07_

I always forget that `ResolvedPythonType` doesn't do the same thing as the helpers in the `typing` module:

https://github.com/astral-sh/ruff/blob/981bd70d393fe29324c07c3ffce5ecddd40e6bd6/crates/ruff_python_semantic/src/analyze/typing.rs#L980-L983

Something like this is closer to what I had in mind. 

---

_@LaBatata101 reviewed on 2025-05-08 22:05_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:187 on 2025-05-08 22:05_

I tried using `Generator` but it didn't work, still got a `SyntaxError`. I'm going to disable the fix for that case.

---

_@LaBatata101 reviewed on 2025-05-08 22:48_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:130 on 2025-05-08 22:48_

The only problem is that if we have a variable, and it resolves the type to `int`, we can't know if the integer is positive or negative.

---

_@MichaReiser reviewed on 2025-05-09 06:42_

I skimmed over it and this looks good to me. 

I suspect there might be some edge cases with f-strings if the expression is in a format spec or debug expression but these seem rare enough (and also isn't something we handle in other fixes)

---

_@ntBre reviewed on 2025-05-09 18:16_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:130 on 2025-05-09 18:16_

Ah I see what you mean, I was thinking it was only a problem for negative literals. I think this and my comment below on the test are both resolved. Thanks!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:193 on 2025-05-09 18:21_

Do we need the same check for single  quotes? I think we might be  able to do something like this:

```rust
if checker.target_version() <= PythonVersion::PY311 && inner_source.contains(quote.as_str()) { ... }
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:193 on 2025-05-09 18:22_

I did a similar check here, but I think it's fine to do a simple `contains` check instead:

https://github.com/astral-sh/ruff/blob/43b905bad805e4b6aeaa4fd5e208f9c4e00bbde2/crates/ruff_python_parser/src/parser/expression.rs#L1460-L1468

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB116_FURB116.py.snap`:204 on 2025-05-09 18:26_

Should this comment say display-only too?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB116_FURB116.py.snap`:204 on 2025-05-09 18:29_

Oh, this is one of the 3.11 cases right?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:32 on 2025-05-09 18:35_

Should we update this to say "display-only" instead of "unsafe"? We could probably expand this with some of the examples of display-only cases, but I don't feel strongly about that.

---

_@ntBre reviewed on 2025-05-09 18:36_

Looking good! Just one question about quotes and two about docs/comments.

---

_@LaBatata101 reviewed on 2025-05-09 19:36_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:193 on 2025-05-09 19:36_

> Do we need the same check for single quotes? I think we might be able to do something like this:
> 
> ```rust
> if checker.target_version() <= PythonVersion::PY311 && inner_source.contains(quote.as_str()) { ... }
> ```

Yeah, I'll update the check.

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/refurb/rules/fstring_number_format.rs`:32 on 2025-05-09 19:42_

Forgot to update that.

---

_@LaBatata101 reviewed on 2025-05-09 19:42_

---

_@LaBatata101 reviewed on 2025-05-09 19:44_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB116_FURB116.py.snap`:204 on 2025-05-09 19:44_

This snapshot is for the latest version of Python, I think. The snapshot for python 3.11 is [this one](https://github.com/astral-sh/ruff/pull/17692/files/99b828fd0118a92a6887a28027c7073326ebf9e3..abdb2a9be64207a7bc589ac32086a984335535a4#diff-fef76f77d4f5ad382b778fd2d8b56863a4ad546181ebd0bc2e8c869d9a5c0aef), they share the same source file, so I'll just update the comment to state there's no autofix for versions 3.11 and earlier.

---

_Review requested from @ntBre by @LaBatata101 on 2025-05-09 19:48_

---

_@ntBre approved on 2025-05-12 20:07_

Thanks!

---

_Merged by @ntBre on 2025-05-12 20:08_

---

_Closed by @ntBre on 2025-05-12 20:08_

---

_Branch deleted on 2025-05-12 20:22_

---

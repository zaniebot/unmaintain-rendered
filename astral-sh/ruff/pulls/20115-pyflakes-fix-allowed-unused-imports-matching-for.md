```yaml
number: 20115
title: "[`pyflakes`] Fix `allowed-unused-imports` matching for top-level modules (`F401`)"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - bug
assignees: []
merged: true
base: main
head: allowed-unused-imports-top-level-module
created_at: 2025-08-27T15:54:56Z
updated_at: 2025-08-28T13:09:45Z
url: https://github.com/astral-sh/ruff/pull/20115
synced_at: 2026-01-12T15:56:54Z
```

# [`pyflakes`] Fix `allowed-unused-imports` matching for top-level modules (`F401`)

---

_@TaKO8Ki_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #19664

Fix allowed unused imports matching for top-level modules.

I've simply replaced `from_dotted_name` with `user_defined`. Since QualifiedName for imports is created in crates/ruff_python_semantic/src/imports.rs, I guess it's acceptable to use `user_defined` here. Please tell me if there is better way.

https://github.com/astral-sh/ruff/blob/0c5089ed9e180da7bc76733ffe1a1d132e9142b0/crates/ruff_python_semantic/src/imports.rs#L62

## Test Plan

<!-- How was it tested? -->

I've added a snapshot test `f401_allowed_unused_imports_top_level_module`.


---

_Label `bug` added by @ntBre on 2025-08-27 17:24_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_35.py`:7 on 2025-08-27 17:30_

Should we add a case that's just `import hvplot`? I see by comparing to the [playground](https://play.ruff.rs/aafcb007-2f0f-45f9-b75e-a5d46732e7a0) that your change is having the intended effect, but that might be a nice one to throw in since it's closest to the original report.

Based on another [quirk](https://github.com/astral-sh/ruff/issues/4656) of F401, and as you can also see on the playground, only a couple of these actually trigger F401 on `main`. We might have to wrap each test case in a function so that F401 actually tries to trigger in each case:

```py
def f(): import hvplot
def f(): import hvplot.pandas
# ...
```

---

_@ntBre reviewed on 2025-08-27 17:36_

Thank you! I just had  a couple of suggestions about the tests, but this looks great to me.

Do you want to test if this fixes https://github.com/astral-sh/ruff/issues/15705 as well? There's no need to extend the PR if not, I just thought it sounded related and had a chance of fixing it already.

---

_Comment by @github-actions[bot] on 2025-08-27 17:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @TaKO8Ki on 2025-08-28 00:35_

@ntBre 

Thank you for the review. I have added a simple test case `import hvplot` and wrapped all test cases in a function.

> Do you want to test if this fixes https://github.com/astral-sh/ruff/issues/15705 as well? There's no need to extend the PR if not, I just thought it sounded related and had a chance of fixing it already.

I want to try it and I guess it takes some time to fix it, so can I work on it in a different pull request?

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_35.py`:12 on 2025-08-28 02:01_

Sorry, I actually meant to wrap each one in a separate function:


```suggestion
def f():
    import hvplot
def f():
    import hvplot.pandas
def f():
    import hvplot.pandas.plots
def f():
    from hvplot.pandas import scatter_matrix
def f():
    from hvplot.pandas.plots import scatter_matrix
```

Otherwise each import interferes with the others, like in the first example reported in #4656. Wrapping each import in its own function ensures that the `allowed-unused-import` setting allows all of these cases.

---

_@ntBre reviewed on 2025-08-28 02:03_

Looks great, just one clarification on the test.

> I want to try it and I guess it takes some time to fix it, so can I work on it in a different pull request?

That sounds good! I think your fix here might resolve that issue too without any other code changes, but it makes sense to add a test in a separate PR even so. No pressure to work on this, just an idea :)

---

_@TaKO8Ki reviewed on 2025-08-28 02:20_

---

_Review comment by @TaKO8Ki on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_35.py`:12 on 2025-08-28 02:20_

I see. I have added separate functions.

---

_@ntBre approved on 2025-08-28 12:59_

Thank you!

---

_Renamed from "[`pyflakes`] Fix allowed unused imports matching for top-level modules" to "[`pyflakes`] Fix `allowed-unused-imports` matching for top-level modules (`F401`)" by @ntBre on 2025-08-28 13:01_

---

_Merged by @ntBre on 2025-08-28 13:02_

---

_Closed by @ntBre on 2025-08-28 13:02_

---

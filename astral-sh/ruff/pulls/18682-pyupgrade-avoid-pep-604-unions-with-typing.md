```yaml
number: 18682
title: "[`pyupgrade`] Avoid PEP-604 unions with `typing.NamedTuple` (`UP007`, `UP045`)"
type: pull_request
state: merged
author: robsdedude
labels:
  - rule
assignees: []
merged: true
base: main
head: fix/18619-up007-named-tuple
created_at: 2025-06-15T11:08:37Z
updated_at: 2025-06-30T22:18:13Z
url: https://github.com/astral-sh/ruff/pull/18682
synced_at: 2026-01-10T18:39:08Z
```

# [`pyupgrade`] Avoid PEP-604 unions with `typing.NamedTuple` (`UP007`, `UP045`)

---

_Pull request opened by @robsdedude on 2025-06-15 11:08_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
Make `UP045` ignore `Optional[NamedTuple]` as `NamedTuple` is a function (not a proper type). Rewriting it to `NamedTuple | None` breaks at runtime. While type checkers currently accept `NamedTuple` as a type, they arguably shouldn't. Therefore, we outright ignore it and don't touch or lint on it.

For a more detailed discussion, see the linked issue.

## Test Plan
Added examples to the existing tests.

## Related Issues
Fixes: https://github.com/astral-sh/ruff/issues/18619


---

_Comment by @github-actions[bot] on 2025-06-15 11:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @robsdedude on 2025-06-15 11:46_

---

_Label `rule` added by @ntBre on 2025-06-15 17:19_

---

_Review requested from @ntBre by @ntBre on 2025-06-15 17:21_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP007.py`:1 on 2025-06-16 19:42_

This is getting very slightly ahead of ourselves, but we're stabilizing UP045 and the [preview](https://docs.astral.sh/ruff/rules/non-pep604-annotation-union/#preview) behavior of UP007 in the upcoming release, which is taking place tomorrow. So could you update the tests in this file only to test the `Union` behavior? More generally,  do we need to add tests for `Union`? I'm assuming an error is raised for `NamedTuple | int`, for example, not just `NamedTuple | None`.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP007.py`:5 on 2025-06-16 19:44_

nit: I know it's not a good way to format real Python code, but I generally prefer adding new imports at the bottom of the file too, just to avoid shifting all of the existing snapshots and making the diff larger. Another option is moving the existing test file to a name like `UP007_0.py` and adding an entirely new fixture called `UP007_1.py` to accompany it.

---

_@ntBre reviewed on 2025-06-16 19:45_

---

_Review requested from @ntBre by @robsdedude on 2025-06-24 19:28_

---

_Renamed from "[`pyupgrade`] Fix `UP007`: ignore `Optional[NamedTuple]`" to "[`pyupgrade`] Fix `UP045`: ignore `Optional[NamedTuple]`" by @robsdedude on 2025-06-24 19:29_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/use_pep604_annotation.rs`:138 on 2025-06-26 18:47_

nit: I think this might make sense as the docstring for `is_optional_named_tuple` instead of here in the middle of the code.

---

_@ntBre reviewed on 2025-06-26 18:53_

Thanks, I think this looks good for UP045. However, we have the same problem for UP007, which is actually the rule from the initial report. Maybe my earlier comments were a bit confusing, but we need to handle both UP045, which replaces `Optional[NamedTuple]` with `NamedTuple | None` _and_ UP007, which replaces `Union[NamedTuple, int]` (for example) with `NamedTuple | int`, which is also an error.

https://play.ruff.rs/d0e71fc1-5210-4eb0-a0a5-e37b01fb6b11

---

_Review requested from @ntBre by @robsdedude on 2025-06-29 13:36_

---

_@ntBre approved on 2025-06-30 21:21_

Thanks!

---

_Renamed from "[`pyupgrade`] Fix `UP045`: ignore `Optional[NamedTuple]`" to "[`pyupgrade`] Avoid PEP-604 unions with `typing.NamedTuple` (`UP007`, `UP045`)" by @ntBre on 2025-06-30 21:21_

---

_Merged by @ntBre on 2025-06-30 21:22_

---

_Closed by @ntBre on 2025-06-30 21:22_

---

_Branch deleted on 2025-06-30 22:18_

---

```yaml
number: 16263
title: "[`pylint`] Fix false positives, add missing methods, and support positional-only parameters (`PLE0302`)"
type: pull_request
state: merged
author: dcarrier
labels:
  - bug
assignees: []
merged: true
base: main
head: issue-16217
created_at: 2025-02-19T19:35:50Z
updated_at: 2025-02-25T17:30:21Z
url: https://github.com/astral-sh/ruff/pull/16263
synced_at: 2026-01-12T15:55:54Z
```

# [`pylint`] Fix false positives, add missing methods, and support positional-only parameters (`PLE0302`)

---

_@dcarrier_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves 3/4 requests in #16217:

- ✅ Remove not special methods: `__cmp__`, `__div__`, `__nonzero__`, and `__unicode__`.
- ✅ Add special methods: `__next__`, `__buffer__`, `__class_getitem__`, `__mro_entries__`, `__release_buffer__`, and `__subclasshook__`.
- ✅ Support positional-only arguments.
- ❌ Add support for module functions `__dir__` and `__getattr__`. As mentioned in the issue the check is scoped for methods rather than module functions. I am hesitant to expand the scope of this check without a discussion.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
- Manually confirmed each example file from the issue functioned as expected.
- Ran cargo nextest to ensure `unexpected_special_method_signature` test still passed.

Fixes #16217.

---

_Comment by @github-actions[bot] on 2025-02-19 19:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-02-20 03:51_

---

_Comment by @MichaReiser on 2025-02-20 08:50_

@ntBre do you want to take this one?

---

_Comment by @ntBre on 2025-02-20 12:16_

Sure!

---

_Assigned to @ntBre by @ntBre on 2025-02-20 12:16_

---

_@ntBre requested changes on 2025-02-20 15:17_

Thanks for working on this! The code changes look right to me, but I think it would be great to paste in all of the examples from the issue and add their snapshot results just to make sure.

I definitely agree that the module functions are good to leave for a separate rule, so this will be perfect with a few more tests.

---

_Comment by @dcarrier on 2025-02-20 19:29_

Thank you for the review! I have added the test cases along with the snapshots as requested.

Side note, we are asserting `__cmp__`, `__div__`, `__nonzero__`, and `__unicode__` are skipped by including failure cases which do not generate a snapshot.

---

_Review requested from @ntBre by @dcarrier on 2025-02-20 19:31_

---

_@ntBre approved on 2025-02-20 20:10_

Nice, this looks great. Thanks again!

---

_Renamed from "[pylint] fix false positives, add missing methods and support posonlyargs (PLE0302)" to "[`pylint`] Fix false positives, add missing methods, and support positional-only parameters (`PLE0302`)" by @ntBre on 2025-02-20 20:12_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pylint/unexpected_special_method_signature.py`:74 on 2025-02-20 20:18_

Oh I missed this on the first review, but we actually still ignore these right? I'm not seeing any snapshots for these lines. I would just revert these two changes, if you don't mind.

Also, this isn't from your changes, but the second comment is actually wrong, the ones with the `*` are keyword-only not positional-only like the first one.

~~I guess you could also update the first one to say `ignore positional-only args with defaults` because that looks like the reason we're still skipping it.~~

---

_@ntBre reviewed on 2025-02-20 20:18_

---

_@ntBre reviewed on 2025-02-20 20:29_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pylint/unexpected_special_method_signature.py`:74 on 2025-02-20 20:29_

Oh I think I'm wrong about that last part, I forgot that the positional-only arguments come *before* the `/` :sweat_smile: 

So I think you're right that we do support the positional arguments! I was also wrong that we'd skip it for having defaults. I misread the `filter` code below.

I believe we *do* still ignore keyword-only parameters (line 150 below), so that part of my comment is still reasonable.

---

_@dcarrier reviewed on 2025-02-21 03:00_

---

_Review comment by @dcarrier on `crates/ruff_linter/resources/test/fixtures/pylint/unexpected_special_method_signature.py`:74 on 2025-02-21 03:00_

You are correct, good catch. I have updated the comment to note that we are properly skipping keyword-only args for the `*` `__eq__` test.

---

_Merged by @ntBre on 2025-02-21 13:38_

---

_Closed by @ntBre on 2025-02-21 13:38_

---

_Branch deleted on 2025-02-25 17:30_

---

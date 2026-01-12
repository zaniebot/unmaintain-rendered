```yaml
number: 19440
title: "[`flake8-use-pathlib`] Expand `PTH201` to check all `PurePath` subclasses"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-19437
created_at: 2025-07-20T16:57:50Z
updated_at: 2025-08-01T02:37:54Z
url: https://github.com/astral-sh/ruff/pull/19440
synced_at: 2026-01-12T15:56:39Z
```

# [`flake8-use-pathlib`] Expand `PTH201` to check all `PurePath` subclasses

---

_@danparizher_

## Summary

Fixes #19437


---

_Comment by @github-actions[bot] on 2025-07-20 17:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @ntBre on 2025-07-20 23:04_

---

_Review requested from @ntBre by @ntBre on 2025-07-20 23:04_

---

_Renamed from "[`flake8_use_pathlib`] Expand PTH201 to check all PurePath subclasses" to "[`flake8-use-pathlib`] Expand `PTH201` to check all `PurePath` subclasses" by @ntBre on 2025-07-20 23:04_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/path_constructor_current_directory.rs`:79 on 2025-07-28 19:43_

It seems we have a couple of different existing checks that are similar to this:

https://github.com/astral-sh/ruff/blob/e867830848f6cc996186ecaf6570c43a56c0cf2c/crates/ruff_python_semantic/src/analyze/typing.rs#L1091-L1093

which will check `Binding`s and includes all of these except `PackagePath`, and

https://github.com/astral-sh/ruff/blob/e867830848f6cc996186ecaf6570c43a56c0cf2c/crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs#L14-L21

which only checks for `pathlib.Path`. I don't think we can switch all of the current calls to one or the other, but we might at least want to factor this code into a helper function (`is_pure_path_subclass` maybe) so that we can use it in the other PTH rules, if appropriate. 

cc @chirizxc who has been working on PTH autofixes

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_use_pathlib/PTH201.py`:14 on 2025-07-28 19:58_

Can we move these and the new imports to the bottom of the file? Adding them here has created quite a large diff for a file that should mostly be the same.

---

_@ntBre reviewed on 2025-07-28 20:00_

Thanks! I had one nit about the tests that will make this much easier to review, and then a suggestion about code reuse. I think factoring this out into a helper function would be fine for now, and then we could create a follow-up issue to check the other PTH rules.

---

_Review requested from @ntBre by @danparizher on 2025-07-30 00:52_

---

_Comment by @danparizher on 2025-07-30 00:57_

It looks like the overall snapshot diff is still rather large because of the additional line from importing `PackagePath`. Would it be best to squish that onto one line in this case?

---

_Comment by @ntBre on 2025-07-30 14:45_

I would just move the new imports to the bottom too, with the new code. It's obviously not good Python style in general, but it works well for our tests :) Or we could split out a separate test file, that works too.

---

_@ntBre approved on 2025-07-30 21:49_

Thanks!

---

_Comment by @ntBre on 2025-07-30 21:53_

It's a bit annoying, but I'm wondering if we need to preview-gate this. There aren't any new ecosystem hits, but it's still a pretty big expansion to the rule. I guess we should put it in preview, just to be safe.

---

_Review requested from @ntBre by @danparizher on 2025-07-31 00:48_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/mod.rs`:166 on 2025-07-31 16:54_

Could this just be a test case here:

https://github.com/astral-sh/ruff/blob/a3f28baab4fc085e1448484be23df7036d13191d/crates/ruff_linter/src/rules/flake8_use_pathlib/mod.rs#L130-L132

---

_@ntBre approved on 2025-07-31 16:56_

Looks good, just one nit about the test location.

---

_@danparizher reviewed on 2025-07-31 22:58_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/flake8_use_pathlib/mod.rs`:166 on 2025-07-31 22:58_

Oh yes, I have no idea how this slipped by me 😂

---

_Review requested from @ntBre by @danparizher on 2025-07-31 23:13_

---

_@ntBre approved on 2025-08-01 02:17_

---

_Label `preview` added by @ntBre on 2025-08-01 02:17_

---

_Merged by @ntBre on 2025-08-01 02:18_

---

_Closed by @ntBre on 2025-08-01 02:18_

---

_Branch deleted on 2025-08-01 02:37_

---

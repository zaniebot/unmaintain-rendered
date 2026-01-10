```yaml
number: 18637
title: "[`flake8_pyi`] Fix `PYI041`'s fix causing TypeError with `None | None | ...`"
type: pull_request
state: merged
author: robsdedude
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix/18298-PYI041-fix-causing-type-error
created_at: 2025-06-11T21:51:00Z
updated_at: 2025-06-24T20:52:11Z
url: https://github.com/astral-sh/ruff/pull/18637
synced_at: 2026-01-10T18:39:08Z
```

# [`flake8_pyi`] Fix `PYI041`'s fix causing TypeError with `None | None | ...`

---

_Pull request opened by @robsdedude on 2025-06-11 21:51_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
Fix `PYI041`'s fix turning `None | int | None | float` into `None | None | float`, which raises a `TypeError` when executed.

The fix consists of making sure that the merged super-type is inserted where the first type that is merged was before.

## Test Plan
Tests have been expanded with examples from the issue.

## Related Issue
Fixes https://github.com/astral-sh/ruff/issues/18298


---

_Comment by @github-actions[bot] on 2025-06-11 21:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @robsdedude on 2025-06-11 22:10_

---

_Review requested from @AlexWaygood by @robsdedude on 2025-06-11 22:10_

---

_Label `bug` added by @ntBre on 2025-06-12 04:06_

---

_Label `fixes` added by @ntBre on 2025-06-12 04:06_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI041.py`:96 on 2025-06-12 10:45_

```suggestion
# fix must not yield `None | None | ...` (TypeError)
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI041.pyi`:76 on 2025-06-12 10:46_

```suggestion
# fix must not yield `None | None | ...` (TypeError)
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:108 on 2025-06-12 10:46_

why is this commented out?

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI041_PYI041.pyi.snap`:33 on 2025-06-12 10:50_

this is a clever way of fixing the issue, but I'm not totally sure that users will like it if the fix ends up reordering unions like this, even when the unions don't contain `None`. I think that would be pretty unexpected if I were a Ruff user.

I'm leaning towards just not providing a fix for the rule if there are multiple `None`s in the union. It feels easy to understand, and well-written code should never have multiple `None`s in a union anyway -- we even have other lints that would already detect that!

---

_@AlexWaygood reviewed on 2025-06-12 10:50_

Thanks!

---

_@robsdedude reviewed on 2025-06-12 11:36_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI041_PYI041.pyi.snap`:33 on 2025-06-12 11:36_

Sure. Your project, your direction. I personally wouldn't be surprised. I've seen ruff fixes go beyond such changes. Further, I'd think of this more as merging elements in the union than reordering them. The PR will put the super-type in the place of the first type it's subsuming.

Anyway, I'll change the PR to just omit the fix. However, I suggest to only suppress the fix if not in a typing only context (as this is only a problem at runtime).

---

_@robsdedude reviewed on 2025-06-12 12:21_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:1 on 2025-06-12 12:21_

While working on this fix, I've noticed that this checker is run before deferred string type annotations are visited and resolved. This means that the lint won't trigger on those. Not sure you'd want that to be changed or whether that's a deliberate design decision. Happy to pour this into an issue to tackle it later if you're interested.

---

_@robsdedude reviewed on 2025-06-12 12:35_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:108 on 2025-06-12 12:35_

Left-over from an earlier approach :innocent: 

---

_Review requested from @AlexWaygood by @robsdedude on 2025-06-12 12:42_

---

_Review requested from @ntBre by @AlexWaygood on 2025-06-17 09:51_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:279 on 2025-06-20 18:30_

Do these get sorted somewhere? It's not obvious to me why just checking the first two would work.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI041_1.py`:111 on 2025-06-20 18:31_

This one is also not obvious to me. Why does this get a fix and `f1` doesn't?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:1 on 2025-06-20 18:37_

Yeah I think it makes sense to follow up on this, thanks! While testing this in the playground, I noticed that PYI016 _does_ apply to string annotations, so you may be able to copy however it handles this.

---

_@ntBre reviewed on 2025-06-20 18:37_

Thanks, I think this looks good overall. I just had a couple of questions to make sure I'm understanding what's going on.

---

_@dscorbett reviewed on 2025-06-20 19:00_

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:279 on 2025-06-20 19:00_

There’s only a problem if the first two are `None`. Other combinations work fine.
```pycon
>>> None | int | None
None | int
>>> int | None | None
int | None
>>> None | None | int
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for |: 'NoneType' and 'NoneType'
```

---

_@ntBre reviewed on 2025-06-20 19:02_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:279 on 2025-06-20 19:02_

Oh gotcha, thanks!

---

_@ntBre reviewed on 2025-06-20 19:03_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI041_1.py`:111 on 2025-06-20 19:03_

The answer to my other question resolved this one too.

---

_@ntBre approved on 2025-06-20 19:04_

Questions resolved! I was just confused, as expected.

---

_Merged by @ntBre on 2025-06-20 19:04_

---

_Closed by @ntBre on 2025-06-20 19:04_

---

_Branch deleted on 2025-06-24 20:52_

---

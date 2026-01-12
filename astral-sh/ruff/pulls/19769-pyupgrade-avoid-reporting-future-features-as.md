```yaml
number: 19769
title: "[`pyupgrade`] Avoid reporting `__future__` features as unnecessary when they are used (`UP010`)"
type: pull_request
state: merged
author: IDrokin117
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix/ruff-19561
created_at: 2025-08-05T18:48:53Z
updated_at: 2025-08-20T19:22:04Z
url: https://github.com/astral-sh/ruff/pull/19769
synced_at: 2026-01-12T15:56:46Z
```

# [`pyupgrade`] Avoid reporting `__future__` features as unnecessary when they are used (`UP010`)

---

_@IDrokin117_

## Summary
Resolves #19561

Fixes the [unnecessary-future-import (UP010)](https://docs.astral.sh/ruff/rules/unnecessary-future-import/) rule to correctly identify when imported __future__ modules are actually used in the code, preventing false positives.

I assume there is no way to check usage in `analyze::statements`, because we don't have any usage bindings for imports. To determine unused imports, we have to fully scan the file to create bindings and then check usage, similar to [unused-import (F401)](https://docs.astral.sh/ruff/rules/unused-import/#unused-import-f401). So, `Rule::UnnecessaryFutureImport` was moved from the `analyze::statements` to the `analyze::deferred_scopes` stage. This caused the need to change the logic of future import handling to a bindings-based approach.

Also, the diagnostic report was changed.
Before
```
  |
1 | from __future__ import nested_scopes, generators
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ UP010
```
after
```
  |
1 | from __future__ import nested_scopes, generators
  |                        ^^^^^^^^^^^^^ UP010
```

I believe this is the correct way, because `generators` may be used, but `nested_scopes` is not.

### Special case
I've found out about some specific case.
```python
from __future__ import nested_scopes

nested_scopes = 1
```
Here we can treat `nested_scopes` as an unused import because the variable `nested_scopes` shadows it and we can safely remove the future import (my fix does it).

But [F401](https://docs.astral.sh/ruff/rules/unused-import/#unused-import-f401) not triggered for such case ([sandbox](https://play.ruff.rs/296d9c7e-0f02-4659-b0c0-78cc21f3de76))
```
from foo import print_function

print_function = 1
```
In my mind, `print_function` here is an unused import and should be deleted (my IDE highlight it). What do you think?

## Test Plan

Added test cases and snapshots:
- Split test file into separate _0 and _1 files for appropriate checks.
- Added test cases to verify fixes when future module are used.


---

_Comment by @github-actions[bot] on 2025-08-05 19:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @ntBre on 2025-08-06 18:21_

---

_Label `bug` added by @ntBre on 2025-08-06 18:21_

---

_Label `rule` added by @ntBre on 2025-08-06 18:21_

---

_Comment by @MichaReiser on 2025-08-11 08:13_

> Also, the diagnostic report was changed.

Note: This requires gating behind preview because it changes the rule's suppression ranges. 

---

_Comment by @ntBre on 2025-08-12 15:56_

Thanks Micha, that makes sense. Hopefully the preview-gating isn't too involved, so I think I'll still take a look at this. Your special case is also very interesting. Apparently assigning to `__future__.print_function` will override it but not assigning to `print_function` alone:

```pycon
>>> from __future__ import print_function
>>> import __future__
>>> __future__.print_function
_Feature((2, 6, 0, 'alpha', 2), (3, 0, 0, 'alpha', 0), 1048576)
>>> print_function = 1
>>> __future__.print_function
_Feature((2, 6, 0, 'alpha', 2), (3, 0, 0, 'alpha', 0), 1048576)
>>> __future__.print_function = 1
>>> __future__.print_function
1
>>> from __future__ import print_function
>>> print_function
1
```

I'm not sure if we need this to be covered by F401, though, since F811 flags it as `redefined-while-unused`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_future_import.rs`:163 on 2025-08-12 16:03_

I think we could use `std::iter::once` here to avoid allocating a `Vec` just to call `into_iter`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_future_import.rs`:118 on 2025-08-12 16:09_

I think it would make more sense to loop over all of the bindings in `scope` and check if it's one of the future names. That looks more like how F401 is structured:

https://github.com/astral-sh/ruff/blob/6f42c0d143460adf833caff139c11e9eaff7752d/crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs#L297-L306

and seems like it should be a bit cheaper too. Plus, it will remove a level of indentation here.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_future_import.rs`:120 on 2025-08-12 16:11_

Yeah, I think these checks can be combined with the `PY33_PLUS_REMOVE_FUTURES.contains` and `PY37_PLUS_REMOVE_FUTURES.contains` checks we had before, and we can just `continue` like in F401 if these conditions aren't met.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_future_import.rs`:140 on 2025-08-12 16:12_

I'm not totally sold on splitting off this helper function. I think with judicious use of `let-else` we could put this back since it won't be too indented. But I don't feel too strongly either way.

---

_@ntBre reviewed on 2025-08-12 16:18_

Thanks, this looks good to me in general. I just had a few minor suggestions on the code itself.

More broadly, I think we should probably split this into two PRs. Converting to a binding-based rule and checking for uses of the `__future__` imports is a bug fix that resolves the linked issue. But emitting separate diagnostics for each unused import and updating the ranges accordingly are conceptually separate and need to be preview-gated as Micha said. I think they're separate enough that it would be easier to review them in a separate PR.

I do think these changes are nice and worth having, just a better fit for a separate patch, in my opinion.

---

_Review comment by @IDrokin117 on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_future_import.rs`:118 on 2025-08-13 15:20_

I made a guess that looping over `chain(PY33_PLUS_REMOVE_FUTURES, PY37_PLUS_REMOVE_FUTURES).unique()` will be cheaper than looping over all the bindings, because we need to check only 9 names instead of all bindings, which I assume will be more than 9 on average. For `F401` it makes sense to iterate over all of the bindings because it has to check all unused bindings, not only related to `future`. So, it might be an optimization. Am I wrong?

---

_Review comment by @IDrokin117 on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_future_import.rs`:140 on 2025-08-13 15:25_

Removed helper function.

---

_Review comment by @IDrokin117 on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_future_import.rs`:163 on 2025-08-13 15:28_

Noticed. Replaced with `std::iter::once`

---

_Comment by @IDrokin117 on 2025-08-13 16:01_

@ntBre Thanks for your review.
I haven't put the rule behind a preview yet. Is setting `RuleGroup::Preview` sufficient?
> More broadly, I think we should probably split this into two PRs. Converting to a binding-based rule and checking for uses of the `__future__` imports is a bug fix that resolves the linked issue. But emitting separate diagnostics for each unused import and updating the ranges accordingly are conceptually separate and need to be preview-gated as Micha said. I think they're separate enough that it would be easier to review them in a separate PR.

Sorry, but I don't understand how to split the PR into two independent PRs. If I fix the bug which implies determining used names from the `__future__` module, then I need to have the ability to remove only unused ones. Otherwise, it will introduce a bug. Previously, the fix removes the entire import statement, because there were no exceptions, but now it has to remove only the dedicated one. So, here:
```
from __future__ import absolute_import, division

print(division)
```
only `absolute_import` must be removed, not the entire line. Please let me know what I'm missing.



---

_@IDrokin117 reviewed on 2025-08-13 16:02_

---

_Comment by @ntBre on 2025-08-13 16:49_

I think you can update the range of the fix without updating the range of the diagnostic itself. The diagnostic range is used for the noqa code and needs to be modified only in `preview`, but updating the fix to remove only the unused import is a bug fix. Does that help? The old code also had support for removing only part of an import line without emitting multiple diagnostics, as you can see in this snapshot that didn't change on this branch:

https://github.com/astral-sh/ruff/blob/f0b03c3e8607c686aa03da154c7afa90cdb60a37/crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP010.py.snap#L109-L116

For the bug fix, I think you'd need to preserve the old behavior of gathering all of the unused imports in the same import line and emitting a single diagnostic. This is work we have to do one way or another because even if we keep the changes in the same PR, we have to preserve the old behavior when preview is disabled.

One way of implementing this might be grouping the diagnostics by parent statement, which you're already computing. Hopefully it's not too much trouble.

---

_@ntBre reviewed on 2025-08-13 16:53_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_future_import.rs`:118 on 2025-08-13 16:53_

Ah I guess that makes sense. I was kind of assuming scopes are small, and we could `continue` very quickly if the name isn't from `__future__`. I think I'd feel better in either case if we could short-circuit somehow if there are no `__future__` imports in scope, but I'm not sure how easy that is to do.

---

_Comment by @codspeed-hq[bot] on 2025-08-13 17:20_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/IDrokin117%3Afix%2Fruff-19561?runnerMode=Instrumentation)

### Merging #19769 will **not alter performance**

<sub>Comparing <code>IDrokin117:fix/ruff-19561</code> (e85d3d1) with <code>main</code> (df0648a)</sub>



### Summary

`✅ 42` untouched benchmarks  





---

_@IDrokin117 reviewed on 2025-08-13 18:18_

---

_Review comment by @IDrokin117 on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_future_import.rs`:118 on 2025-08-13 18:18_

@ntBre During analysis, 161 bindings will be created for this piece of code. So, I think it is a good optimization to only iterate over a list of REMOVE_FUTURE.
```python
from __future__ import nested_scopes, generators
from __future__ import with_statement, unicode_literals

print(with_statement)
```

---

_@ntBre reviewed on 2025-08-13 18:24_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_future_import.rs`:118 on 2025-08-13 18:24_

Ahhh, I think we inject bindings for all the builtins. Okay, sounds good the way you have it then!

---

_Comment by @IDrokin117 on 2025-08-13 18:30_

@ntBre 
> I think you can update the range of the fix without updating the range of the diagnostic itself. The diagnostic range is used for the noqa code and needs to be modified only in `preview`, but updating the fix to remove only the unused import is a bug fix. Does that help?

It was very helpful, thanks. I believe I implemented what you suggested. So
1. I reverted diagnosis changes -> previous test cases didn't change (except filename).
2. Current implementation aggregates aliases and applies one diagnosis per stmt (as before).

I will create a second PR with Preview changes when you approve the current PR. 

---

_Review requested from @ntBre by @IDrokin117 on 2025-08-17 10:22_

---

_@ntBre approved on 2025-08-20 19:20_

Excellent, thank you!

I'll look forward to the preview PR (if you're still interested, no pressure!). The ranges highlighting only the specific unused import were really nice :) but this neatly avoids a breaking change for now.

---

_Renamed from "[`pyupgrade`] Reworked to not report future features as unnecessary when they are used (`UP010`)" to "[`pyupgrade`] Avoid reporting `__future__` features as unnecessary when they are used (`UP010`)" by @ntBre on 2025-08-20 19:21_

---

_Merged by @ntBre on 2025-08-20 19:22_

---

_Closed by @ntBre on 2025-08-20 19:22_

---

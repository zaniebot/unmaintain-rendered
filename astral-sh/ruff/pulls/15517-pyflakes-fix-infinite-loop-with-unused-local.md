```yaml
number: 15517
title: "[`pyflakes`] Fix infinite loop with unused local import in `__init__.py` (`F401`)"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - fixes
  - preview
assignees: []
merged: true
base: main
head: brent/f401-loop
created_at: 2025-01-15T22:25:09Z
updated_at: 2025-01-16T15:43:35Z
url: https://github.com/astral-sh/ruff/pull/15517
synced_at: 2026-01-10T20:34:00Z
```

# [`pyflakes`] Fix infinite loop with unused local import in `__init__.py` (`F401`)

---

_Pull request opened by @ntBre on 2025-01-15 22:25_

## Summary

This fixes the infinite loop reported in #12897, where an `unused-import` that is undefined at the scope of `__all__` is "fixed" by adding it to `__all__` repeatedly. These changes make it so that only imports in the global scope will be suggested to add to `__all__` and the unused local import is simply removed.

## Test Plan

Added a CLI integration test that sets up the same module structure as the original report

Closes #12897


---

_Label `bug` added by @ntBre on 2025-01-15 22:25_

---

_Comment by @github-actions[bot] on 2025-01-15 22:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:306 on 2025-01-16 06:57_

I think it would be cheaper to retrieve the binding with said `id` and then test if `binding.scope.is_global` (`O(1)` vs `O(n)`)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:339 on 2025-01-16 07:02_

I'm inclined to store the `ScopeId` instead. It seems more generally useful and is cheap as well

---

_@MichaReiser reviewed on 2025-01-16 07:03_

I'd prefer if we can move the test closer to the rule rather than making it a CLI tests. We only use CLI tests for features that can't be tested otherwise but we prefer unit tests or tool specific integration tests for everything else because they're faster to run, easier to find, and there's less code "between" the test and the implementation if something goes wrong. 

I'm requesting review from @AlexWaygood to review the rule behavior change. 

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-16 07:03_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:339 on 2025-01-16 11:42_

And passing around a `ScopeId` is more strongly typed/more readable than passing around a boolean value

---

_@AlexWaygood reviewed on 2025-01-16 11:42_

---

_Comment by @AlexWaygood on 2025-01-16 12:05_

Nice diagnosis! The change to the rule's semantics look correct to me.

It's nice that this is a targeted fix as well. I think I tried to fix this a while back, but made the mistake of trying to untangle some of the more spaghetti-code-like parts of this rule at the same time, and got bogged down in a tricky refactor that was hard to pull off.

I agree that I think it should be possible to test this fix using fixtures rather than a CLI integration test. We already have several mock packages set up here that we use for testing F401; can we create another mock package for this scenario? <https://github.com/astral-sh/ruff/tree/main/crates/ruff_linter/resources/test/fixtures/pyflakes>. The actual tests that use the fixtures are here: https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyflakes/mod.rs

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-01-16 12:06_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_33/__init__.py`:3 on 2025-01-16 12:52_

(you'll have to update the snapshots if you accept this change)

```suggestion
"""Regression test for https://github.com/astral-sh/ruff/issues/12897"""

__all__ = ('Spam',)
```

---

_Comment by @ntBre on 2025-01-16 12:52_

Thank you both for the suggestions! For some reason I immediately assumed that multiple files meant I needed an integration test, but these examples were very helpful. I first made the unit test changes on `main` to make sure they triggered the original error before applying them here. And using the `ScopeId` is much nicer, thank you!

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/mod.rs`:290 on 2025-01-16 12:52_

```suggestion
    // Regression test for https://github.com/astral-sh/ruff/issues/12897
    #[test_case(Rule::UnusedImport, Path::new("F401_33/__init__.py"))]
```

---

_@AlexWaygood approved on 2025-01-16 12:53_

Very nice!

---

_@ntBre reviewed on 2025-01-16 12:54_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_33/__init__.py`:3 on 2025-01-16 12:54_

Oops, good call. I'll accept these and rerun.

---

_Label `preview` added by @AlexWaygood on 2025-01-16 12:57_

---

_Label `fixes` added by @AlexWaygood on 2025-01-16 12:57_

---

_@MichaReiser approved on 2025-01-16 13:02_

---

_Merged by @ntBre on 2025-01-16 15:43_

---

_Closed by @ntBre on 2025-01-16 15:43_

---

_Branch deleted on 2025-01-16 15:43_

---

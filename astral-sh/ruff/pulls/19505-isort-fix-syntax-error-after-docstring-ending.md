```yaml
number: 19505
title: "[`isort`] Fix syntax error after docstring ending with backslash (`I002`)"
type: pull_request
state: merged
author: jimhoekstra
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: bug/insert-required-imports-after-continuation
created_at: 2025-07-23T10:32:00Z
updated_at: 2025-07-31T07:13:25Z
url: https://github.com/astral-sh/ruff/pull/19505
synced_at: 2026-01-10T17:58:13Z
```

# [`isort`] Fix syntax error after docstring ending with backslash (`I002`)

---

_Pull request opened by @jimhoekstra on 2025-07-23 10:32_

Issue: https://github.com/astral-sh/ruff/issues/19498

## Summary

[missing-required-import](https://docs.astral.sh/ruff/rules/missing-required-import/) inserts the missing import on the line immediately following the last line of the docstring. However, if the dosctring is immediately followed by a continuation token (i.e. backslash) then this leads to a syntax error because Python interprets the docstring and the inserted import to be on the same line. 

The proposed solution in this PR is to check if the first token after a file docstring is a continuation character, and if so, to advance an additional line before inserting the missing import.

## Test Plan

Added a unit test, and the following example was verified manually:

Given this simple test Python file:

```python
"Hello, World!"\

print(__doc__)
```

and this ruff linting configuration in the `pyproject.toml` file:

```toml
[tool.ruff.lint]
select = ["I"]

[tool.ruff.lint.isort]
required-imports = ["import sys"]
```

Without the changes in this PR, the ruff linter would try to insert the missing import in line 2, resulting in a syntax error, and report the following:

`error: Fix introduced a syntax error. Reverting all changes.`

With the changes in this PR, ruff correctly advances one more line before adding the missing import, resulting in the following output:

```python
"Hello, World!"\

import sys

print(__doc__)
```


---

_Marked ready for review by @jimhoekstra on 2025-07-23 11:36_

---

_Review requested from @ntBre by @ntBre on 2025-07-24 13:07_

---

_Label `bug` added by @ntBre on 2025-07-25 22:01_

---

_Label `fixes` added by @ntBre on 2025-07-25 22:01_

---

_@ntBre reviewed on 2025-07-28 20:18_

This is great, thank you!

Would you mind adding an I002 test too? That would also have the benefit of showing a nice snapshot instead of comparing to a `TextSize` directly. I think `22` is correct from counting manually, but the snapshot will be an easy way to double check :)

I think you could probably add another `test_case` on this function or one of the others nearby:

https://github.com/astral-sh/ruff/blob/1f0d39a7cedc0c08a1a4265f943d70caa87f1c11/crates/ruff_linter/src/rules/isort/mod.rs#L804-L806

---

_Comment by @github-actions[bot] on 2025-07-28 20:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-07-28 20:29_

Don't worry about the ecosystem check yet, I think it's relative to an old commit since I only just approved the workflow. I expect it to clear up after a new commit.

---

_Comment by @jimhoekstra on 2025-07-29 20:09_

Thanks for your reply! I've added a snapshot test in my latest commit, how does it look?

---

_@jimhoekstra reviewed on 2025-07-29 21:23_

---

_Review comment by @jimhoekstra on `crates/ruff_linter/src/rules/isort/mod.rs`:799 on 2025-07-29 21:23_

@ntBre Do you think it would be a good idea to rename this existing test case (`docstring_with_continuation.py`) to something like `docstring_with_semicolon_and_continuation.py` to avoid ambiguity with the test case that I just added (i.e. `docstring_followed_by_continuation.py`)?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/isort/mod.rs`:799 on 2025-07-30 15:08_

I think it's fine, but good catch!

---

_@ntBre reviewed on 2025-07-30 15:09_

The new snapshots look great, thank you! This is good to merge once CI passes.

---

_@ntBre approved on 2025-07-30 15:09_

---

_Merged by @ntBre on 2025-07-30 16:12_

---

_Closed by @ntBre on 2025-07-30 16:12_

---

_Renamed from "fix missing-required-imports introducing syntax error after dosctring ending with backslash" to "[`isort`] Fix syntax error after docstring ending with backslash (`I002`)" by @ntBre on 2025-07-30 16:13_

---

_Branch deleted on 2025-07-31 07:13_

---

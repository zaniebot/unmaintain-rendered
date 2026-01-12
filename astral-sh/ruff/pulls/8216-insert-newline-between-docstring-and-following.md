```yaml
number: 8216
title: Insert newline between docstring and following own line comment
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: insert-newline-between-docstring-and-comment
created_at: 2023-10-25T13:44:10Z
updated_at: 2023-10-31T11:07:24Z
url: https://github.com/astral-sh/ruff/pull/8216
synced_at: 2026-01-10T23:40:55Z
```

# Insert newline between docstring and following own line comment

---

_Pull request opened by @konstin on 2023-10-25 13:44_

**Summary** Previously, own line comment following after a docstring followed by newline(s) before the first content statement were treated as trailing on the docstring and we didn't insert a newline after the docstring as black would.

Before:
```python
class ModuleBrowser:
    """Browse module classes and functions in IDLE."""
    # This class is also the base class for pathbrowser.PathBrowser.

    def __init__(self, master, path, *, _htest=False, _utest=False):
        pass
```
After:
```python
class ModuleBrowser:
    """Browse module classes and functions in IDLE."""

    # This class is also the base class for pathbrowser.PathBrowser.

    def __init__(self, master, path, *, _htest=False, _utest=False):
        pass
```

I'm not entirely happy about hijacking `handle_own_line_comment_between_statements`, but i don't know a better spot to put it.

Fixes #7948

**Test Plan** Fixtures

---

_Label `formatter` added by @konstin on 2023-10-25 13:44_

---

_Review requested from @MichaReiser by @konstin on 2023-10-25 13:44_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-10-26 00:26_

---

_@MichaReiser approved on 2023-10-26 00:28_

I like the solution

---

_@charliermarsh reviewed on 2023-10-26 01:57_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:564 on 2023-10-26 01:57_

What if there's no statement in the body after the comment? Like:

```python
class ModuleBrowser:
    """Browse...."""
    # Insert a newline above
```

Black inserts a newline there -- does this handle that case?


---

_@charliermarsh reviewed on 2023-10-26 17:24_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:564 on 2023-10-26 17:24_

Looks like it doesn't support that case yet. How else could we approach it? 🤔 

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:564 on 2023-10-27 10:45_

Fixed, manually inserting a newline seems to be the best option

---

_@konstin reviewed on 2023-10-27 10:45_

---

_@charliermarsh reviewed on 2023-10-28 01:07_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/suite.rs`:610 on 2023-10-28 01:07_

This should only be applied to class docstrings, right? (Wouldn't it also apply to function docstrings as written?)

---

_@charliermarsh reviewed on 2023-10-28 01:07_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/suite.rs`:591 on 2023-10-28 01:07_

I think this comment is stale.

---

_Merged by @konstin on 2023-10-30 13:18_

---

_Closed by @konstin on 2023-10-30 13:18_

---

_Branch deleted on 2023-10-30 13:18_

---

_Comment by @github-actions[bot] on 2023-10-30 13:26_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no format changes.



---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/suite.rs`:610 on 2023-10-30 13:29_

@konstin - Sorry to ping, did this get addressed? I don't see test cases for it.

---

_@charliermarsh reviewed on 2023-10-30 13:29_

---

_Comment by @MichaReiser on 2023-10-31 08:27_

It seems that this PR regressed the Black compatibility for django, home-assistan, transformers, and zullip. The index is unchanged, but each project has significantly more changed files:

**Before**

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| django         |           0.99984 |              2772 |                33 |
| home-assistant |           0.99963 |             10596 |               148 |
| transformers   |           0.99967 |              2657 |               328 |
| zulip          |           0.99970 |              1459 |                22 |

**After**

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| django         |           0.99984 |              2772 |                48 |
| home-assistant |           0.99963 |             10596 |               181 |
| transformers   |           0.99967 |              2657 |               339 |
| zulip          |           0.99970 |              1459 |                23 |


I'm not sure why this wasn't catched by the ecosystem check (CC: @zanieb) but we should make sure that this doesn't get released by either reverting the change, or following up with a follow up PR before the next release goes out. 

---

_@konstin reviewed on 2023-10-31 11:07_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/suite.rs`:610 on 2023-10-31 11:07_

Addressed in https://github.com/astral-sh/ruff/pull/8375

---

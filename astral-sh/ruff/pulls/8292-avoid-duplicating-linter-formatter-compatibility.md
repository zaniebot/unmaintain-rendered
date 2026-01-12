```yaml
number: 8292
title: Avoid duplicating linter-formatter compatibility warnings
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/warn-user-once
created_at: 2023-10-28T01:29:07Z
updated_at: 2023-10-30T23:32:56Z
url: https://github.com/astral-sh/ruff/pull/8292
synced_at: 2026-01-10T23:40:55Z
```

# Avoid duplicating linter-formatter compatibility warnings

---

_Pull request opened by @charliermarsh on 2023-10-28 01:29_

## Summary

Uses `warn_user_once!` instead of `warn!` to ensure that every warning is shown exactly once, regardless of whether there are duplicates in the list, or warnings that are raised by multiple configuration files.

Closes #8271.


---

_@charliermarsh reviewed on 2023-10-28 01:29_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/format.rs`:756 on 2023-10-28 01:29_

Gave this a custom warning based on the indent-width.

---

_@charliermarsh reviewed on 2023-10-28 01:29_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/format.rs`:739 on 2023-10-28 01:29_

Gave this a custom warning based on the indent-style.

---

_Label `formatter` added by @charliermarsh on 2023-10-28 01:49_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-10-28 01:52_

---

_Comment by @github-actions[bot] on 2023-10-28 02:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.




---

_@zanieb reviewed on 2023-10-28 03:08_

---

_Review comment by @zanieb on `crates/ruff_cli/src/commands/format.rs`:729 on 2023-10-28 03:08_

Minor question — since `warn_user_once` works using the call site, does this mean if we find a second set of incompatible rules in a different pyproject file we will not emit a second message with the different set? Probably not common... but not ideal?

---

_@charliermarsh reviewed on 2023-10-28 03:23_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/format.rs`:729 on 2023-10-28 03:23_

No, for this _specific_ warning, we iterate over all `pyproject.toml` and collect all incompatible rules, so they would all be flagged here.

(All the other warnings are static, this is the only warning with dynamic input.)

---

_Review comment by @zanieb on `crates/ruff_cli/src/commands/format.rs`:729 on 2023-10-29 05:12_

Sounds good!

---

_@zanieb reviewed on 2023-10-29 05:12_

---

_@zanieb approved on 2023-10-29 05:12_

---

_Comment by @MichaReiser on 2023-10-29 10:11_

I'd like to double check the removal of the on Monday. I verified them all manually  and believe that I was able to come up with a conflict

---

_Comment by @charliermarsh on 2023-10-29 15:59_

@MichaReiser - Sorry, you're right. I think we don't reformat module-level docstrings which is why I was seeing different results. If we re-indent a function or class docstring, we do indeed change from spaces to tabs. Will restore.

---

_Comment by @charliermarsh on 2023-10-29 16:03_

@MichaReiser - Restored.

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/format.rs`:700 on 2023-10-30 00:01_

What's the motivation for using a `BTreeSet` when we still need to sort the rules on line 722? Why not use a regular `HashSet` (or create a wrapper around `BTreeSet` with a `insert(Rule)` method that inserts the formatted name)

---

_@MichaReiser approved on 2023-10-30 00:02_

---

_Merged by @charliermarsh on 2023-10-30 23:32_

---

_Closed by @charliermarsh on 2023-10-30 23:32_

---

_Branch deleted on 2023-10-30 23:32_

---

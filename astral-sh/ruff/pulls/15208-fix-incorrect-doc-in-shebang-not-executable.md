```yaml
number: 15208
title: "Fix incorrect doc in `shebang-not-executable (EXE001)` and add git+windows solution to executable bit"
type: pull_request
state: merged
author: Avasam
labels:
  - documentation
assignees: []
merged: true
base: main
head: EXE001-EXE002-update-docs
created_at: 2024-12-31T00:42:47Z
updated_at: 2025-01-03T07:24:52Z
url: https://github.com/astral-sh/ruff/pull/15208
synced_at: 2026-01-12T15:55:50Z
```

# Fix incorrect doc in `shebang-not-executable (EXE001)` and add git+windows solution to executable bit

---

_@Avasam_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

I noticed that the solution mentioned in [shebang-not-executable (EXE001)](https://docs.astral.sh/ruff/rules/shebang-not-executable/#shebang-not-executable-exe001) was incorrect and likely copy-pasted from [shebang-missing-executable-file (EXE002)](https://docs.astral.sh/ruff/rules/shebang-missing-executable-file/#shebang-missing-executable-file-exe002)

It was telling users to remove the executable bit from a non-executable file. Which does nothing.

I also noticed locally that:
- `chmod` wouldn't cause any file change to be noticed by git (`EXE` was also passing locally) under WSL
- Using git allows anyone to fix this lint across OSes, for projects with CIs using git

So I added a solution using [git update-index --chmod](https://git-scm.com/docs/git-update-index#Documentation/git-update-index.txt---chmod-x)

## Test Plan

<!-- How was it tested? -->
No test plan, doc changes only.
As for running the chmod commands: https://github.com/python/typeshed/pull/13346

---

_Comment by @github-actions[bot] on 2024-12-31 00:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_executable/rules/shebang_not_executable.rs`:31 on 2024-12-31 05:17_

Let's remove the code block as it's irrelevant now.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_executable/rules/shebang_not_executable.rs`:44 on 2024-12-31 05:18_

```suggestion
/// - [Git documentation: `git update-index --chmod`](https://git-scm.com/docs/git-update-index#Documentation/git-update-index.txt---chmod-x)
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_executable/rules/shebang_missing_executable_file.rs`:39 on 2024-12-31 05:18_

```suggestion
/// - [Git documentation: `git update-index --chmod`](https://git-scm.com/docs/git-update-index#Documentation/git-update-index.txt---chmod-x)
```

---

_@dhruvmanila approved on 2024-12-31 05:19_

Thank you!

---

_Label `documentation` added by @dhruvmanila on 2024-12-31 05:19_

---

_@dhruvmanila reviewed on 2024-12-31 05:21_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_executable/rules/shebang_not_executable.rs`:25 on 2024-12-31 05:21_

```suggestion
/// If the file is meant to be executable, add the executable bit to the file
```

---

_@Avasam reviewed on 2024-12-31 05:26_

---

_Review comment by @Avasam on `crates/ruff_linter/src/rules/flake8_executable/rules/shebang_not_executable.rs`:31 on 2024-12-31 05:26_

oops, yeah, that should be removed

---

_Merged by @dhruvmanila on 2024-12-31 05:35_

---

_Closed by @dhruvmanila on 2024-12-31 05:35_

---

_Branch deleted on 2025-01-03 07:24_

---

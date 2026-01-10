```yaml
number: 8384
title: Fix to respect comments positioning in pyproject.toml on change
type: pull_request
state: merged
author: flyaroundme
labels:
  - bug
assignees: []
merged: true
base: main
head: fix_comments_position_in_pyproject.toml
created_at: 2024-10-20T15:07:37Z
updated_at: 2024-10-20T17:16:13Z
url: https://github.com/astral-sh/uv/pull/8384
synced_at: 2026-01-10T12:54:08Z
```

# Fix to respect comments positioning in pyproject.toml on change

---

_Pull request opened by @flyaroundme on 2024-10-20 15:07_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR is is to address the problem when the same-line comments in `pyproject.toml` could be found in unpredictable positions after `uv add` or `remove` reformats the `pyproject.toml` file.

Introduced the `Comment` structure in `pyproject_mut` module to distinguish "same-line" comments and "full-line" comments while reformatting, because logic for them differs.
Sorry, the implementation could be clumsy, I'm just learning Rust, but it seems to work 😅

Closes https://github.com/astral-sh/uv/issues/8343

## Test Plan

Added the new test: `add_preserves_comments_indentation_and_sameline_comments`

To test followed the actions from the issue ticket https://github.com/astral-sh/uv/issues/8343



---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-20 16:11_

---

_@charliermarsh approved on 2024-10-20 17:02_

Thanks -- I think this is an improvement!

---

_Label `bug` added by @charliermarsh on 2024-10-20 17:07_

---

_@charliermarsh reviewed on 2024-10-20 17:11_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/pyproject_mut.rs`:57 on 2024-10-20 17:11_

I just renamed these to match terminology we use in the Ruff formatter.

---

_@charliermarsh reviewed on 2024-10-20 17:12_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/edit.rs`:6392 on 2024-10-20 17:12_

Small follow-up PR for this (the behavior is unrelated to this change): https://github.com/astral-sh/uv/pull/8387

---

_Merged by @charliermarsh on 2024-10-20 17:16_

---

_Closed by @charliermarsh on 2024-10-20 17:16_

---

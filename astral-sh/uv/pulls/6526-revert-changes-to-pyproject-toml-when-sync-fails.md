```yaml
number: 6526
title: "Revert changes to pyproject.toml when sync fails duing `uv add`"
type: pull_request
state: merged
author: tjquillan
labels:
  - cli
assignees: []
merged: true
base: main
head: tjquillan/issue_6486
created_at: 2024-08-23T15:31:54Z
updated_at: 2024-08-23T17:54:51Z
url: https://github.com/astral-sh/uv/pull/6526
synced_at: 2026-01-12T16:07:24Z
```

# Revert changes to pyproject.toml when sync fails duing `uv add`

---

_@tjquillan_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This is a attempt at fixing https://github.com/astral-sh/uv/issues/6486. It reverts changes made to `pyproject.toml` when sync fails during `uv add`. This solution felt a little heavy handed and could probably be improved but it is what happens when locking fails during `uv add` so I thought it would be a good start.

## Test Plan

<!-- How was it tested? -->

I have added a test case for this to `tests/edit.rs`. It uses `pytorch==1.0.2` to achieve the desired failure.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-23 15:54_

---

_Comment by @charliermarsh on 2024-08-23 15:54_

Thanks, this looks good! Feel free to un-mark as draft.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/add.rs`:600 on 2024-08-23 15:54_

You can use `if let Err(err) = ...` instead of `match` here.

---

_@charliermarsh reviewed on 2024-08-23 15:54_

---

_Marked ready for review by @tjquillan on 2024-08-23 16:02_

---

_Comment by @charliermarsh on 2024-08-23 16:33_

I can fix that test failure -- or you can add this:
```rust
    let filters = std::iter::once((r"exit code: 1", "exit status: 1"))
        .chain(context.filters())
        .collect::<Vec<_>>();
    uv_snapshot!(filters, ...)
```

---

_Label `cli` added by @charliermarsh on 2024-08-23 16:33_

---

_Merged by @charliermarsh on 2024-08-23 17:54_

---

_Closed by @charliermarsh on 2024-08-23 17:54_

---

_Comment by @charliermarsh on 2024-08-23 17:54_

Thank you!

---

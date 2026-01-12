```yaml
number: 4611
title: "fix the incorrect handling of markers in `pip tree`"
type: pull_request
state: merged
author: ChannyClaus
labels:
  - bug
assignees: []
merged: true
base: main
head: pip-tree-fix
created_at: 2024-06-28T05:05:27Z
updated_at: 2024-06-28T13:28:43Z
url: https://github.com/astral-sh/uv/pull/4611
synced_at: 2026-01-12T16:06:20Z
```

# fix the incorrect handling of markers in `pip tree`

---

_@ChannyClaus_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
resolves https://github.com/astral-sh/uv/issues/4609

previously, the implementation of `required_with_no_extra` was incorrect, particularly when there are packages that do not require any extras but have other types of markers.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
the existing tests also did cover this (my bad... missed it) but added a smaller test since this bug would've been more obvious with this new test.
<!-- How was it tested? -->


---

_Comment by @ChannyClaus on 2024-06-28 05:16_

@zanieb also oops noticed the cycle message still uses # 😭 (seems like i messed up the merge conflict)

i can make a follow-up sometime to fix that as well

---

_Comment by @ChannyClaus on 2024-06-28 05:19_

there seem to be a bunch of CI failures from 429... (i'll try re-triggering tomorrow or something)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-28 10:51_

---

_Label `bug` added by @charliermarsh on 2024-06-28 10:51_

---

_@charliermarsh reviewed on 2024-06-28 12:49_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/tree.rs`:103 on 2024-06-28 12:49_

Is this not sufficient?

```rust
requirement.marker
    .as_ref()
    .map_or(true, |m| m.evaluate(marker_environment, &[]))
```

Why do we need the `has_extra` piece?

---

_@ChannyClaus reviewed on 2024-06-28 13:05_

---

_Review comment by @ChannyClaus on `crates/uv/src/commands/pip/tree.rs`:103 on 2024-06-28 13:05_

oops, you're right, removed 👍 

---

_Merged by @charliermarsh on 2024-06-28 13:28_

---

_Closed by @charliermarsh on 2024-06-28 13:28_

---

_Comment by @charliermarsh on 2024-06-28 13:28_

Thanks!

---

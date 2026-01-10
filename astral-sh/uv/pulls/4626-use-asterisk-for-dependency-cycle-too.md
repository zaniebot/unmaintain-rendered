```yaml
number: 4626
title: use asterisk for dependency cycle too
type: pull_request
state: merged
author: ChannyClaus
labels:
  - cli
assignees: []
merged: true
base: main
head: pip-tree-cycle-use-asterisk
created_at: 2024-06-28T16:10:34Z
updated_at: 2024-06-28T22:35:37Z
url: https://github.com/astral-sh/uv/pull/4626
synced_at: 2026-01-10T13:48:28Z
```

# use asterisk for dependency cycle too

---

_Pull request opened by @ChannyClaus on 2024-06-28 16:10_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
https://github.com/astral-sh/uv/pull/4611#issuecomment-2196159445 https://github.com/astral-sh/uv/pull/4449#issuecomment-2184174557 this change was lost in merge conflict resolution :.(

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
almost purely aesthetic change 🌵 
<!-- How was it tested? -->


---

_Comment by @ChannyClaus on 2024-06-28 16:10_

@zanieb fyi

---

_@ibraheemdev reviewed on 2024-06-28 16:34_

---

_Review comment by @ibraheemdev on `crates/uv/tests/pip_tree.rs`:627 on 2024-06-28 16:34_

Why is the message changing?

---

_@ibraheemdev reviewed on 2024-06-28 16:35_

---

_Review comment by @ibraheemdev on `crates/uv/tests/pip_tree.rs`:627 on 2024-06-28 16:35_

Ah I see, if `--no-dedupe` isn't passed we se the same message for both.

---

_@ibraheemdev approved on 2024-06-28 16:35_

---

_@zanieb reviewed on 2024-06-28 16:43_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/tree.rs`:61 on 2024-06-28 16:43_

Should we say "Package tree is a cycle and cannot be shown" as we discussed before?

---

_Label `cli` added by @zanieb on 2024-06-28 16:43_

---

_@ibraheemdev reviewed on 2024-06-28 16:48_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/tree.rs`:61 on 2024-06-28 16:48_

Or "Package tree forms/contains a cycle and cannot be shown"

---

_Merged by @ibraheemdev on 2024-06-28 22:35_

---

_Closed by @ibraheemdev on 2024-06-28 22:35_

---

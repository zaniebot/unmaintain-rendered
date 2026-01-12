```yaml
number: 8324
title: "Docs: add `.lint` on generating options examples."
type: pull_request
state: merged
author: T-256
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-3
created_at: 2023-10-29T13:53:32Z
updated_at: 2023-10-29T16:39:54Z
url: https://github.com/astral-sh/ruff/pull/8324
synced_at: 2026-01-12T02:11:58Z
```

# Docs: add `.lint` on generating options examples.

---

_Pull request opened by @T-256 on 2023-10-29 13:53_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

In continue of #8317 
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Marked ready for review by @T-256 on 2023-10-29 13:58_

---

_Comment by @github-actions[bot] on 2023-10-29 14:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no format changes.



---

_@charliermarsh reviewed on 2023-10-29 15:56_

---

_Review comment by @charliermarsh on `crates/ruff_dev/src/generate_options.rs`:135 on 2023-10-29 15:56_

Unfortunately we may need to go through and add `.lint` to the appropriate settings, since some settings only exist at the top-level and aren't part of `.lint` (e.g., `.extend-exclude`).

---

_@T-256 reviewed on 2023-10-29 16:28_

---

_Review comment by @T-256 on `crates/ruff_dev/src/generate_options.rs`:135 on 2023-10-29 16:28_

This scope is only for non-top-level settings.
Top-level names already excluded at [here](https://github.com/astral-sh/ruff/pull/8324/files#diff-f0bd91d69ddb50873c3b753b352c6e5e6dfbd791f662b04060cb6d70dbef60b0R130) and always replaced with empty string. did I understand wrong?

---

_@charliermarsh reviewed on 2023-10-29 16:39_

---

_Review comment by @charliermarsh on `crates/ruff_dev/src/generate_options.rs`:135 on 2023-10-29 16:39_

Thanks -- you're right! Had to refresh my memory here.

---

_Label `documentation` added by @charliermarsh on 2023-10-29 16:39_

---

_Merged by @charliermarsh on 2023-10-29 16:39_

---

_Closed by @charliermarsh on 2023-10-29 16:39_

---

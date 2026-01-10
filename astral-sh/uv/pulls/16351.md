```yaml
number: 16351
title: Fix typo in MissingTopLevel warning
type: pull_request
state: merged
author: Dahlgren
labels:
  - error messages
assignees: []
merged: true
base: main
head: fix/top-level-warning
created_at: 2025-10-17T21:50:14Z
updated_at: 2025-10-19T09:32:50Z
url: https://github.com/astral-sh/uv/pull/16351
synced_at: 2026-01-10T06:36:16Z
```

# Fix typo in MissingTopLevel warning

---

_Pull request opened by @Dahlgren on 2025-10-17 21:50_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
The warning shown when `egg-info` lacks `top_level.txt` incorrectly warns about missing `top-level.txt`
https://github.com/astral-sh/uv/blob/ee2649feaa78b847ebcd44daebdce28c2e974842/crates/uv-install-wheel/src/uninstall.rs#L179-L188
https://github.com/astral-sh/uv/blob/ee2649feaa78b847ebcd44daebdce28c2e974842/crates/uv/src/commands/pip/operations.rs#L687-L693

The non fatal warning with incorrect filename was introduced in https://github.com/astral-sh/uv/pull/6881 which changed previous fatal error https://github.com/astral-sh/uv/issues/6872 with correct `top_level.txt` output.

## Test Plan

<!-- How was it tested? -->
Updated unit test to reflect change in warning


---

_Label `error messages` added by @konstin on 2025-10-19 09:32_

---

_@konstin approved on 2025-10-19 09:32_

Thanks!

---

_Merged by @konstin on 2025-10-19 09:32_

---

_Closed by @konstin on 2025-10-19 09:32_

---

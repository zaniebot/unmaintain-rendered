```yaml
number: 15231
title: Fix/sync only group entry points dose not install projects with entry points
type: pull_request
state: closed
author: yumeminami
labels: []
assignees: []
base: main
head: fix/sync-only-group-entry-points
created_at: 2025-08-12T03:48:48Z
updated_at: 2025-08-12T05:50:13Z
url: https://github.com/astral-sh/uv/pull/15231
synced_at: 2026-01-10T06:44:33Z
```

# Fix/sync only group entry points dose not install projects with entry points

---

_Pull request opened by @yumeminami on 2025-08-12 03:48_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

  ## Summary

 Fixed `uv sync --only-group dev` to install the project itself (including entry points) while still filtering dependencies to only the specified groups. Previously it incorrectly skipped project installation entirely. Modified group filtering logic in installable.rs to use `InstallOptions.include_package()` instead of `dev.prod()`.


## Test Plan

<!-- How was it tested? -->
 - Added comprehensive test case covering: `--only-group`
  - Verified fix with manual reproduction case from issue #15215
  - Confirmed all existing tests pass without changes
  - Validated backward compatibility: `--no-install-project` behavior unchanged

---

_Closed by @yumeminami on 2025-08-12 05:38_

---

_Reopened by @yumeminami on 2025-08-12 05:44_

---

_Closed by @yumeminami on 2025-08-12 05:50_

---

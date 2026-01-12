```yaml
number: 15921
title: "Fix implied `platform_machine` marker for `win_arm64` platform tag"
type: pull_request
state: merged
author: RazerM
labels: []
assignees: []
merged: true
base: main
head: fix/win_arm64-implied-platform-machine-marker
created_at: 2025-09-17T21:46:56Z
updated_at: 2025-09-19T20:34:42Z
url: https://github.com/astral-sh/uv/pull/15921
synced_at: 2026-01-12T16:12:01Z
```

# Fix implied `platform_machine` marker for `win_arm64` platform tag

---

_@RazerM_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

I'm back again after #14041, this time for `win_arm64`. I [asked a Windows on ARM user](https://github.com/getlogbook/logbook/pull/451#issuecomment-3295513650) what the value of `platform.machine()` is, and it's `ARM64`.

## Test Plan

Updated/added tests


---

_@charliermarsh approved on 2025-09-17 21:48_

---

_Comment by @zanieb on 2025-09-17 22:13_

Huh I thought we knew this https://github.com/astral-sh/uv/blob/330e56e778cb5313a8ddb84cace8ce32e87a944b/crates/uv-configuration/src/target_triple.rs#L586

ref https://github.com/astral-sh/uv/pull/15347#discussion_r2282554858

Thanks!

---

_Merged by @charliermarsh on 2025-09-17 23:33_

---

_Closed by @charliermarsh on 2025-09-17 23:33_

---

_Branch deleted on 2025-09-19 20:34_

---

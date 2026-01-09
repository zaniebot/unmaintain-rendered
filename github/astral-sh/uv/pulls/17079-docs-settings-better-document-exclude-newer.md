---
number: 17079
title: "docs(settings): better document `exclude-newer*`"
type: pull_request
state: closed
author: mkniewallner
labels: []
assignees: []
base: main
head: docs/update-exclude-newer-settings
created_at: 2025-12-10T21:22:38Z
updated_at: 2025-12-11T15:25:27Z
url: https://github.com/astral-sh/uv/pull/17079
synced_at: 2026-01-07T13:12:19-06:00
---

# docs(settings): better document `exclude-newer*`

---

_Pull request opened by @mkniewallner on 2025-12-10 21:22_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Following the changes in https://github.com/astral-sh/uv/pull/16814, documentation for [`--exclude-newer`](https://docs.astral.sh/uv/reference/cli/#uv-sync--exclude-newer) and [`--exclude-newer-package`](https://docs.astral.sh/uv/reference/cli/#uv-sync--exclude-newer-package) arguments were updated, but not their settings counterparts, so this just updates the settings ones to closely match the arguments ones.

## Test Plan

Ran documentation locally.

---

_@zanieb reviewed on 2025-12-10 22:22_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:809 on 2025-12-10 22:22_

We explicitly do not support local dates here.

---

_Review comment by @mkniewallner on `crates/uv-settings/src/settings.rs`:809 on 2025-12-10 22:28_

Oh sorry, I assumed it was as well here but I should have checked.

---

_@mkniewallner reviewed on 2025-12-10 22:28_

---

_Merged by @zanieb on 2025-12-11 15:23_

---

_Closed by @zanieb on 2025-12-11 15:23_

---

_Branch deleted on 2025-12-11 15:25_

---

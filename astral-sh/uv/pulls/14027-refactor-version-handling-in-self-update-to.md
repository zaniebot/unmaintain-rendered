```yaml
number: 14027
title: Refactor version handling in self_update to report upgradable version
type: pull_request
state: open
author: miried
labels: []
assignees: []
base: main
head: dryrun
created_at: 2025-06-13T14:18:22Z
updated_at: 2025-06-13T21:20:42Z
url: https://github.com/astral-sh/uv/pull/14027
synced_at: 2026-01-10T11:10:42Z
```

# Refactor version handling in self_update to report upgradable version

---

_Pull request opened by @miried on 2025-06-13 14:18_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This change is an improvement on the `dry_run` functionality introduced in https://github.com/astral-sh/uv/pull/9829. It now shows the version that would be upgraded to.

Note that despite its name, `updater.is_update_needed()` does not only check the version numbers, but it also checks if the executable can be actually upgraded. This is reflected in the wording of the new message. See https://docs.rs/axoupdater/0.9.0/axoupdater/struct.AxoUpdater.html#method.is_update_needed

## Test Plan

<!-- How was it tested? -->
Tested locally
```
info: Current version: v0.7.13
info: Checking for updates...
info: Latest/requested version: v0.7.13
info: Would not update uv. Already on the latest/requested version of uv.
```

---

_Assigned to @Gankra by @zanieb on 2025-06-13 16:52_

---

_Review requested from @Gankra by @zanieb on 2025-06-13 16:52_

---

```yaml
number: 16552
title: Update the spdx dependency to version 0.12
type: pull_request
state: merged
author: musicinmybrain
labels: []
assignees: []
merged: true
base: main
head: spdx-0.12
created_at: 2025-11-02T12:32:40Z
updated_at: 2025-11-02T18:21:29Z
url: https://github.com/astral-sh/uv/pull/16552
synced_at: 2026-01-12T16:12:19Z
```

# Update the spdx dependency to version 0.12

---

_@musicinmybrain_

https://github.com/EmbarkStudios/spdx/blob/0.12.0/CHANGELOG.md

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Simply keeps up with the latest `spdx` crate release.

## Test Plan

<!-- How was it tested? -->
I don’t currently have access to my machine that has enough memory to compile uv without workarounds, so I tested this via [a patch to the downstream Fedora packages](https://copr.fedorainfracloud.org/coprs/music/spdx/packages/).

---

_Merged by @charliermarsh on 2025-11-02 18:21_

---

_Closed by @charliermarsh on 2025-11-02 18:21_

---

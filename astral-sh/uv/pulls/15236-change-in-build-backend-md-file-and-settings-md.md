```yaml
number: 15236
title: change in build-backend.md file and settings.md
type: pull_request
state: open
author: lavisha25
labels: []
assignees: []
base: main
head: fix-uv-doc-typo
created_at: 2025-08-12T12:30:50Z
updated_at: 2025-08-12T18:18:00Z
url: https://github.com/astral-sh/uv/pull/15236
synced_at: 2026-01-10T06:44:33Z
```

# change in build-backend.md file and settings.md

---

_Pull request opened by @lavisha25 on 2025-08-12 12:30_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Review comment by @zanieb on `docs/reference/settings.md`:481 on 2025-08-12 18:17_

This file is generated, so you'd need to change this in the source code. Isn't this wrong though? It's a toml file not a Python dictionary.

---

_@zanieb reviewed on 2025-08-12 18:18_

---

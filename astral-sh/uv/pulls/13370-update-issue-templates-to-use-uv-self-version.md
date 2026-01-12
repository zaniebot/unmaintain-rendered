```yaml
number: 13370
title: "Update issue templates to use `uv self version` command"
type: pull_request
state: merged
author: liby
labels:
  - documentation
assignees: []
merged: true
base: main
head: update-version-command-in-issue-templates
created_at: 2025-05-09T21:30:43Z
updated_at: 2025-05-10T10:06:52Z
url: https://github.com/astral-sh/uv/pull/13370
synced_at: 2026-01-12T16:10:40Z
```

# Update issue templates to use `uv self version` command

---

_@liby_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This PR updates the issue templates to recommend using the `uv self version` command instead of `uv version` for retrieving uv's own version information. The `uv version` command is intended to show the current project's version (from pyproject.toml), not the uv tool version, which leads to confusion when users try to report issues.

## Test Plan

<!-- How was it tested? -->
n/a


---

_@zanieb approved on 2025-05-09 21:41_

Thanks!

---

_Merged by @zanieb on 2025-05-09 21:41_

---

_Closed by @zanieb on 2025-05-09 21:41_

---

_Label `documentation` added by @zanieb on 2025-05-09 21:41_

---

_Branch deleted on 2025-05-10 10:06_

---

```yaml
number: 16450
title: Upgrade setup-python action to version 6
type: pull_request
state: merged
author: sadikkuzu
labels:
  - internal
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-10-25T16:30:08Z
updated_at: 2025-10-27T01:54:23Z
url: https://github.com/astral-sh/uv/pull/16450
synced_at: 2026-01-12T16:12:15Z
```

# Upgrade setup-python action to version 6

---

_@sadikkuzu_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This pull request updates the GitHub Actions workflow documentation to use the latest version of the `actions/setup-python` action. This ensures compatibility with recent improvements and bug fixes in the action.

Workflow version updates:

* Updated the `uses: actions/setup-python` step from version `v5` to `v6` in two separate workflow job examples in `docs/guides/integration/github.md`. [[1]](diffhunk://#diff-e864b910728c865e8e16ddb7892761fc2ef4838f2bf256eb1e20c35b24edd9fbL96-R96) [[2]](diffhunk://#diff-e864b910728c865e8e16ddb7892761fc2ef4838f2bf256eb1e20c35b24edd9fbL119-R119)

---

_Merged by @charliermarsh on 2025-10-27 01:54_

---

_Closed by @charliermarsh on 2025-10-27 01:54_

---

_Label `internal` added by @charliermarsh on 2025-10-27 01:54_

---

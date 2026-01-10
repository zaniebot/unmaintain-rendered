```yaml
number: 1764
title: "fix: expose DefaultResolverProvider"
type: pull_request
state: merged
author: baszalmstra
labels:
  - internal
assignees: []
merged: true
base: main
head: fix/expose_default_resolver_provider
created_at: 2024-02-20T16:22:29Z
updated_at: 2024-02-20T16:34:20Z
url: https://github.com/astral-sh/uv/pull/1764
synced_at: 2026-01-10T15:33:24Z
```

# fix: expose DefaultResolverProvider

---

_Pull request opened by @baszalmstra on 2024-02-20 16:22_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The `DefaultResolverProvider` struct was not public. This PR exposes it so we can build our own and use this as a fallback.

## Test Plan

I did not explicitly test this trivial change. 

---

_@charliermarsh approved on 2024-02-20 16:34_

---

_Label `internal` added by @charliermarsh on 2024-02-20 16:34_

---

_Merged by @charliermarsh on 2024-02-20 16:34_

---

_Closed by @charliermarsh on 2024-02-20 16:34_

---

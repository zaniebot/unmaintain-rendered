```yaml
number: 16036
title: Remove docs for custom GitHub caching
type: pull_request
state: open
author: eifinger
labels:
  - documentation
assignees: []
base: main
head: patch-1
created_at: 2025-09-26T08:48:21Z
updated_at: 2025-09-26T12:21:01Z
url: https://github.com/astral-sh/uv/pull/16036
synced_at: 2026-01-10T06:36:15Z
```

# Remove docs for custom GitHub caching

---

_Pull request opened by @eifinger on 2025-09-26 08:48_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Remove docs for custom GitHub caching

The alternative instructions confused users ([example](https://github.com/astral-sh/setup-uv/issues/589)) and did not explictily state that they only apply if `setup-uv` is not used. `setup-uv` allows full control over the cache, comes with good defaults and bugs/missing features should be discussed there.

This only applies to GitHub.

## Test Plan

N/A


---

_Review requested from @zanieb by @konstin on 2025-09-26 12:20_

---

_Label `documentation` added by @konstin on 2025-09-26 12:21_

---

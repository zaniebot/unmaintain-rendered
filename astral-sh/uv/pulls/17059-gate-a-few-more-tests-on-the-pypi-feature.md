```yaml
number: 17059
title: Gate a few more tests on the pypi feature
type: pull_request
state: merged
author: musicinmybrain
labels:
  - internal
assignees: []
merged: true
base: main
head: pypi-feature-20251209
created_at: 2025-12-09T23:37:48Z
updated_at: 2025-12-10T09:41:48Z
url: https://github.com/astral-sh/uv/pull/17059
synced_at: 2026-01-12T16:12:35Z
```

# Gate a few more tests on the pypi feature

---

_@musicinmybrain_



<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Gate a few more tests on the `pypi` feature. All of these fail in offline environments because they try to communicate with PyPI.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
Applied as a patch to Fedora’s `uv` package, version 0.9.16.

---

_Label `internal` added by @konstin on 2025-12-10 09:41_

---

_Merged by @konstin on 2025-12-10 09:41_

---

_Closed by @konstin on 2025-12-10 09:41_

---

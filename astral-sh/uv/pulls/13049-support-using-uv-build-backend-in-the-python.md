```yaml
number: 13049
title: "Support using `uv build-backend` in the Python backend"
type: pull_request
state: merged
author: mgorny
labels: []
assignees: []
merged: true
base: main
head: pep517-support-using-uv
created_at: 2025-04-22T10:16:07Z
updated_at: 2025-04-22T12:00:34Z
url: https://github.com/astral-sh/uv/pull/13049
synced_at: 2026-01-12T16:10:32Z
```

# Support using `uv build-backend` in the Python backend

---

_@mgorny_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Provide an in-code switch to permit using the `uv build-backend` command rather than the default `uv-build` in the Python PEP517 backend.  This option is intended to be used by downstream packagers to provide an option of reusing `uv` that was built already instead of having to build a second `uv-build` executable that largely overlaps with `uv`.

Fixes #12389

## Test Plan

The option is intended for downstream consumption only, and it is tested downstream (via attempting to build a package using the `uv_build` backend). The backend itself is covered by tests already.

---

_Assigned to @konstin by @konstin on 2025-04-22 10:19_

---

_@konstin approved on 2025-04-22 11:35_

---

_Merged by @konstin on 2025-04-22 11:46_

---

_Closed by @konstin on 2025-04-22 11:46_

---

_Branch deleted on 2025-04-22 12:00_

---

_Comment by @mgorny on 2025-04-22 12:00_

Thank you!

---

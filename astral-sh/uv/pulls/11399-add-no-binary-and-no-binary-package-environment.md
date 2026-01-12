```yaml
number: 11399
title: "Add `NO_BINARY` and `NO_BINARY_PACKAGE` environment variables"
type: pull_request
state: merged
author: lengau
labels:
  - configuration
assignees: []
merged: true
base: main
head: 4291/no-binary-env-vars
created_at: 2025-02-10T19:55:08Z
updated_at: 2025-02-10T22:20:20Z
url: https://github.com/astral-sh/uv/pull/11399
synced_at: 2026-01-12T16:09:49Z
```

# Add `NO_BINARY` and `NO_BINARY_PACKAGE` environment variables

---

_@lengau_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This adds `NO_BINARY` and `NO_BINARY_PACKAGE` environment variables to the uv CLI, allowing the user to specify packages to build from source using environment variables. Its not a complete fix for #4291 as it does not handle the `pip` subcommand.

## Test Plan

This was tested by running `uv sync` with various `UV_NO_BINARY` and `UV_NO_BINARY_PACKAGE` environment variables set and checking that the correct set of packages were compiled rather than taken from pre-built wheels.


---

_@zanieb approved on 2025-02-10 20:05_

---

_Label `configuration` added by @zanieb on 2025-02-10 20:05_

---

_Comment by @lengau on 2025-02-10 20:14_

(Sorry for the force-push - it fixed the linting error)

---

_Renamed from "feat: add NO_BINARY and NO_BINARY_PACKAGE environment variables" to "Add `NO_BINARY` and `NO_BINARY_PACKAGE` environment variables" by @zanieb on 2025-02-10 21:05_

---

_Merged by @zanieb on 2025-02-10 21:11_

---

_Closed by @zanieb on 2025-02-10 21:11_

---

_Branch deleted on 2025-02-10 22:20_

---

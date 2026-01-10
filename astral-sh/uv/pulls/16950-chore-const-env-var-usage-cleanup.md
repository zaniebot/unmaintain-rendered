```yaml
number: 16950
title: "chore(🧹): const env var usage cleanup"
type: pull_request
state: merged
author: samypr100
labels: []
assignees: []
merged: true
base: main
head: env-vars-cleanup
created_at: 2025-12-03T02:52:40Z
updated_at: 2025-12-03T14:04:45Z
url: https://github.com/astral-sh/uv/pull/16950
synced_at: 2026-01-10T05:49:14Z
```

# chore(🧹): const env var usage cleanup

---

_Pull request opened by @samypr100 on 2025-12-03 02:52_

## Summary

* Updates existing references to use EnvVars where usage was missing.
* Adds missing entries to env var usages, e.g. new env var declarations in uv-trampoline, tests, etc.
  * Note: this doesn't affect trampoline sizes as the end result is the same
* Fixes versioning of `UV_HIDE_BUILD_OUTPUT`.

## Test Plan

Existing Tests. Compiled the trampolines locally to verify zero changes (size, binary).

## Question

Will this complicate the crates publishing release process? I'm not certain yet if it will be an issue for uv-trampoline (non-workspace member) to reference a uv workspace member from a bump & release perspective wrt lock files. If so, I'll revert the uv-trampoline changes but keep the others.

---

_Marked ready for review by @samypr100 on 2025-12-03 03:23_

---

_@charliermarsh approved on 2025-12-03 06:16_

---

_Merged by @charliermarsh on 2025-12-03 06:16_

---

_Closed by @charliermarsh on 2025-12-03 06:16_

---

_Branch deleted on 2025-12-03 14:04_

---

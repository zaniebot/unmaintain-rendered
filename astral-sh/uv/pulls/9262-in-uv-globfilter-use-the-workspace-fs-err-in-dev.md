```yaml
number: 9262
title: In uv-globfilter, use the workspace fs-err in dev-dependencies
type: pull_request
state: merged
author: musicinmybrain
labels:
  - internal
assignees: []
merged: true
base: main
head: globfilter-fs-err-3
created_at: 2024-11-20T03:48:27Z
updated_at: 2024-11-20T03:59:37Z
url: https://github.com/astral-sh/uv/pull/9262
synced_at: 2026-01-10T12:00:00Z
```

# In uv-globfilter, use the workspace fs-err in dev-dependencies

---

_Pull request opened by @musicinmybrain on 2024-11-20 03:48_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

In uv-globfilter, use the workspace `fs-err` in `dev-dependencies`.

This fixes an unnecessary dev-dependency on `fs-err` 2.x even after the workspace fs-err was updated to 3.x in https://github.com/astral-sh/uv/pull/8625.

The `Cargo.lock` file still has `fs-err v2.11.0` after this PR, but it is via `tracing-durations-export v0.3.0` rather than directly required by any `uv` crate.

## Test Plan

<!-- How was it tested? -->

```
$ cd crates/uv-globfilter/
$ cargo test
```

---

_@charliermarsh approved on 2024-11-20 03:51_

Thanks!

---

_Label `internal` added by @charliermarsh on 2024-11-20 03:51_

---

_Merged by @charliermarsh on 2024-11-20 03:59_

---

_Closed by @charliermarsh on 2024-11-20 03:59_

---

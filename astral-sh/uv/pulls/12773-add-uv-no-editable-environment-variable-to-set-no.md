```yaml
number: 12773
title: "Add `UV_NO_EDITABLE` environment variable to set `--no-editable` on all invocations"
type: pull_request
state: merged
author: haarisr
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: no-editable
created_at: 2025-04-09T07:15:25Z
updated_at: 2025-04-10T19:56:48Z
url: https://github.com/astral-sh/uv/pull/12773
synced_at: 2026-01-10T11:10:40Z
```

# Add `UV_NO_EDITABLE` environment variable to set `--no-editable` on all invocations

---

_Pull request opened by @haarisr on 2025-04-09 07:15_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Adds the env arg `UV_NO_EDITABLE`.
Closes #12735 

## Test Plan

<!-- How was it tested? -->

![image](https://github.com/user-attachments/assets/0bfde9e1-ce6e-4fcc-a8c2-0bf970c9aa9e)


I could not find a place where to add tests, any help would be appreciated



---

_Review comment by @Gankra on `crates/uv-static/src/env_vars.rs`:163 on 2025-04-09 13:29_

```suggestion
    /// Equivalent to the `--no-editable` command-line argument. If set, uv
    /// installs any editable dependencies, including the project and any workspace members, as
    /// non-editable.
```

---

_@Gankra requested changes on 2025-04-09 13:34_

You can add tests for this by copying something like

https://github.com/astral-sh/uv/blob/980599f4fa4d03787331db578d515ebe772ec9d0/crates/uv/tests/it/sync.rs#L5329

and changing the `.arg("--no-editable")` to `.env(EnvVars::UV_NO_EDITABLE, "1")`

---

_@Gankra approved on 2025-04-10 19:55_

thanks!

---

_Merged by @Gankra on 2025-04-10 19:56_

---

_Closed by @Gankra on 2025-04-10 19:56_

---

_Label `enhancement` added by @Gankra on 2025-04-10 19:56_

---

_Label `cli` added by @Gankra on 2025-04-10 19:56_

---

_Renamed from "Add env arg UV_NO_EDITABLE" to "Add `UV_NO_EDITABLE` environment variable to set `--no-editable` on all invocations" by @Gankra on 2025-04-10 19:56_

---

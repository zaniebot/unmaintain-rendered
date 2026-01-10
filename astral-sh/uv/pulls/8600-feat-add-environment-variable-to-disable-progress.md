```yaml
number: 8600
title: "feat: add environment variable to disable progress output"
type: pull_request
state: merged
author: zzztimbo
labels:
  - configuration
assignees: []
merged: true
base: main
head: feat-enable-configuration-of-no-progress-with-env-var
created_at: 2024-10-26T19:58:39Z
updated_at: 2024-10-27T19:14:13Z
url: https://github.com/astral-sh/uv/pull/8600
synced_at: 2026-01-10T12:54:13Z
```

# feat: add environment variable to disable progress output

---

_Pull request opened by @zzztimbo on 2024-10-26 19:58_

The changes in this commit introduce the `UV_NO_PROGRESS` environment variable as an alternative way to control progress output suppression in uv-cli, equivalent to using the `--no-progress` flag. This enhancement simplifies configuration in CI environments and automated scripts by eliminating the need to detect whether the script is running in a CI environment.

Previously, disabling progress output required either passing the `--no-progress` flag directly or implementing script logic to detect CI environments and conditionally add the flag. With this change, users can now simply set `UV_NO_PROGRESS=true` in their environment to achieve the same effect.

The changes include:

- Adding the `UV_NO_PROGRESS` environment variable to the `EnvVars` struct in `crates/uv-static/src/env_vars.rs`.
- Updating the `GlobalArgs` struct in `crates/uv-cli/src/lib.rs` to include a new `no_progress` field that is bound to the `UV_NO_PROGRESS` environment variable.
- Adding documentation for the new `UV_NO_PROGRESS` environment variable in `docs/configuration/environment.md`.
## Test Plan

After creating a uv project using `uv init` in a temp directory in this project:
```
cargo run cache clean && cargo run venv && UV_NO_PROGRESS=false cargo run sync 
cargo run cache clean && cargo run venv && cargo run sync  
```
produce the expected default behavior 

```
cargo run cache clean && cargo run venv && UV_NO_PROGRESS=false cargo run sync  
```
produces the same behavior as having the `--no-progress` flag. 

---

_@zanieb reviewed on 2024-10-27 00:44_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:235 on 2024-10-27 00:44_

Is there a reason it overrides with `no_progress`? Do you mean `"progress"` there?

---

_@zanieb approved on 2024-10-27 00:45_

Thanks for the detailed description!

---

_Label `configuration` added by @zanieb on 2024-10-27 00:45_

---

_@zzztimbo reviewed on 2024-10-27 17:17_

---

_Review comment by @zzztimbo on `crates/uv-cli/src/lib.rs`:235 on 2024-10-27 17:17_

Good point. I did a copy paste of the `preview` field and didn't think very hard. I removed the `overrides_with` parameter because there isn't a `progress` field. 

---

_Review requested from @zanieb by @zzztimbo on 2024-10-27 17:18_

---

_Merged by @charliermarsh on 2024-10-27 19:14_

---

_Closed by @charliermarsh on 2024-10-27 19:14_

---

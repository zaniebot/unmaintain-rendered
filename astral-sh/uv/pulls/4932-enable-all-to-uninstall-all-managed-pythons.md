```yaml
number: 4932
title: "Enable `--all` to uninstall all managed Pythons"
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/uninstall
created_at: 2024-07-09T18:02:25Z
updated_at: 2024-07-09T18:15:17Z
url: https://github.com/astral-sh/uv/pull/4932
synced_at: 2026-01-10T13:42:52Z
```

# Enable `--all` to uninstall all managed Pythons

---

_Pull request opened by @charliermarsh on 2024-07-09 18:02_

## Summary

Allows `--all` as an alternative to specifying specific targets.

## Test Plan

Verified that `cargo run python uninstall` still fails.

```
❯ cargo run python uninstall --all
   Compiling uv-cli v0.0.1 (/Users/crmarsh/workspace/puffin/crates/uv-cli)
   Compiling uv v0.2.23 (/Users/crmarsh/workspace/puffin/crates/uv)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 2.86s
     Running `target/debug/uv python uninstall --all`
warning: `uv python uninstall` is experimental and may change without warning.
Searching for Python installations
Found existing installation: cpython-3.9.18-macos-aarch64-none
Found existing installation: cpython-3.8.18-macos-aarch64-none
Found existing installation: cpython-3.8.12-macos-aarch64-none
Found existing installation: cpython-3.12.1-macos-aarch64-none
Found existing installation: cpython-3.11.7-macos-aarch64-none
Found existing installation: cpython-3.10.13-macos-aarch64-none
Uninstalled cpython-3.10.13-macos-aarch64-none
Uninstalled cpython-3.11.7-macos-aarch64-none
Uninstalled cpython-3.12.1-macos-aarch64-none
Uninstalled cpython-3.8.12-macos-aarch64-none
Uninstalled cpython-3.8.18-macos-aarch64-none
Uninstalled cpython-3.9.18-macos-aarch64-none
Uninstalled 6 versions in 479ms
```


---

_Review requested from @zanieb by @charliermarsh on 2024-07-09 18:02_

---

_Label `preview` added by @charliermarsh on 2024-07-09 18:02_

---

_@zanieb reviewed on 2024-07-09 18:07_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/uninstall.rs`:99 on 2024-07-09 18:07_

Why this change?

---

_@zanieb reviewed on 2024-07-09 18:08_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/uninstall.rs`:99 on 2024-07-09 18:08_

Oh I see nevermind

---

_@zanieb reviewed on 2024-07-09 18:08_

---

_Review comment by @zanieb on `crates/uv/src/settings.rs`:379 on 2024-07-09 18:08_

We should mark these as conflicting right?

---

_@zanieb reviewed on 2024-07-09 18:09_

---

_Review comment by @zanieb on `crates/uv/src/settings.rs`:379 on 2024-07-09 18:09_

Omg sorry I'm dumb I looked at the settings struct. Bad review comments...

---

_@zanieb approved on 2024-07-09 18:09_

---

_@charliermarsh reviewed on 2024-07-09 18:10_

---

_Review comment by @charliermarsh on `crates/uv/src/settings.rs`:379 on 2024-07-09 18:10_

Lol appreciate you!

---

_Merged by @charliermarsh on 2024-07-09 18:15_

---

_Closed by @charliermarsh on 2024-07-09 18:15_

---

_Branch deleted on 2024-07-09 18:15_

---

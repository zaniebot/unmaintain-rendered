```yaml
number: 9706
title: "Replace executables with broken symlinks during `uv python install`"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/link-invalid-install
created_at: 2024-12-07T16:49:10Z
updated_at: 2024-12-10T22:39:24Z
url: https://github.com/astral-sh/uv/pull/9706
synced_at: 2026-01-12T16:08:56Z
```

# Replace executables with broken symlinks during `uv python install`

---

_@zanieb_

I somehow got in a state where we'd fail to install with

```
error: Failed to install cpython-3.13.0-macos-aarch64-none
  Caused by: Executable already exists at `/Users/zb/.local/bin/python3` but is not managed by uv; use `--force` to replace it
error: Failed to install cpython-3.13.0-macos-aarch64-none
  Caused by: Executable already exists at `/Users/zb/.local/bin/python` but is not managed by uv; use `--force` to replace it
```

but `python` / `python3` _were_ managed by uv, they just were linked to an installation that was deleted.

This updates the logic to replace broken executables that are broken symlinks. We apply this to broken links regardless of whether or not we think the target is managed by uv.

---

_Label `preview` added by @zanieb on 2024-12-07 16:49_

---

_Marked ready for review by @zanieb on 2024-12-10 20:27_

---

_@charliermarsh reviewed on 2024-12-10 20:43_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:358 on 2024-12-10 20:43_

I think you could write this as:
```rust
target.read_link().and_then(|target| target.try_exists()).inspect_err(...).unwrap_or(true)
```

---

_@charliermarsh reviewed on 2024-12-10 20:43_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:367 on 2024-12-10 20:43_

`anyf`

---

_@charliermarsh reviewed on 2024-12-10 20:43_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:386 on 2024-12-10 20:43_

This logic only runs on Linux, right? It doesn't look like it's gated, but I assume the error it's handling only runs on Linux?

---

_@charliermarsh approved on 2024-12-10 20:43_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:400 on 2024-12-10 20:44_

Maybe "Replacing invalid symlink at `{}`"?

---

_@charliermarsh reviewed on 2024-12-10 20:44_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:386 on 2024-12-10 20:58_

I think this technically would run on Windows too (which is valid, you _could_ have a broken symlink it's just rare).

---

_@zanieb reviewed on 2024-12-10 20:58_

---

_@zanieb reviewed on 2024-12-10 20:58_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:400 on 2024-12-10 20:58_

Hm I was sticking with the existing style but I do like that wording.

---

_@zanieb reviewed on 2024-12-10 20:58_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:367 on 2024-12-10 20:58_

```suggestion
                    // Figure out what installation it references, if any
```

---

_@zanieb reviewed on 2024-12-10 20:59_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:358 on 2024-12-10 20:59_

True! Not really any better imo but happy to change it.

---

_@zanieb reviewed on 2024-12-10 22:19_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:358 on 2024-12-10 22:19_

It does format nicely :)

---

_Merged by @zanieb on 2024-12-10 22:39_

---

_Closed by @zanieb on 2024-12-10 22:39_

---

_Branch deleted on 2024-12-10 22:39_

---

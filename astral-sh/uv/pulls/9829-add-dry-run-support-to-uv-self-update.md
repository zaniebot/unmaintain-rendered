```yaml
number: 9829
title: "Add `--dry-run` support to `uv self update`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/self-up-dry
created_at: 2024-12-12T01:28:09Z
updated_at: 2025-05-04T21:54:39Z
url: https://github.com/astral-sh/uv/pull/9829
synced_at: 2026-01-12T16:08:59Z
```

# Add `--dry-run` support to `uv self update`

---

_@zanieb_

See commentary at https://github.com/astral-sh/uv/issues/9828#issuecomment-2537542100 regarding the limitations and future upstream changes needed.

```
❯ cargo build --features self-update
   Compiling uv v0.5.8 (/Users/zb/workspace/uv/crates/uv)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 7.28s
❯ cp ./target/debug/uv ~/.cargo/bin
❯ uv self update --dry-run
info: Checking for updates...
Nothing to do. You're on the latest version of uv (v0.5.8)
❯ uv self update --dry-run 0.5.7
info: Checking for updates...
Would update uv from v0.5.8 to v0.5.7
❯ vi ~/.config/uv/uv-receipt.json  # Edit the receipt to think its on an older version
❯ uv self update --dry-run
info: Checking for updates...
Would update uv from v0.5.8 to the latest version
```

---

_Label `enhancement` added by @zanieb on 2024-12-12 01:28_

---

_Review requested from @charliermarsh by @zanieb on 2024-12-13 04:01_

---

_@charliermarsh reviewed on 2024-12-13 04:12_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/self_update.rs`:116 on 2024-12-13 04:12_

I might remove "Nothing to do".

---

_@charliermarsh reviewed on 2024-12-13 04:13_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:517 on 2024-12-13 04:13_

Might nix this second sentence personally.

---

_@charliermarsh approved on 2024-12-13 04:13_

---

_Merged by @charliermarsh on 2025-05-04 21:54_

---

_Closed by @charliermarsh on 2025-05-04 21:54_

---

_Branch deleted on 2025-05-04 21:54_

---

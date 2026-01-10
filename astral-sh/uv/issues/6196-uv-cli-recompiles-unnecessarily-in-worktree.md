```yaml
number: 6196
title: "`uv-cli` recompiles unnecessarily in worktree"
type: issue
state: closed
author: eth3lbert
labels:
  - internal
assignees: []
created_at: 2024-08-19T03:00:22Z
updated_at: 2024-08-29T18:11:52Z
url: https://github.com/astral-sh/uv/issues/6196
synced_at: 2026-01-10T04:45:09Z
```

# `uv-cli` recompiles unnecessarily in worktree

---

_Issue opened by @eth3lbert on 2024-08-19 03:00_

Currently, when working in a worktree, `uv-cli` still recompiles even when there are no changes. Running `cargo check --verbose` multiple times shows something similar to the following::

``` shell-session
...
       Fresh backtrace v0.3.73
       Fresh uv-dispatch v0.0.1 (/Users/eth/workspace/astral-sh/uv-dev/crates/uv-dispatch)
       Fresh tikv-jemalloc-sys v0.6.0+5.3.0-1-ge13ca993e8ccb9ba9847cc330696e02839f328f7
       Dirty uv-cli v0.0.1 (/Users/eth/workspace/astral-sh/uv-dev/crates/uv-cli): the file `.git/HEAD` is missing
   Compiling uv-cli v0.0.1 (/Users/eth/workspace/astral-sh/uv-dev/crates/uv-cli)
       Fresh nix v0.28.0
       Fresh clap_complete_command v0.6.1
       Fresh console v0.15.8
     Running `/Users/eth/workspace/astral-sh/uv-dev/target/debug/build/uv-cli-aeca9008583611ce/build-script-build`
       Fresh is_ci v1.2.0
       Fresh tikv-jemallocator v0.6.0
       Fresh ctrlc v3.4.4
...
```

This occurs because a worktree's `.git` is a file, not a directory, which means there is no `.git/HEAD` file for the worktree.


---

_Label `internal` added by @charliermarsh on 2024-08-19 13:54_

---

_Closed by @BurntSushi on 2024-08-29 18:11_

---

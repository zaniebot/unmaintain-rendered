```yaml
number: 4806
title: Check hash of downloaded python toolchain
type: pull_request
state: merged
author: j178
labels:
  - enhancement
  - security
assignees: []
merged: true
base: main
head: check-hash
created_at: 2024-07-04T14:12:24Z
updated_at: 2024-07-05T03:16:55Z
url: https://github.com/astral-sh/uv/pull/4806
synced_at: 2026-01-12T16:06:28Z
```

# Check hash of downloaded python toolchain

---

_@j178_

## Summary

Check the sha256 checksum when downloading a managed python toolchain.

## Test Plan

```sh
$ cargo run -- python install 3.12

warning: `uv python install` is experimental and may change without warning.
Looking for installation Python 3.12.3 (any-3.12.3-any-any-any)
Downloading cpython-3.12.3-windows-x86_64-none
Installed Python 3.12.3 to C:\Users\jo\AppData\Roaming\uv\data\python\cpython-3.12.3-windows-x86_64-none
Installed 1 installation in 6s

$ cargo run -- python uninstall 3.12

$ # manually change the hash in `crates/uv-python/src/downloads.inc`

$ cargo run -- python install 3.12

warning: `uv python install` is experimental and may change without warning.
Looking for installation Python 3.12 (any-3.12-any-any-any)
Downloading cpython-3.12.3-windows-x86_64-none
error: Hash mismatch for `cpython-3.12.3-windows-x86_64-none`

Expected:
xx

Computed:
776568c92c5f3b47dbf5f17c1c58578f70d75a32654419a158aa8bdc6f95b09a
```


---

_Comment by @zanieb on 2024-07-04 15:12_

Exciting to see this! Thank you :)

---

_Marked ready for review by @j178 on 2024-07-04 15:37_

---

_Comment by @zanieb on 2024-07-04 16:28_

@j178 Don't worry about those Windows stack overflows we're investigating that separately.

---

_@charliermarsh approved on 2024-07-04 17:39_

Thanks, looks great! Tested it too.

---

_Label `enhancement` added by @charliermarsh on 2024-07-04 17:39_

---

_Label `security` added by @charliermarsh on 2024-07-04 17:39_

---

_Merged by @charliermarsh on 2024-07-04 17:49_

---

_Closed by @charliermarsh on 2024-07-04 17:49_

---

_Branch deleted on 2024-07-05 03:16_

---

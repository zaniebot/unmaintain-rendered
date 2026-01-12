```yaml
number: 10046
title: Backtrack to non-local versions when wheels are missing platform support
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/incomplete-locals-only
created_at: 2024-12-20T03:01:18Z
updated_at: 2024-12-20T19:11:29Z
url: https://github.com/astral-sh/uv/pull/10046
synced_at: 2026-01-12T16:09:05Z
```

# Backtrack to non-local versions when wheels are missing platform support

---

_@charliermarsh_

## Summary

This is yet another variation on https://github.com/astral-sh/uv/pull/9928, with a few minor changes:

1. It only applies to local versions (e.g., `2.5.1+cpu`).
2. It only _considers_ the non-local version as an alternative (e.g., `2.5.1`).
3. It only _considers_ the non-local alternative if it _does_ support the unsupported platform.
4. Instead of failing, it falls back to using the local version.

So, this is far less strict, and is effectively designed to solve PyTorch but nothing else. It's also not user-configurable, except by way of using `environments` to exclude platforms.

---

_Label `bug` added by @charliermarsh on 2024-12-20 03:01_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-12-20 03:04_

---

_Review requested from @zanieb by @charliermarsh on 2024-12-20 03:04_

---

_Review requested from @konstin by @charliermarsh on 2024-12-20 03:04_

---

_Marked ready for review by @charliermarsh on 2024-12-20 03:04_

---

_Comment by @codspeed-hq[bot] on 2024-12-20 03:20_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Fincomplete-locals-only)

### Merging #10046 will **not alter performance**

<sub>Comparing <code>charlie/incomplete-locals-only</code> (6b79efd) with <code>main</code> (f3c5b63)</sub>



### Summary

`✅ 14` untouched benchmarks  





---

_@BurntSushi approved on 2024-12-20 14:33_

Given PyTorch's setup, I feel comfortable with this. But if it's workable, I like @konstin's idea to use the complement of the implied markers instead of hard-coding the Windows/macOS/Linux platforms.

---

_Merged by @charliermarsh on 2024-12-20 19:11_

---

_Closed by @charliermarsh on 2024-12-20 19:11_

---

_Branch deleted on 2024-12-20 19:11_

---

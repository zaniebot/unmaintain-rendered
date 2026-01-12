```yaml
number: 5845
title: Avoid reusing incompatible distributions across lock and sync
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/debug
created_at: 2024-08-07T00:04:12Z
updated_at: 2024-08-07T00:54:52Z
url: https://github.com/astral-sh/uv/pull/5845
synced_at: 2026-01-12T16:07:03Z
```

# Avoid reusing incompatible distributions across lock and sync

---

_@charliermarsh_

## Summary

We need to avoid using incompatible versions for build dependencies that are also part of the resolved
environment. This is a very subtle issue, but: when locking, we don't enforce platform
compatibility. So, if we reuse the resolver state to install, and the install itself has to
preform a resolution (e.g., for the build dependencies of a source distribution), that
resolution may choose incompatible versions.

The key property here is that there's a shared package between the build dependencies and the
project dependencies.

Closes https://github.com/astral-sh/uv/issues/5836.

---

_Comment by @codspeed-hq[bot] on 2024-08-07 00:42_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/debug)

### Merging #5845 will **not alter performance**

<sub>Comparing <code>charlie/debug</code> (910b5dd) with <code>main</code> (f3cc8e4)</sub>



### Summary

`✅ 13` untouched benchmarks






---

_Label `bug` added by @charliermarsh on 2024-08-07 00:42_

---

_Renamed from "Debug environment for NumPy mis-install" to "Avoid reusing incompatible distributions across lock and sync" by @charliermarsh on 2024-08-07 00:42_

---

_Marked ready for review by @charliermarsh on 2024-08-07 00:42_

---

_Comment by @charliermarsh on 2024-08-07 00:47_

This is fixable but require some work in the resolver.

(That performance regression isn't real, it's from me adding a bunch of debug logging.)

---

_Merged by @charliermarsh on 2024-08-07 00:54_

---

_Closed by @charliermarsh on 2024-08-07 00:54_

---

_Branch deleted on 2024-08-07 00:54_

---

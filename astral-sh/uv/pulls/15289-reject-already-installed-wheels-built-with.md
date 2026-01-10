```yaml
number: 15289
title: Reject already-installed wheels built with outdated settings
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - no-build
assignees: []
merged: true
base: main
head: charlie/build-info
created_at: 2025-08-14T20:38:49Z
updated_at: 2025-08-15T15:15:56Z
url: https://github.com/astral-sh/uv/pull/15289
synced_at: 2026-01-10T06:44:33Z
```

# Reject already-installed wheels built with outdated settings

---

_Pull request opened by @charliermarsh on 2025-08-14 20:38_

## Summary

With this PR, we track the settings that were used to build a wheel (`--config-settings`, plus any `extra-build-dependencies` or `extra-build-variables`) and write those to the `.dist-info` directory upon install. This then allows us to "reject" already-installed wheels, if the user changes the build dependencies or `--config-settings` (or, crucially, if they use `match-runtime = true` and the resolution changes).

Closes https://github.com/astral-sh/uv/issues/15218.


---

_Label `no-build` added by @charliermarsh on 2025-08-14 20:38_

---

_Label `no-test` added by @charliermarsh on 2025-08-14 20:38_

---

_Comment by @charliermarsh on 2025-08-14 20:43_

(Needs tests, still iterating on the abstractions.)

---

_Label `no-test` removed by @charliermarsh on 2025-08-14 22:42_

---

_Review requested from @zanieb by @charliermarsh on 2025-08-14 22:47_

---

_Marked ready for review by @charliermarsh on 2025-08-14 22:47_

---

_Label `bug` added by @charliermarsh on 2025-08-14 22:47_

---

_@zanieb reviewed on 2025-08-15 13:41_

---

_Review comment by @zanieb on `crates/uv-distribution/src/index/built_wheel_index.rs`:139 on 2025-08-15 13:41_

I would probably implement this as `let cache_shard = build_info.cache_shard().and_then(cache_shard.shard).unwrap_or(cache_shard)` if the compiler will let you.

---

_@zanieb approved on 2025-08-15 13:43_

---

_Merged by @charliermarsh on 2025-08-15 15:15_

---

_Closed by @charliermarsh on 2025-08-15 15:15_

---

_Branch deleted on 2025-08-15 15:15_

---

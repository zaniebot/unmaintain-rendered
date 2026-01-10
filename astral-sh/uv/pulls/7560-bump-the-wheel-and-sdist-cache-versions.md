```yaml
number: 7560
title: Bump the wheel and sdist cache versions
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/bucket
created_at: 2024-09-19T19:39:55Z
updated_at: 2024-09-19T19:53:07Z
url: https://github.com/astral-sh/uv/pull/7560
synced_at: 2026-01-10T12:53:50Z
```

# Bump the wheel and sdist cache versions

---

_Pull request opened by @charliermarsh on 2024-09-19 19:39_

## Summary

Both of these can contain rkyv data in their HTTP cache envelopes. As such, the entries aren't readable by earlier versions of uv, and `uv cache prune` can break. I should make `uv cache prune` robust to this, but this feels safest.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-09-19 19:39_

---

_Review requested from @zanieb by @charliermarsh on 2024-09-19 19:39_

---

_@charliermarsh reviewed on 2024-09-19 19:40_

---

_Review comment by @charliermarsh on `crates/uv-cache/src/lib.rs`:762 on 2024-09-19 19:40_

I want to keep this short because it actually matters for Windows file path lengths right now.

---

_@zanieb approved on 2024-09-19 19:42_

---

_@BurntSushi approved on 2024-09-19 19:45_

LGTM. Are we sure these are the only two that need to be bumped? Does anything else used the cached client? (I know `simple` does, but I bumped at least that one.)

---

_Comment by @charliermarsh on 2024-09-19 19:47_

I believe so, yes... The only other one that would be a "maybe" is the interpreter bucket, but I checked and that uses MsgPack directly.

---

_Merged by @charliermarsh on 2024-09-19 19:53_

---

_Closed by @charliermarsh on 2024-09-19 19:53_

---

_Branch deleted on 2024-09-19 19:53_

---

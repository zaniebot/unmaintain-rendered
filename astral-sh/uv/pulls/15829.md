```yaml
number: 15829
title: Improve BSD tag construction
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/bsd
created_at: 2025-09-14T01:38:39Z
updated_at: 2025-09-14T14:48:39Z
url: https://github.com/astral-sh/uv/pull/15829
synced_at: 2026-01-10T06:36:15Z
```

# Improve BSD tag construction

---

_Pull request opened by @charliermarsh on 2025-09-14 01:38_

## Summary

I had to use ChatGPT to help with my research on the "correct" architecture names for these platforms; there could still be some rough edges, but this seems like an improvement.

Closes https://github.com/astral-sh/uv/issues/15799.


---

_Label `bug` added by @charliermarsh on 2025-09-14 01:38_

---

_Marked ready for review by @charliermarsh on 2025-09-14 01:38_

---

_Review requested from @geofft by @charliermarsh on 2025-09-14 01:39_

---

_Review requested from @konstin by @charliermarsh on 2025-09-14 01:39_

---

_Renamed from "Improve FreeBSD tag construction" to "Improve BSD tag construction" by @charliermarsh on 2025-09-14 01:39_

---

_Review comment by @konstin on `crates/uv-platform-tags/src/platform.rs`:221 on 2025-09-14 11:13_

maturin uses a different table: https://github.com/PyO3/maturin/blob/8ab42219247277fee513eac753a3e90e76cd46b9/src/target/mod.rs#L128-L149

---

_Review comment by @konstin on `crates/uv-platform-tags/src/platform.rs`:241 on 2025-09-14 11:14_

maturin uses the arch name for all BSDs, only the platform release casing is different: https://github.com/PyO3/maturin/blob/6ceb4219b166157303cb041e3361d27ef8c089b4/src/build_context.rs#L626-L646

---

_@konstin reviewed on 2025-09-14 11:19_

I would prefer to use maturin's implementation as reference, it has some users and got user contributed fixes. maturin also has the `sysconfig` folder with snapshots of the configuration from real BSD system that we can use as reference..

---

_Comment by @charliermarsh on 2025-09-14 13:20_

Fixed, thanks @konstin.

---

_@charliermarsh reviewed on 2025-09-14 13:22_

---

_Review comment by @charliermarsh on `crates/uv-platform-tags/src/platform.rs`:228 on 2025-09-14 13:22_

@konstin -- We can just wait until we get user feedback on it, but I'm surprised by this, I would've expected `powerpc64`.

---

_Review requested from @konstin by @charliermarsh on 2025-09-14 13:23_

---

_@zanieb approved on 2025-09-14 13:32_

---

_@konstin approved on 2025-09-14 14:38_

---

_Merged by @charliermarsh on 2025-09-14 14:48_

---

_Closed by @charliermarsh on 2025-09-14 14:48_

---

_Branch deleted on 2025-09-14 14:48_

---

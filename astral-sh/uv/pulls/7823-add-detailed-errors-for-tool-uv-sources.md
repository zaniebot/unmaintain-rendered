```yaml
number: 7823
title: "Add detailed errors for `tool.uv.sources` deserialization failures"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/serde-untagged
created_at: 2024-09-30T23:55:45Z
updated_at: 2024-10-01T15:49:08Z
url: https://github.com/astral-sh/uv/pull/7823
synced_at: 2026-01-10T12:53:56Z
```

# Add detailed errors for `tool.uv.sources` deserialization failures

---

_Pull request opened by @charliermarsh on 2024-09-30 23:55_

## Summary

Closes https://github.com/astral-sh/uv/issues/7817.


---

_Label `error messages` added by @charliermarsh on 2024-09-30 23:55_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-09-30 23:55_

---

_Review requested from @konstin by @charliermarsh on 2024-09-30 23:55_

---

_Marked ready for review by @charliermarsh on 2024-09-30 23:55_

---

_@charliermarsh reviewed on 2024-09-30 23:56_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/requires_dist.rs`:375 on 2024-09-30 23:56_

This part I don't quite understand. It looks like https://github.com/dtolnay/serde-untagged/issues/2.

---

_Converted to draft by @charliermarsh on 2024-10-01 00:03_

---

_Marked ready for review by @charliermarsh on 2024-10-01 00:09_

---

_@charliermarsh reviewed on 2024-10-01 00:12_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/requires_dist.rs`:375 on 2024-10-01 00:12_

This is really bothering me...

---

_@konstin approved on 2024-10-01 06:56_

---

_Review comment by @BurntSushi on `crates/uv-distribution/src/metadata/requires_dist.rs`:375 on 2024-10-01 13:13_

Yeah annoying, but still an improvement. :)

A solution here isn't obvious to me. (Other than hand-rolling a `Deserialize` impl.)

---

_@BurntSushi approved on 2024-10-01 13:14_

Nice!

---

_Merged by @charliermarsh on 2024-10-01 15:49_

---

_Closed by @charliermarsh on 2024-10-01 15:49_

---

_Branch deleted on 2024-10-01 15:49_

---

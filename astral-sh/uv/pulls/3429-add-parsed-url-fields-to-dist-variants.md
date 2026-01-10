```yaml
number: 3429
title: " Add parsed URL fields to `Dist` variants "
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/add-parsed-information-to-dist
created_at: 2024-05-07T16:12:26Z
updated_at: 2024-05-14T01:23:29Z
url: https://github.com/astral-sh/uv/pull/3429
synced_at: 2026-01-10T14:37:54Z
```

#  Add parsed URL fields to `Dist` variants 

---

_Pull request opened by @konstin on 2024-05-07 16:12_

Avoid reparsing urls by storing the parsed parts across resolution on `Dist`.

Part 2 of https://github.com/astral-sh/uv/issues/3408 and part of #3409

Closes #3408

---

_Label `internal` added by @konstin on 2024-05-07 16:12_

---

_Converted to draft by @konstin on 2024-05-07 16:13_

---

_Renamed from " Store parsed url fields in Dist variants " to " Add parsed url fields to Dist variants " by @konstin on 2024-05-08 13:20_

---

_Marked ready for review by @konstin on 2024-05-08 13:41_

---

_@konstin reviewed on 2024-05-08 17:57_

---

_Review comment by @konstin on `crates/distribution-types/src/lib.rs`:175 on 2024-05-08 17:57_

The intention is to eventually remove the `VerbatimUrl`, leaving only `given: String`, for now we just eliminate the fallible re-parsing of urls

---

_@konstin reviewed on 2024-05-08 17:58_

---

_Review comment by @konstin on `crates/distribution-types/src/lib.rs`:214 on 2024-05-08 17:58_

Without boxing, this would increase the size of the entire `Dist`

---

_@konstin reviewed on 2024-05-08 18:00_

---

_Review comment by @konstin on `crates/distribution-types/src/lib.rs`:1138 on 2024-05-08 18:00_

I have a branch boxing the entire `Dist` struct, but it didn't help with the windows debug stack overflows.

---

_@konstin reviewed on 2024-05-08 18:00_

---

_Review comment by @konstin on `crates/uv-dev/src/wheel_metadata.rs`:34 on 2024-05-08 18:00_

This is just the dev script

---

_Review comment by @konstin on `crates/uv-requirements/src/lookahead.rs`:185 on 2024-05-08 18:03_

This was introduced with https://github.com/astral-sh/uv/commit/cf3093283100ef7ff916ce2b7e9cc0061adb2886#diff-49326157b193e4affae1b7a1c56c7c7e1ea2039168e33948b6c30a4fe7f4ef9eR76, where the `Dist::from_url` dropped the information. Is there an argument that the lookahead should or shouldn't consider editables?

---

_@konstin reviewed on 2024-05-08 18:03_

---

_Renamed from " Add parsed url fields to Dist variants " to " Add parsed URL fields to `Dist` variants " by @charliermarsh on 2024-05-14 01:15_

---

_@charliermarsh reviewed on 2024-05-14 01:15_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/lookahead.rs`:185 on 2024-05-14 01:15_

I think it's because we currently assume editables can't be transitive deps.

---

_Merged by @charliermarsh on 2024-05-14 01:23_

---

_Closed by @charliermarsh on 2024-05-14 01:23_

---

_Branch deleted on 2024-05-14 01:23_

---

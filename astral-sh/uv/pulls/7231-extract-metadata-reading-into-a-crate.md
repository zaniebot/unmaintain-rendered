```yaml
number: 7231
title: Extract METADATA reading into a crate
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/split-out-uv-metadata
created_at: 2024-09-09T20:36:55Z
updated_at: 2024-09-10T13:31:04Z
url: https://github.com/astral-sh/uv/pull/7231
synced_at: 2026-01-12T16:07:44Z
```

# Extract METADATA reading into a crate

---

_@konstin_

This is preparatory work for the upload functionality, which needs to read the METADATA file and attach its parsed contents to the POST request: We move finding the `.dist-info` from `install-wheel-rs` and `uv-client` to a new `uv-metadata` crate, so it can be shared with the publish crate.

I don't properly know if its the right place since the upload code isn't ready, but i'm PR-ing it now because it already had merge conflicts.

---

_Label `internal` added by @konstin on 2024-09-09 20:36_

---

_@charliermarsh approved on 2024-09-09 20:55_

---

_@konstin reviewed on 2024-09-10 13:26_

---

_Review comment by @konstin on `crates/uv/tests/pip_sync.rs`:2626 on 2024-09-10 13:26_

This is a regression from the pervious error message, but fixing it requires propagating changes through a bunch of error types so i'll be following up next week.

---

_Merged by @konstin on 2024-09-10 13:31_

---

_Closed by @konstin on 2024-09-10 13:31_

---

_Branch deleted on 2024-09-10 13:31_

---

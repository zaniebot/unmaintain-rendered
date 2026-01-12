```yaml
number: 1762
title: "feat: add installer to `InstalledDist`"
type: pull_request
state: merged
author: tdejager
labels:
  - internal
assignees: []
merged: true
base: main
head: feat/add-installer
created_at: 2024-02-20T15:36:52Z
updated_at: 2024-02-20T17:07:09Z
url: https://github.com/astral-sh/uv/pull/1762
synced_at: 2026-01-12T16:04:43Z
```

# feat: add installer to `InstalledDist`

---

_@tdejager_

## Summary

Add `installer` method to `InstalledDist` to distinguish between different installers. Might be nice to add an enum for all possible installers, but this might be too hard to keep up to date :).

The `INSTALLER` file is a file that can be added to the `.dist-info` folder with the installer name.

Closes: #1759 

## Test Plan

Not sure if there is a place I can automatically test it, if you have a pointer I would be happy to add a test.




---

_@charliermarsh reviewed on 2024-02-20 16:12_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/installed.rs`:115 on 2024-02-20 16:12_

Do you mind making this `Result<Option<String>>`, and erroring if the error kind is anything other than "not found"?

---

_@charliermarsh reviewed on 2024-02-20 16:16_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/installed.rs`:115 on 2024-02-20 16:16_

(I feel like the `direct_url` method above should function similarly, but that doesn't need to be changed here.)

---

_@charliermarsh reviewed on 2024-02-20 16:57_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/installed.rs`:115 on 2024-02-20 16:57_

Gonna change this so that I can include it in the release.

---

_Label `internal` added by @charliermarsh on 2024-02-20 16:57_

---

_@tdejager reviewed on 2024-02-20 17:00_

---

_Review comment by @tdejager on `crates/distribution-types/src/installed.rs`:115 on 2024-02-20 17:00_

Sure thanks!

---

_Merged by @charliermarsh on 2024-02-20 17:07_

---

_Closed by @charliermarsh on 2024-02-20 17:07_

---

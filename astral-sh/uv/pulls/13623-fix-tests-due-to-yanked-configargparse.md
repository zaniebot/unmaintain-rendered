```yaml
number: 13623
title: Fix tests due to yanked configargparse
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/fix-yanked-test
created_at: 2025-05-23T18:35:13Z
updated_at: 2025-05-23T18:46:53Z
url: https://github.com/astral-sh/uv/pull/13623
synced_at: 2026-01-10T11:10:42Z
```

# Fix tests due to yanked configargparse

---

_Pull request opened by @konstin on 2025-05-23 18:35_

The release of configargparse locked in the tests was yanked, we fix this by updating the snapshots.

---

_Label `internal` added by @konstin on 2025-05-23 18:35_

---

_@charliermarsh approved on 2025-05-23 18:36_

---

_Comment by @charliermarsh on 2025-05-23 18:37_

If it gets unyanked, would we need to update again?

---

_Merged by @konstin on 2025-05-23 18:43_

---

_Closed by @konstin on 2025-05-23 18:43_

---

_Branch deleted on 2025-05-23 18:43_

---

_Comment by @konstin on 2025-05-23 18:46_

As I understood it, yanking is intended as a final action, with users migrating either to the previous release, if the new release was accidental, or to the next release, if the problem was fixed there. configargparse made the new 1.7.1 release to replace the broken 1.7 (https://github.com/bw2/ConfigArgParse/issues/300#issuecomment-2904729288:

> fixed metadata through Tristan merging PR https://github.com/bw2/ConfigArgParse/pull/306 , yanked versions with incorrect metadata (v1.5.5 and v1.7) from PyPI, tagged and released a new version (v1.7.1)

Regular users can update to that release, but due to our exclude newer test filter, we have to downgrade to the older version.

---

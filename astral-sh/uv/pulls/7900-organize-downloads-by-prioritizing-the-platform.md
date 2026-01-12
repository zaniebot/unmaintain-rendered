```yaml
number: 7900
title: Organize downloads by prioritizing the platform, then the architecture.
type: pull_request
state: merged
author: j178
labels:
  - internal
assignees: []
merged: true
base: main
head: triple-sort
created_at: 2024-10-03T15:29:19Z
updated_at: 2024-10-03T15:57:11Z
url: https://github.com/astral-sh/uv/pull/7900
synced_at: 2026-01-12T16:08:04Z
```

# Organize downloads by prioritizing the platform, then the architecture.

---

_@j178_

## Summary

PythonDownloadKey (cpython-3.13.0rc3-darwin-aarch64-none) and PlatformTriple in `fetch-download-metadata.py` have a slight inconsistency in the ordering of `os` and `arch`. In PythonDownloadKey, `os` precedes `arch`, while in PlatformTriple, `arch` comes before `platform` (equivalent to os). This difference in ordering affects the sorting logic, giving arch higher priority than platform in the `download-metadata.json` file, leading to a little bit of unexpected order of entries.

Before:
<img width="676" alt="image" src="https://github.com/user-attachments/assets/adb24a2e-da70-4a09-a702-4b5d71600b2c">
After:
<img width="725" alt="image" src="https://github.com/user-attachments/assets/c6c76e6a-d3fd-43dc-bfb0-b3a4a3fe2b6b">



---

_Assigned to @zanieb by @zanieb on 2024-10-03 15:50_

---

_Label `internal` added by @zanieb on 2024-10-03 15:50_

---

_@zanieb approved on 2024-10-03 15:50_

Thanks!

---

_Merged by @zanieb on 2024-10-03 15:51_

---

_Closed by @zanieb on 2024-10-03 15:51_

---

_Branch deleted on 2024-10-03 15:57_

---

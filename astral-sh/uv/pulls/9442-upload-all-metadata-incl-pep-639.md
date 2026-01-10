```yaml
number: 9442
title: "Upload: All metadata incl. PEP 639"
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: konsti/upload-all-metadata
created_at: 2024-11-26T14:37:28Z
updated_at: 2024-11-26T23:25:10Z
url: https://github.com/astral-sh/uv/pull/9442
synced_at: 2026-01-10T12:00:00Z
```

# Upload: All metadata incl. PEP 639

---

_Pull request opened by @konstin on 2024-11-26 14:37_

We were previously not uploading all metadata in the formdata of an upload request in the legacy api. Notably, we were missing the PEP 639 license-files field.

I had to switch to pdm due to https://github.com/pypa/hatch/issues/1828

---

_Label `preview` added by @konstin on 2024-11-26 14:37_

---

_Marked ready for review by @konstin on 2024-11-26 14:56_

---

_Label `bug` added by @konstin on 2024-11-26 14:56_

---

_Merged by @konstin on 2024-11-26 23:25_

---

_Closed by @konstin on 2024-11-26 23:25_

---

_Branch deleted on 2024-11-26 23:25_

---

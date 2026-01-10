```yaml
number: 5390
title: "Update `python-build-standalone` workflow"
type: pull_request
state: merged
author: j178
labels:
  - internal
assignees: []
merged: true
base: main
head: action
created_at: 2024-07-24T01:20:00Z
updated_at: 2024-07-24T02:42:03Z
url: https://github.com/astral-sh/uv/pull/5390
synced_at: 2026-01-10T13:37:23Z
```

# Update `python-build-standalone` workflow

---

_Pull request opened by @j178 on 2024-07-24 01:20_

## Summary

After #5337, `fetch-download-metadata.py` fetches not just from `python-build-standalone`, so updates the workflow to `sync-python-releases.yml`.

Also includes `crates/uv-python/download-metadata.json` in `add-paths`.


---

_Review comment by @zanieb on `.github/workflows/sync-python-releases.yml`:4 on 2024-07-24 02:01_

Fwiw we don't capitalize all the words in most of our other jobs with names like this.

Wdyt of "Sync Python downloads"?

```suggestion
name: "Sync Python downloads"
```

---

_@zanieb reviewed on 2024-07-24 02:01_

---

_@zanieb approved on 2024-07-24 02:01_

---

_Label `internal` added by @zanieb on 2024-07-24 02:01_

---

_Comment by @zanieb on 2024-07-24 02:08_

Appreciate the follow-through!

---

_Merged by @zanieb on 2024-07-24 02:11_

---

_Closed by @zanieb on 2024-07-24 02:11_

---

_Review comment by @j178 on `.github/workflows/sync-python-releases.yml`:35 on 2024-07-24 02:14_

an extra quote here…

---

_@j178 reviewed on 2024-07-24 02:14_

---

_Branch deleted on 2024-07-24 02:42_

---

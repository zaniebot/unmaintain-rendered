```yaml
number: 588
title: Restructure Git cache to include package name
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/git
created_at: 2023-12-08T00:26:59Z
updated_at: 2023-12-08T01:17:42Z
url: https://github.com/astral-sh/uv/pull/588
synced_at: 2026-01-10T15:44:44Z
```

# Restructure Git cache to include package name

---

_Pull request opened by @charliermarsh on 2023-12-08 00:26_

## Summary

This PR modifies the Git wheel cache to: (1) use a shorter version of the SHA, to save space; and (2) include the package name, for consistency with all other buckets.

I considered removing the URL hash entirely, and _just_ using the SHA, which would be even _more_ consistent with other buckets. But if we remove the URL, then we won't have separate directories for subdirectories (which are part of the URL).

Before:

<img width="1035" alt="Screen Shot 2023-12-07 at 7 23 42 PM" src="https://github.com/astral-sh/puffin/assets/1309177/86afce67-682f-464f-9ba1-0b60d5b7f19f">

After:

<img width="1232" alt="Screen Shot 2023-12-07 at 8 09 23 PM" src="https://github.com/astral-sh/puffin/assets/1309177/eda42a19-974f-47fe-8c83-54a602ddfd2d">



---

_Renamed from "Remove extra hash layer in Git wheel cache" to "Restructure Git cache to include package name" by @charliermarsh on 2023-12-08 01:09_

---

_Marked ready for review by @charliermarsh on 2023-12-08 01:11_

---

_Label `internal` added by @charliermarsh on 2023-12-08 01:11_

---

_Merged by @charliermarsh on 2023-12-08 01:17_

---

_Closed by @charliermarsh on 2023-12-08 01:17_

---

_Branch deleted on 2023-12-08 01:17_

---

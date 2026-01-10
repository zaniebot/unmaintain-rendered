---
number: 195
title: Keep track of in-flight downloads
type: issue
state: closed
author: konstin
labels:
  - internal
assignees: []
created_at: 2023-10-26T11:40:27Z
updated_at: 2024-01-16T05:38:18Z
url: https://github.com/astral-sh/uv/issues/195
synced_at: 2026-01-10T01:23:03Z
---

# Keep track of in-flight downloads

---

_Issue opened by @konstin on 2023-10-26 11:40_

Like with resolver requests, we should make sure that we only schedule one download at a time. This is e.g. relevant when obtaining the build requirements for multiple source distributions at once.

---

_Comment by @charliermarsh on 2023-10-26 13:26_

This also needs to be global, right? Or at least, shared between the installer, resolver, and source distribution builds, and any recursive tasks between those?

---

_Comment by @konstin on 2023-10-26 13:32_

yes, we'll need some kind of `DownloadClient` we keep on the `BuildDispatch`

---

_Comment by @charliermarsh on 2024-01-16 05:38_

I believe this is done. We moved the in-flight map up one level.

---

_Closed by @charliermarsh on 2024-01-16 05:38_

---

_Label `internal` added by @charliermarsh on 2024-01-16 05:38_

---

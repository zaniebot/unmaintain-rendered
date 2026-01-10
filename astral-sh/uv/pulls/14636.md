```yaml
number: 14636
title: Skip Windows Python interpreters that return a broken MSIX package code
type: pull_request
state: merged
author: zanieb
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: zb/corrupt-package
created_at: 2025-07-15T19:49:21Z
updated_at: 2025-07-15T21:47:36Z
url: https://github.com/astral-sh/uv/pull/14636
synced_at: 2026-01-10T06:53:02Z
```

# Skip Windows Python interpreters that return a broken MSIX package code

---

_Pull request opened by @zanieb on 2025-07-15 19:49_

Currently we treat all spawn failures as fatal, because they indicate a broken interpreter. In this case, I think we should just skip these broken interpreters — though I don't know the root cause of why it's broken yet.

Closes https://github.com/astral-sh/uv/issues/14637
See https://discord.com/channels/1039017663004942429/1039017663512449056/1394758502647333025

---

_Label `bug` added by @zanieb on 2025-07-15 19:49_

---

_@charliermarsh approved on 2025-07-15 19:49_

---

_Label `windows` added by @charliermarsh on 2025-07-15 19:50_

---

_Renamed from "Skip Windows Python interpreters that return a `APPMODEL_ERROR_NO_PACKAGE` code" to "Skip Windows Python interpreters that return a broken MSIX package code" by @zanieb on 2025-07-15 20:01_

---

_Comment by @MeGaGiGaGon on 2025-07-15 20:28_

Testing locally this looks to work - using the artifact from the [`build binary | windows x86_64`](https://github.com/astral-sh/uv/actions/runs/16303117032/job/46042516510?pr=14636#logs) action `PS ~> .\uv-windows-x86_64-e7c0217db3c05d6b4d60025341605d3a24a068e3\uv.exe python list` finishes with no errors.

---

_Merged by @zanieb on 2025-07-15 21:47_

---

_Closed by @zanieb on 2025-07-15 21:47_

---

_Branch deleted on 2025-07-15 21:47_

---

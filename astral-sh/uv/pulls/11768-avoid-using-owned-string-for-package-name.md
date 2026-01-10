```yaml
number: 11768
title: "Avoid using owned `String` for package name constructors"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/p-5
created_at: 2025-02-25T06:25:43Z
updated_at: 2025-02-25T07:06:19Z
url: https://github.com/astral-sh/uv/pull/11768
synced_at: 2026-01-10T11:10:38Z
```

# Avoid using owned `String` for package name constructors

---

_Pull request opened by @charliermarsh on 2025-02-25 06:25_

## Summary

Since we use `SmallString` internally, there's no benefit to passing an owned string to the `PackageName` constructor (same goes for `ExtraName`, etc.). I've kept them for now (maybe that will change in the future, so it's useful to have clients passed own values if they _can_), but removed a bunch of usages where we were casting from `&str` to `String` needlessly to use the constructor.


---

_Renamed from "Avoid using owned Strings for package name constructors" to "Avoid using owned `String` for package name constructors" by @charliermarsh on 2025-02-25 06:25_

---

_Label `performance` added by @charliermarsh on 2025-02-25 06:25_

---

_Merged by @charliermarsh on 2025-02-25 07:06_

---

_Closed by @charliermarsh on 2025-02-25 07:06_

---

_Branch deleted on 2025-02-25 07:06_

---

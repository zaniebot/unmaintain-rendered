```yaml
number: 10911
title: make test filter permissive of bdist.win32
type: pull_request
state: merged
author: Gankra
labels:
  - internal
assignees: []
merged: true
base: main
head: gankra/bdist
created_at: 2025-01-23T18:54:45Z
updated_at: 2025-01-23T20:34:05Z
url: https://github.com/astral-sh/uv/pull/10911
synced_at: 2026-01-10T11:45:18Z
```

# make test filter permissive of bdist.win32

---

_Pull request opened by @Gankra on 2025-01-23 18:54_

I swear a few days ago there were way more of this filter but now there's only one so *shrug*.

---

_Label `internal` added by @Gankra on 2025-01-23 18:54_

---

_Comment by @charliermarsh on 2025-01-23 18:55_

Ah yeah I moved those tests away from using setuptools so the filter disappeared.

---

_Comment by @Gankra on 2025-01-23 19:11_

"Aria just fix your computer so it doesn't think it's win32" is also an option but on the other hand having an evil computer has its benefits for finding bugs haha.

---

_@charliermarsh approved on 2025-01-23 19:12_

---

_Merged by @Gankra on 2025-01-23 19:13_

---

_Closed by @Gankra on 2025-01-23 19:13_

---

_Branch deleted on 2025-01-23 19:13_

---

_Comment by @konstin on 2025-01-23 20:34_

> "Aria just fix your computer so it doesn't think it's win32" is also an option but on the other hand having an evil computer has its benefits for finding bugs haha.

Some methods such as [sys.platform](https://docs.python.org/3/library/sys.html#sys.platform) always return `win32` on windows for legacy reasons.

---

```yaml
number: 6949
title: Avoid canonicalizing cache directory
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/t
created_at: 2024-09-02T21:52:05Z
updated_at: 2024-09-03T00:11:45Z
url: https://github.com/astral-sh/uv/pull/6949
synced_at: 2026-01-10T12:53:37Z
```

# Avoid canonicalizing cache directory

---

_Pull request opened by @charliermarsh on 2024-09-02 21:52_

Taking a look at #6948.

---

_Review comment by @burgholzer on `.github/workflows/ci.yml`:100 on 2024-09-02 22:39_

```suggestion
          nuget.exe install python-freethreaded -Version 3.13.0-rc1 -FallbackSource https://api.nuget.org/v3/index.json -OutputDirectory 'C:\Users\runneradmin\AppData\Local\test\Cache\nuget-cpython'
          cargo run venv --python "C:\Users\runneradmin\AppData\Local\test\Cache\nuget-cpython\python-freethreaded.3.13.0-rc1\tools\python.exe"
```
adapted from https://github.com/cda-tum/mqt-qcec/blob/use-mqt-core-package/.github/workflows/cd.yml

---

_@burgholzer reviewed on 2024-09-02 22:39_

---

_Renamed from "Test out Python 3.13t with Git on Windows" to "Avoid canonicalizing cache directory" by @charliermarsh on 2024-09-02 23:57_

---

_Label `bug` added by @charliermarsh on 2024-09-02 23:57_

---

_Label `windows` added by @charliermarsh on 2024-09-02 23:57_

---

_Marked ready for review by @charliermarsh on 2024-09-02 23:57_

---

_Merged by @charliermarsh on 2024-09-03 00:11_

---

_Closed by @charliermarsh on 2024-09-03 00:11_

---

_Branch deleted on 2024-09-03 00:11_

---

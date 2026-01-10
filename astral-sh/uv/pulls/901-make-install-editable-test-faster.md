```yaml
number: 901
title: Make install_editable test faster
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/install_editable-faster
created_at: 2024-01-12T17:15:20Z
updated_at: 2024-01-12T17:50:28Z
url: https://github.com/astral-sh/uv/pull/901
synced_at: 2026-01-10T15:39:02Z
```

# Make install_editable test faster

---

_Pull request opened by @konstin on 2024-01-12 17:15_

Remove a test case from the `install_editable` that slows it down from 3.6s to 6.5s while providing low test coverage. It also seems to block other tests sometimes, `cargo nextest run -E "test(editable)" --all-features` has more consistent and lower runtimes. Surprisingly this seems to have bigger effect than switching from pyo3 to cffi.

Used test commands:
```
rm -rf scripts/editable-installs/maturin_editable/target/ && time cargo nextest run -E "test(=install_editable)" --all-features
rm -rf scripts/editable-installs/maturin_editable/target/ && time cargo nextest run -E "test(editable)" --all-features
 ```

Part of #878

---

_Label `internal` added by @konstin on 2024-01-12 17:15_

---

_@charliermarsh approved on 2024-01-12 17:45_

---

_Merged by @konstin on 2024-01-12 17:50_

---

_Closed by @konstin on 2024-01-12 17:50_

---

_Branch deleted on 2024-01-12 17:50_

---

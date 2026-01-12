```yaml
number: 2340
title: install_extra_index_url_has_priority without exclude newer
type: pull_request
state: merged
author: konstin
labels:
  - testing
assignees: []
merged: true
base: main
head: konsti/install_extra_index_url_has_priority-2
created_at: 2024-03-10T16:05:14Z
updated_at: 2024-03-10T19:03:08Z
url: https://github.com/astral-sh/uv/pull/2340
synced_at: 2026-01-12T16:04:59Z
```

# install_extra_index_url_has_priority without exclude newer

---

_@konstin_

Manual synthesis of #2337 and #2339, i think one accidentally reverted the other?

No rush in merging, CI is green.

---

_Review requested from @BurntSushi by @konstin on 2024-03-10 16:05_

---

_Review requested from @charliermarsh by @konstin on 2024-03-10 16:05_

---

_@charliermarsh reviewed on 2024-03-10 16:11_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_install.rs`:825 on 2024-03-10 16:11_

I think we should include both (`--no-deps` and an `--exclude-newer`).

---

_@charliermarsh approved on 2024-03-10 16:11_

---

_Label `testing` added by @charliermarsh on 2024-03-10 16:12_

---

_Merged by @charliermarsh on 2024-03-10 19:03_

---

_Closed by @charliermarsh on 2024-03-10 19:03_

---

_Branch deleted on 2024-03-10 19:03_

---

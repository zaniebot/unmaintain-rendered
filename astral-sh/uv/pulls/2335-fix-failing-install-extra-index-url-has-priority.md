```yaml
number: 2335
title: Fix failing install_extra_index_url_has_priority test
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/install_extra_index_url_has_priority
created_at: 2024-03-10T13:16:59Z
updated_at: 2024-03-11T16:43:13Z
url: https://github.com/astral-sh/uv/pull/2335
synced_at: 2026-01-12T16:04:58Z
```

# Fix failing install_extra_index_url_has_priority test

---

_@konstin_

`install_extra_index_url_has_priority` started failing because `packaging` had a new release. I'm not sure if this preserves the index order check as intended, but it does unblock CI.

---

_Label `internal` added by @konstin on 2024-03-10 13:16_

---

_Review requested from @BurntSushi by @konstin on 2024-03-10 13:35_

---

_Comment by @konstin on 2024-03-10 13:35_

@BurntSushi I'm going ahead merging this to unblock CI

---

_Merged by @konstin on 2024-03-10 13:35_

---

_Closed by @konstin on 2024-03-10 13:35_

---

_Branch deleted on 2024-03-10 13:35_

---

_Review comment by @BurntSushi on `crates/uv/tests/pip_install.rs`:860 on 2024-03-11 14:47_

I think @charliermarsh fixed this in a follow-up PR, but yeah, this test was very specifically _not_ using `--exclude-newer`. I think we also could have updated the snapshot here instead?

---

_@BurntSushi reviewed on 2024-03-11 14:47_

---

_@konstin reviewed on 2024-03-11 16:42_

---

_Review comment by @konstin on `crates/uv/tests/pip_install.rs`:860 on 2024-03-11 16:42_

I just took the first thing that worked, this was not meant to last. See #2337 and #2340 for the follow-ups

---

_@charliermarsh reviewed on 2024-03-11 16:43_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_install.rs`:860 on 2024-03-11 16:43_

Isn't it ok to use `--exclude-newer` here as long as it's this later date? I added that.

---

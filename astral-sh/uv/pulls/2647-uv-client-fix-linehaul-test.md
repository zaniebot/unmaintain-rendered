```yaml
number: 2647
title: "uv-client: fix linehaul test"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
assignees: []
merged: true
base: main
head: ag/fix-linehaul-test-on-archlinux
created_at: 2024-03-25T13:36:27Z
updated_at: 2024-03-25T14:41:19Z
url: https://github.com/astral-sh/uv/pull/2647
synced_at: 2026-01-12T16:05:09Z
```

# uv-client: fix linehaul test

---

_@BurntSushi_

This test was introduced in 42973cd9cb098e59f75e287d72a425af8a614e91. It
looks like it compares some values against some platform specific code
that attempts to find the OS version. But the comparisons made some
assumptions about what kind of data is available. In this commit, we try
to make the test a little more flexible on Linux by not assuming that
`Option` values are `Some`.


---

_Review requested from @charliermarsh by @BurntSushi on 2024-03-25 13:36_

---

_Review requested from @zanieb by @BurntSushi on 2024-03-25 13:36_

---

_Review request for @zanieb removed by @BurntSushi on 2024-03-25 13:37_

---

_Review requested from @konstin by @BurntSushi on 2024-03-25 13:37_

---

_Comment by @BurntSushi on 2024-03-25 13:37_

cc @samypr100 

---

_@konstin approved on 2024-03-25 14:11_

---

_Merged by @BurntSushi on 2024-03-25 14:12_

---

_Closed by @BurntSushi on 2024-03-25 14:12_

---

_Branch deleted on 2024-03-25 14:12_

---

_Comment by @samypr100 on 2024-03-25 14:21_

Thanks, changes look good. Sorry for the unnecessary panics that could've occured on some cases outside of ubuntu.

---

_Comment by @zanieb on 2024-03-25 14:22_

Related #2564 

---

_Label `internal` added by @BurntSushi on 2024-03-25 14:41_

---

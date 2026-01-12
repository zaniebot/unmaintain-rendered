```yaml
number: 1491
title: "ci: switch to GitHub Actions"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/ci
created_at: 2020-02-18T23:16:06Z
updated_at: 2020-02-20T21:07:58Z
url: https://github.com/BurntSushi/ripgrep/pull/1491
synced_at: 2026-01-12T18:23:13Z
```

# ci: switch to GitHub Actions

---

_@BurntSushi_

_No description provided._

---

_@LPGhatguy reviewed on 2020-02-19 01:39_

---

_Review comment by @LPGhatguy on `tests/util.rs`:179 on 2020-02-19 01:39_

You can use [`std::env::consts::EXE_SUFFIX`](https://doc.rust-lang.org/stable/std/env/consts/constant.EXE_SUFFIX.html) here instead of branching on Windows if you want to save a `cfg`!

---

_Review comment by @BurntSushi on `tests/util.rs`:179 on 2020-02-19 12:12_

Nice catch, thanks! Fixed!

---

_@BurntSushi reviewed on 2020-02-19 12:12_

---

_Closed by @BurntSushi on 2020-02-19 19:39_

---

_Reopened by @BurntSushi on 2020-02-19 19:39_

---

_Merged by @BurntSushi on 2020-02-20 21:07_

---

_Closed by @BurntSushi on 2020-02-20 21:07_

---

_Branch deleted on 2020-02-20 21:07_

---

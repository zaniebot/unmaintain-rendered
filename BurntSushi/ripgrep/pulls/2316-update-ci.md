```yaml
number: 2316
title: Update CI
type: pull_request
state: closed
author: LingMan
labels: []
assignees: []
base: master
head: ci
created_at: 2022-09-29T03:42:29Z
updated_at: 2022-09-29T12:05:25Z
url: https://github.com/BurntSushi/ripgrep/pull/2316
synced_at: 2026-01-12T18:23:14Z
```

# Update CI

---

_@LingMan_

The major change here is to switch away from the `ubuntu-18.04` image. It is deprecated and will be removed by 2023-04-01 with scheduled brownouts starting on 2022-10-03.

While making changes in the area, I've also switched `dtolnay/rust-toolchain` to use the master branch and added two minor cleanups.

All of these have already been applied over in the bstr repository.

---

_@BurntSushi approved on 2022-09-29 11:22_

Lovely, thank you!

---

_Closed by @BurntSushi on 2022-09-29 12:03_

---

_Branch deleted on 2022-09-29 12:05_

---

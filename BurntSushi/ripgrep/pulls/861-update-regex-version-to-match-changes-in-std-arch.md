```yaml
number: 861
title: "Update `regex` version to match changes in std::arch"
type: pull_request
state: merged
author: andyfangdz
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2018-03-17T23:22:04Z
updated_at: 2018-03-17T23:33:35Z
url: https://github.com/BurntSushi/ripgrep/pull/861
synced_at: 2026-01-12T18:23:13Z
```

# Update `regex` version to match changes in std::arch

---

_@andyfangdz_

Currently, builds with `simd` and `avx` features are failing due to mismatches in latest `std::arch`. This PR updates the version of `regex` to sync with latest marco in `std::arch`, `is_x86_feature_detected!`. https://github.com/rust-lang/regex/commit/c84bc41e5aceb87b6a5fa3d4b75ca85c1b1fd6f6


---

_@BurntSushi approved on 2018-03-17 23:33_

Thanks!

---

_Merged by @BurntSushi on 2018-03-17 23:33_

---

_Closed by @BurntSushi on 2018-03-17 23:33_

---

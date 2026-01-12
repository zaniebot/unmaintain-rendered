```yaml
number: 2569
title: impl Deserialize for GlobSet
type: pull_request
state: merged
author: dtolnay
labels: []
assignees: []
merged: true
base: master
head: deglobset
created_at: 2023-07-26T23:15:37Z
updated_at: 2023-07-26T23:52:08Z
url: https://github.com/BurntSushi/ripgrep/pull/2569
synced_at: 2026-01-12T18:23:14Z
```

# impl Deserialize for GlobSet

---

_@dtolnay_

I am interested in putting GlobSet as a field of a configuration file in https://github.com/facebookincubator/reindeer.

```rust
#[derive(Deserialize)]
pub struct Fixup {
    pub extra_srcs: GlobSet,
}
```

---

_@BurntSushi approved on 2023-07-26 23:51_

Nice, thanks!

---

_Merged by @BurntSushi on 2023-07-26 23:51_

---

_Closed by @BurntSushi on 2023-07-26 23:51_

---

_Comment by @BurntSushi on 2023-07-26 23:51_

This is on crates.io in `globset 0.4.12`.

---

_Branch deleted on 2023-07-26 23:52_

---

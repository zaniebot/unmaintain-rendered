```yaml
number: 724
title: dependency updates
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/misc-fixes
created_at: 2017-12-30T21:02:17Z
updated_at: 2017-12-30T21:50:23Z
url: https://github.com/BurntSushi/ripgrep/pull/724
synced_at: 2026-01-12T18:23:13Z
```

# dependency updates

---

_@BurntSushi_

This PR updates all dependencies to their latest versions, including clap, which bumps the minimum Rust version required by ripgrep to Rust 1.20.0. We've also removed the dependency on winapi 0.2 in every case but one (the `memmap` crate). Once `memmap` is updated to use winapi 0.3, ripgrep should no longer need 0.2.

This also rolls in #699 and #722.

---

_Merged by @BurntSushi on 2017-12-30 21:50_

---

_Closed by @BurntSushi on 2017-12-30 21:50_

---

_Branch deleted on 2017-12-30 21:50_

---

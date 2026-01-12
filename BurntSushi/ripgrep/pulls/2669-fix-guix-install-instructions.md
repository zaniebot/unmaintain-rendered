```yaml
number: 2669
title: Fix Guix install instructions
type: pull_request
state: merged
author: TimoWilken
labels: []
assignees: []
merged: true
base: master
head: patch-1
created_at: 2023-11-30T15:49:29Z
updated_at: 2023-11-30T15:55:49Z
url: https://github.com/BurntSushi/ripgrep/pull/2669
synced_at: 2026-01-12T18:23:14Z
```

# Fix Guix install instructions

---

_@TimoWilken_

`guix install` should not be run using `sudo`, as per <https://packages.guix.gnu.org/packages/ripgrep/>.

Guix is more like Nix in that users install packages without root privileges.

---

_@BurntSushi approved on 2023-11-30 15:54_

Thanks!

---

_Merged by @BurntSushi on 2023-11-30 15:54_

---

_Closed by @BurntSushi on 2023-11-30 15:54_

---

_Branch deleted on 2023-11-30 15:55_

---

_Comment by @TimoWilken on 2023-11-30 15:55_

Wow that was quick, thank you @BurntSushi!

---

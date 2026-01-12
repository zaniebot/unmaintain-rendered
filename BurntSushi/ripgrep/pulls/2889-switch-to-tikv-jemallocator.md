```yaml
number: 2889
title: switch to tikv-jemallocator
type: pull_request
state: closed
author: krant
labels:
  - rollup
assignees: []
base: master
head: master
created_at: 2024-09-12T14:09:24Z
updated_at: 2025-09-20T01:08:30Z
url: https://github.com/BurntSushi/ripgrep/pull/2889
synced_at: 2026-01-12T18:23:14Z
```

# switch to tikv-jemallocator

---

_@krant_

It is now a recommended crate for jemalloc and it contains an [important fix for compilation on riscv64gc-unknown-linux-musl](https://github.com/tikv/jemallocator/pull/67), I bumped into this when I was trying to [build ripgrep on OpenWrt](https://github.com/openwrt/packages/pull/24961).

---

_Comment by @BurntSushi on 2025-07-26 15:41_

OK thanks. I did a brief review of `tikv-jemallocator` and it LGTM.

---

_Label `rollup` added by @BurntSushi on 2025-07-26 15:42_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---

```yaml
number: 1887
title: "edition: migrate to Rust 2018"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/edition-2018
created_at: 2021-06-02T00:50:13Z
updated_at: 2021-06-02T16:05:11Z
url: https://github.com/BurntSushi/ripgrep/pull/1887
synced_at: 2026-01-12T18:23:14Z
```

# edition: migrate to Rust 2018

---

_@BurntSushi_

This was long overdue. Essentially, we apply `cargo fix --edition`, then add `edition = "2018"` to every `Cargo.toml`, then run `cargo fix --edition --edition-idioms` and then finally do manual touch-ups (mostly to remove uses of `extern crate`).

---

_Merged by @BurntSushi on 2021-06-02 01:07_

---

_Closed by @BurntSushi on 2021-06-02 01:07_

---

_Branch deleted on 2021-06-02 01:07_

---

_Comment by @iago-lito on 2021-06-02 16:02_

Great ! Now soon 2021 ^ ^"

---

_Comment by @BurntSushi on 2021-06-02 16:05_

Yeah, hopefully more quickly than Rust 2018. I didn't wait this long intentionally or anything, just wasn't particularly motivated to do it. It's likely that the move to Rust 2021 will be much less eventful though.

---

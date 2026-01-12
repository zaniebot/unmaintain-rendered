```yaml
number: 3152
title: "cargo: set MSRV to 1.85"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/msrv-debian
created_at: 2025-09-21T13:19:14Z
updated_at: 2025-09-21T13:51:47Z
url: https://github.com/BurntSushi/ripgrep/pull/3152
synced_at: 2026-01-12T18:23:15Z
```

# cargo: set MSRV to 1.85

---

_@BurntSushi_

I believe the current stable version of Debian packages 1.85 rustc. So
if the next release of ripgrep uses a higher MSRV, then I think Debian
won't be able to package it.

It also turned out that I wasn't using anything from beyond Rust 1.85
anyway.

It's likely that I could make use of let-chains in various places, but I
don't think it's worth combing through the code to switch to them at
this point.


---

_Comment by @ltrzesniewski on 2025-09-21 13:42_

Quick question: why not add a `rust-toolchain.toml` file in order to avoid accidentally using the new features?

Is it to benefit from the newer compiler perf improvements for instance?

---

_Comment by @BurntSushi on 2025-09-21 13:51_

I always develop with ~latest nightly. I would never restrain my own local development environment just because Debian wants to lag behind by multiple years.

---

_Merged by @BurntSushi on 2025-09-21 13:51_

---

_Closed by @BurntSushi on 2025-09-21 13:51_

---

_Branch deleted on 2025-09-21 13:51_

---

_Comment by @BurntSushi on 2025-09-21 13:51_

> Is it to benefit from the newer compiler perf improvements for instance?

But yes, this is one reason why I always use the latest nightly.

---

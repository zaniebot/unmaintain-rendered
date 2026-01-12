```yaml
number: 1427
title: update crossbeam-channel to 0.4.0
type: pull_request
state: closed
author: ctaggart
labels: []
assignees: []
base: master
head: crosssbeam-channel
created_at: 2019-11-12T12:16:45Z
updated_at: 2020-01-10T20:07:49Z
url: https://github.com/BurntSushi/ripgrep/pull/1427
synced_at: 2026-01-12T18:23:13Z
```

# update crossbeam-channel to 0.4.0

---

_@ctaggart_

fix #1423

---

_Comment by @ctaggart on 2019-11-12 12:59_

I can use this branch to workaround the issue in my Cargo.toml like so:

``` toml
[replace]
"ignore:0.4.10" = { git = "https://github.com/ctaggart/ripgrep", package = "ignore", branch = "crosssbeam-channel" }
```

https://doc.rust-lang.org/cargo/reference/manifest.html#the-replace-section

---

_Closed by @BurntSushi on 2020-01-10 20:07_

---

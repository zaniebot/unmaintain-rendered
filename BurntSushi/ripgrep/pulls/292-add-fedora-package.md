```yaml
number: 292
title: Add Fedora package
type: pull_request
state: closed
author: ghost
labels: []
assignees: []
base: master
head: master
created_at: 2016-12-25T10:54:00Z
updated_at: 2016-12-27T12:37:32Z
url: https://github.com/BurntSushi/ripgrep/pull/292
synced_at: 2026-01-12T18:23:12Z
```

# Add Fedora package

---

_@ghost_

_No description provided._

---

_Comment by @BurntSushi on 2016-12-27 12:37_

I'm sorry, but I have no hope of maintaining this, and I therefore can't accept it. In the future, it would be better to discuss these changes in a separate issue to avoid doing this work.

---

OK, time to elaborate a bit. The reason why the `pkg` directory exists was because I was initially trying to get ripgrep into distributions for people to use. I personally use Archlinux, so I knew how to do that. I also wanted to get this into the hands of Mac users as easily as possible, so I set up a brew package for it. Since that point, others have taken over the maintenance burden of those packages. (ripgrep is now part of Archlinux and homebrew proper.) There is a homebrew package still in this repo because it permits homebrew users to install a version of ripgrep compiled from Rust nightly, which is faster than the one compiled on Rust stable because of SIMD support. Once SIMD becomes available on Rust stable, it is my intention to remove that homebrew package.

---

_Closed by @BurntSushi on 2016-12-27 12:37_

---

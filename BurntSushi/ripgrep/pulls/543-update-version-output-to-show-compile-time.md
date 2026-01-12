```yaml
number: 543
title: "Update `--version` output to show compile-time features"
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: feature/features
created_at: 2017-07-07T06:31:42Z
updated_at: 2017-07-07T15:31:05Z
url: https://github.com/BurntSushi/ripgrep/pull/543
synced_at: 2026-01-12T18:23:13Z
```

# Update `--version` output to show compile-time features

---

_@okdana_

#524 sounded fun and easy. I only just barely know what i'm doing in Rust though — is this a reasonable way to go about this? I don't think the method will scale elegantly beyond three or four features, but i figured something fancier could quickly be devised if it came to that in the future.

This is the sort of output you get (i don't have Rust nightly so i added a dummy option to test; it's not present in the actual commit of course):

```
heartswap:ripgrep % cargo build -q --release --features 'foo'
heartswap:ripgrep % target/release/rg --version
ripgrep 0.5.2, with features (+/-): +foo -avx-accel -simd-accel
```

---

_Comment by @BurntSushi on 2017-07-07 10:51_

@okdana Yup, this is great! This is probably roughly how I'd do it too. I'm not a huge fan of adding new features (and I actually hope to remove these once Rust's SIMD story improves such that we can enable them dynamically without relying on compilation support), so it should be easy to maintain this list! Thank you. :-)

One nit: could you put `Fixes #524` into the commit message? That way, Github will sync the commit with the issue and close it automatically. :-)

---

_Comment by @okdana on 2017-07-07 14:36_

Oops, changed it now.

---

_Merged by @BurntSushi on 2017-07-07 15:31_

---

_Closed by @BurntSushi on 2017-07-07 15:31_

---

_Comment by @BurntSushi on 2017-07-07 15:31_

Thanks!

---

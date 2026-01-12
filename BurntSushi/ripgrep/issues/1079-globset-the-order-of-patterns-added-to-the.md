```yaml
number: 1079
title: "GlobSet: the order of patterns added to the builder matters and it shouldn't"
type: issue
state: closed
author: calixteman
labels:
  - question
assignees: []
created_at: 2018-10-07T17:43:41Z
updated_at: 2019-03-29T09:49:35Z
url: https://github.com/BurntSushi/ripgrep/issues/1079
synced_at: 2026-01-12T16:13:22Z
```

# GlobSet: the order of patterns added to the builder matters and it shouldn't

---

_@calixteman_

With globset 0.4.2, the following test case is failing:
```rust
extern crate globset;
use globset::{Glob, GlobSetBuilder};

fn main() {
    let mut glob_builder = GlobSetBuilder::new();	
    for i in vec!["libcore/*", "libstd/*"] {
        glob_builder.add(Glob::new(&i).unwrap());
    }
    let globset = glob_builder.build().unwrap();
	
    assert!(globset.is_match("libstd/io/buffered.rs"));
    assert!(globset.is_match("libcore/char/methods.rs"));

    let mut glob_builder = GlobSetBuilder::new();	
    for i in vec!["libstd/*", "libcore/*"] {
        glob_builder.add(Glob::new(&i).unwrap());
    }
    let globset = glob_builder.build().unwrap();
	
    assert!(globset.is_match("libstd/io/buffered.rs"));
    assert!(globset.is_match("libcore/char/methods.rs"));
}
```
with (this is the last assertion in the code):
'assertion failed: globset.is_match("libcore/char/methods.rs")', src/main.rs:22:5



---

_Renamed from "GlobSet: the order of patterns added to the builder matters" to "GlobSet: the order of patterns added to the builder matters and it shouldn't" by @calixteman on 2018-10-07 19:32_

---

_Comment by @calixteman on 2018-10-19 13:55_

@BurntSushi, any chance to have a fix for this bug ?

---

_Comment by @BurntSushi on 2018-10-19 14:07_

@calixteman Thanks for the ping, but I haven't even had time to confirm this and I don't know when I will be able to look at fixing it.

---

_Label `question` added by @BurntSushi on 2018-10-19 14:07_

---

_Closed by @BurntSushi on 2019-02-16 16:45_

---

_Comment by @marco-c on 2019-03-29 09:49_

@BurntSushi could you release a new version of globset with the fix for this?

---

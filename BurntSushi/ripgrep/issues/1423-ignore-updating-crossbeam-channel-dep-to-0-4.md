```yaml
number: 1423
title: "ignore: updating crossbeam-channel dep to 0.4"
type: issue
state: closed
author: calebcartwright
labels: []
assignees: []
created_at: 2019-11-08T17:38:21Z
updated_at: 2020-01-10T20:09:24Z
url: https://github.com/BurntSushi/ripgrep/issues/1423
synced_at: 2026-01-12T16:13:23Z
```

# ignore: updating crossbeam-channel dep to 0.4

---

_@calebcartwright_

Just wanted to see if there were any plans to update the crossbeam-channel dependency to v0.4.0 in `ignore`, and if so, if there's a ballpark timeline?

There was an issue with crossbeam-utils v0.6.6 (which crossbeam-channel depends on) that has subsequently been fixed on the crossbeam side and recently released (v0.7.0)
https://github.com/rust-lang/rust/issues/65424
https://github.com/crossbeam-rs/crossbeam/issues/435

Ignore is pinned to crossbeam-channel v0.3.6, and that version of crossbeam-channel has a ^0.6.x dependency on crossbeam-utils, preventing the ability to use the fixed version (v0.7.0) of crossbeam-utils

https://github.com/BurntSushi/ripgrep/blob/8892bf648cfec111e6e7ddd9f30e932b0371db68/ignore/Cargo.toml#L21

Folks have been able to work around that issue (for example by pinning utils to v0.6.5 in the lockfile) so it's not a terribly urgent item, but it would be nice to be able to avoid this.


---

_Comment by @ctaggart on 2019-11-12 12:08_

I'm actually not sure how to workaround this. The error I get is:
```
error[E0432]: unresolved import `crossbeam_utils::atomic`
 --> /Users/cameron/.cargo/registry/src/github.com-1ecc6299db9ec823/crossbeam-channel-0.3.9/src/flavors/tick.rs:8:22
  |
8 | use crossbeam_utils::atomic::AtomicCell;
  |                      ^^^^^^ could not find `atomic` in `crossbeam_utils`
```

Using `cargo tree`, I can see the dependency is brought in from this project.
```
│   ├── ignore v0.4.10
│   │   ├── crossbeam-channel v0.3.9
│   │   │   └── crossbeam-utils v0.6.6
│   │   │       ├── cfg-if v0.1.10 (*)
│   │   │       └── lazy_static v1.4.0 (*)
```

---

_Comment by @BurntSushi on 2020-01-10 20:09_

Fixed in #1457

---

_Closed by @BurntSushi on 2020-01-10 20:09_

---

```yaml
number: 1342
title: "[Tracking issue] Transitioning crates to Rust 2018"
type: issue
state: closed
author: tesuji
labels: []
assignees: []
created_at: 2019-08-06T16:06:27Z
updated_at: 2019-08-07T14:23:36Z
url: https://github.com/BurntSushi/ripgrep/issues/1342
synced_at: 2026-01-12T16:13:23Z
```

# [Tracking issue] Transitioning crates to Rust 2018

---

_@tesuji_

This issue tracks the transitioning of all crates in this repo to Rust 2018.

---

You can help by filing PRs transitioning a crate at a time to Rust 2018.
When transitioning, please apply the following to the crate root:
```rust
#![deny(rust_2018_idioms)]
```

You can use `cargo +nightly --edition-idioms` to migrate a crate automatically.

---

The following crates exist in the repo.
Checked items have been transitioned to Rust 2018.

- [x] ripgrep
- [ ] globset: In-progress #1341
- [ ] grep-cli
- [ ] grep-matcher
- [ ] grep-pcre2
- [ ] grep-printer
- [ ] grep-regex
- [ ] grep-searcher
- [ ] grep
- [ ] ignore: In-progress #1340


---

_Comment by @BurntSushi on 2019-08-06 16:21_

I'd prefer to hold off on this and do it myself honestly. I don't have the bandwidth to review a bunch of different PRs to every crate in this repo.

I'm also in no rush to do this, as I don't think there is any urgency in moving to Rust 2018.

I'm not sure I agree with `deny(rust_2018_idioms)`. I'll need to look carefully at the changes it implies.

---

_Comment by @tesuji on 2019-08-06 16:52_

No problem. It's your repo after all.
Feels free to close this issue if you like.


---

_Comment by @BurntSushi on 2019-08-07 14:23_

Thanks. I'll close this for now, but I'll try to get it done for the next major version release.

---

_Closed by @BurntSushi on 2019-08-07 14:23_

---

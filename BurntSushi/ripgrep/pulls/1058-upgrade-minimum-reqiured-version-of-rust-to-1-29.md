```yaml
number: 1058
title: Upgrade minimum reqiured version of Rust to 1.29
type: pull_request
state: closed
author: sharkdp
labels: []
assignees: []
base: master
head: fix-916
created_at: 2018-09-17T19:50:21Z
updated_at: 2018-09-17T20:34:34Z
url: https://github.com/BurntSushi/ripgrep/pull/1058
synced_at: 2026-01-12T18:23:13Z
```

# Upgrade minimum reqiured version of Rust to 1.29

---

_@sharkdp_

This upgrades the minimum required version of Rust to 1.29 in order to fix #916 (Zombie processes cause `rg --files` to hang in `/proc`).

I have manually confirmed that the bug is fixed when compiling ripgrep with 1.29.

It's probably better to merge this just before the next release in order for the current Rust version to still be in present in the README(?).

See also:
- Rust compiler bug ticket: https://github.com/rust-lang/rust/issues/50619
- Rust compiler PR with the fix: https://github.com/rust-lang/rust/pull/50630

closes #916

---

_Comment by @BurntSushi on 2018-09-17 20:31_

> It's probably better to merge this just before the next release in order for the current Rust version to still be in present in the README(?).

Ah right yeah. Basically, to keep things sensible, I'd like to make sure any `0.x.y` releases build on the same version as the `0.x.0` release. So I won't bump the minimum Rust version until I know the next release is `0.11`.

I will just do that as part of the normal release process, so I'm going to close this. In theory, it could be months before a `0.11` release. :) Thank you for the reminder though and staying on top of this!

---

_Closed by @BurntSushi on 2018-09-17 20:31_

---

_Comment by @sharkdp on 2018-09-17 20:34_

> I'd like to make sure any `0.x.y` releases build on the same version as the `0.x.0` release

Ah, that makes sense.

> In theory, it could be months before a `0.11` release.

ripgrep 0.10 was six days early :smile:

---

_Branch deleted on 2018-09-17 20:34_

---

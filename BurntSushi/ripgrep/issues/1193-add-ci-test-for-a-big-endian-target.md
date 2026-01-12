```yaml
number: 1193
title: add CI test for a big-endian target
type: issue
state: closed
author: BurntSushi
labels:
  - enhancement
assignees: []
created_at: 2019-02-09T21:16:00Z
updated_at: 2020-03-15T15:38:49Z
url: https://github.com/BurntSushi/ripgrep/issues/1193
synced_at: 2026-01-12T16:13:23Z
```

# add CI test for a big-endian target

---

_@BurntSushi_

Given #1144, it seems prudent for ripgrep to get tested on a big-endian target.

I tried playing with this in #1152 and ran into errors. The `memchr` crate is an example with a big-endian target test that works: https://github.com/BurntSushi/rust-memchr/blob/974943c8bc840538e18d788b6682bbfcdccceb12/.travis.yml#L31-L35

---

_Label `enhancement` added by @BurntSushi on 2019-02-09 21:16_

---

_Comment by @hlieberman-gov on 2019-02-25 19:40_

Though it's not really useful for "live" testing, we do get post-facto testing from Debian's build infra which has several big-endian targets that will at least tell us if we broke something horribly.

i.e., https://buildd.debian.org/status/package.php?p=rust-ripgrep.

---

_Comment by @BurntSushi on 2019-02-25 20:33_

@hlieberman-gov Right, that's how #1144 was reported, AIUI. The point here is to catch problems before they get to the Debian folks. :-)

---

_Comment by @BurntSushi on 2020-03-15 15:38_

This was done in da3431b4780a2654cdbb35613ff428cdef3fc8fe, which adds a `nightly-mips` builder which should cover big endian.

---

_Closed by @BurntSushi on 2020-03-15 15:38_

---

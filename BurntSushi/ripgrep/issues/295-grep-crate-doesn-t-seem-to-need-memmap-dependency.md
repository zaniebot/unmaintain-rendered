```yaml
number: 295
title: "Grep crate doesn't seem to need memmap dependency"
type: issue
state: closed
author: Dr-Emann
labels: []
assignees: []
created_at: 2016-12-27T20:09:25Z
updated_at: 2016-12-27T20:49:16Z
url: https://github.com/BurntSushi/ripgrep/issues/295
synced_at: 2026-01-12T16:13:21Z
```

# Grep crate doesn't seem to need memmap dependency

---

_@Dr-Emann_

I don't see [anywhere](https://github.com/BurntSushi/ripgrep/blob/de5cb7d22e00c18dfce082a68b0cf45924b56e7f/grep/src/lib.rs#L8) in the grep crate which requires the memmap crate, which is specified in the [Cargo.toml](https://github.com/BurntSushi/ripgrep/blob/de5cb7d22e00c18dfce082a68b0cf45924b56e7f/grep/Cargo.toml#L18). I assume this was probably an artifact of the grep crate once being part of ripgrep, which requires memmaps.

---

_Closed by @BurntSushi on 2016-12-27 20:46_

---

_Comment by @BurntSushi on 2016-12-27 20:49_

@Dr-Emann Thanks for noticing that!

You're indeed correct. In the long ago, ripgrep (formerly called `rep`) was my playground for testing the regex library. I initially tried to move all search code into a separate `grep` crate, but couldn't quite figure out how to do it. So I ended up splitting a good chunk of it back out into ripgrep proper, and probably just forgot to get rid of the `memmap` dependency.

I am planning on moving the search code back into the `grep` crate, at which point, the `memmap` dependency may return. (What I know now that I was missing before is that the search code needs to be abstract over a sink/printer interface.)

---

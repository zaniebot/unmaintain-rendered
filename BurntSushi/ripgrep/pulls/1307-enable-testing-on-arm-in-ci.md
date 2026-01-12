```yaml
number: 1307
title: Enable testing on ARM, in CI
type: pull_request
state: closed
author: lilianmoraru
labels: []
assignees: []
base: master
head: master
created_at: 2019-06-19T17:58:19Z
updated_at: 2019-08-01T21:05:07Z
url: https://github.com/BurntSushi/ripgrep/pull/1307
synced_at: 2026-01-12T18:23:13Z
```

# Enable testing on ARM, in CI

---

_@lilianmoraru_

A few issues here.
It seems to fail a test in CI because the QEMU version is old and it does not support ARM very well.

There are at least a few ways to fix this(that I can think of):
- There is no QEMU ppa - so, either it is created by someone(I personally don't know how) or have a precompiled QEMU archive be downloaded.
- Switch the ARM build to another CI, like Azure Pipelines and have it there run on a newer version of Linux?
- Disable some tests on ARM(cfg over some tests)?

---

_Comment by @lilianmoraru on 2019-06-23 22:29_

[`cross`](https://github.com/rust-embedded/cross) also supports running tests in QEMU through a newer Dockerfile(QEMU from Ubuntu 18.04).
`ripgrep` should probably switch to that in order to support a lot more targets.

Edit:
Inspiration: https://github.com/nix-rust/nix/blob/master/.travis.yml

---

_Comment by @BurntSushi on 2019-06-24 11:03_

Thanks! I don't know when (or if) I'll get a chance to dig into this. Honestly, it is _not_ a goal of mine to support lots of targets. Every target added means a higher chance of something going wrong in the release process that I have to debug, and the release process is already laborious enough. If people really want ARM binaries, then they'll need to setup their own infrastructure for building them.

---

_Comment by @BurntSushi on 2019-08-01 21:05_

I'm unfortunately going to close this. I just don't have the bandwidth to maintain releases for more targets. Sorry. If you wanted to maintain this on your own in a separate repo, and it was kept up to date, I would be willing to link it from the README.

---

_Closed by @BurntSushi on 2019-08-01 21:05_

---

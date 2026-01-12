```yaml
number: 2601
title: "ci: replace mips with powerpc64, aarch64 and s390x"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/fix-ci
created_at: 2023-08-29T02:14:11Z
updated_at: 2023-08-29T02:48:24Z
url: https://github.com/BurntSushi/ripgrep/pull/2601
synced_at: 2026-01-12T18:23:14Z
```

# ci: replace mips with powerpc64, aarch64 and s390x

---

_@BurntSushi_

We drop our MIPS target because it no longer works.[1] We were previously using it as a means of testing ripgrep in a big endian environment. So to achieve that without MIPS, we test on powerpc64 and s390x. (No particular reason to do both, but why not.)

We also add aarch64 as a proxy for at least ensuring everything works for the same architecture as Apple silicon. It's not a guarantee that everything works, but it seems better than nothing until we can actually test Apple silicon in CI.

[1]: https://github.com/rust-lang/regex/commit/c788378d6fe407f4774df98a78436cea5d98525b

---

_Comment by @BurntSushi on 2023-08-29 02:43_

This PR is a good example of why I generally dislike using tooling in CI that I don't control. Something about how [Cross](https://github.com/cross-rs/cross) works changed and has cost me several hours of debugging this evening. I hit at least a few problems that are difficult for me to describe because I don't really understand them (and not all of which are necessarily Cross's fault), but the big one is that it looks like the `CROSS_RUNNER` environment variable is now _almost_ useless. (It's still useful for detecting whether we're running under `cross`.) It, I believe, used to point to the `qemu` executable that one can use to run the cross compiled `rg` executable with inside of the Docker container. But now it just points to `qemu-user` in all of the images (except for native `x86_64`) and there is no `qemu-user` binary in the Docker image. So I had to do some detective work to figure out how exactly to invoke `qemu` so that I could run the `rg` executable for integration tests.

If Cross didn't do so damn much and save so much work when it came to cross-compilation I'd drop it in a heart beat. It easily consumes the vast majority of the time I spend dealing with CI problems.

Don't let anyone convince you that cross compilation is easy. It's *easier* because of Cross (a lot easier), but it isn't without its problems.

---

_Merged by @BurntSushi on 2023-08-29 02:45_

---

_Closed by @BurntSushi on 2023-08-29 02:45_

---

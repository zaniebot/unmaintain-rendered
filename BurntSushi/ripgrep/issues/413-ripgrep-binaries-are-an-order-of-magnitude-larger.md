```yaml
number: 413
title: ripgrep binaries are an order of magnitude larger than they need to be
type: issue
state: closed
author: BurntSushi
labels:
  - question
assignees: []
created_at: 2017-03-19T14:48:33Z
updated_at: 2017-08-23T22:07:58Z
url: https://github.com/BurntSushi/ripgrep/issues/413
synced_at: 2026-01-12T16:13:21Z
```

# ripgrep binaries are an order of magnitude larger than they need to be

---

_@BurntSushi_

This discussion started in #412.

The problem is that when you compile ripgrep today, you get big binaries. Both the `cargo install` approach and installing Linux binaries from the Github releases yield large binaries.

To address the inconsistency concern in the abstract: there are *many* inconsistencies between the ripgrep releases and what's installed with `cargo install`, and these inconsistencies will always be there. For example, `cargo install` doesn't provide easy access to a man page, license info or to the various auto-completion scripts. So the fact that there is yet another inconsistency doesn't really bother me. Namely, `cargo install` is *not* the recommended way to install ripgrep, so I'm less sympathetic to issues that people have using it. It's provided on a best effort basis because it's occasionally convenient for folks who are otherwise Rust programmers.

Secondly, I will describe why I have `debug = true` for release builds. Perhaps there is some other solution that I haven't thought of.

There are two primary benefits I perceived when setting `debug = true`:

1. A large fraction of time I spend developing ripgrep is with it compiled in release mode because I want to inspect its runtime performance. Release mode by default doesn't include debug symbols, so this makes profiling harder. If `debug = false` were committed to source control, then I would have to constantly be changing around that value in the `Cargo.toml` during development, which is a huge pain. I don't want to do it.
2. If and when ripgrep panics and the bug is hard to reproduce, the reporter can set `RUST_BACKTRACE=1` on some systems and get a useful stack trace if and only if debug symbols are in the binary.

(2) is interesting, because ripgrep hardly ever panics, so it actually has not turned out to be as useful as I would have thought. In the case where it does happen, we could ask the user to compile ripgrep on their own---which is probably what most projects do---but I had wanted to make it as effortless as possible to report a bug. Nevertheless, since it rarely happens, it seems weird to optimize for that case.

With all that said, it seems like this is an argument in favor of stripping debug info from the binaries released in Github, but to retain the `debug = true` in `Cargo.toml`. Is there another solution? Can Cargo's config do something to help me out here?

---

_Label `question` added by @BurntSushi on 2017-03-19 14:48_

---

_Comment by @sfackler on 2017-03-19 17:18_

As of the 1.16 release, you can set `debug = 1` or `debug = 2` to select the level of debuginfo you want in addition to `debug = true` (which acts like `debug = 2`). I believe level 1 debuginfo includes everything necessary for backtraces and profiling? Not sure what the size difference is though.

---

_Comment by @XAMPPRocky on 2017-03-19 18:32_

I definitely feel the same with having to worry about making sure to not to publish with `debug = true`, one possible solution I was thinking was if there was another cargo profile. A way to differentiate between a `cargo install` and `cargo build --release`. With the install profile restricted in what it can override, maybe it can only toggle debug.

---

_Comment by @pftbest on 2017-03-28 06:55_

Please also note that when debuginfo is enabled frame pointers are not eliminated.
https://github.com/rust-lang/rust/issues/11906
This may (or may not) have some impact on a performance.

---

_Comment by @DoumanAsh on 2017-04-02 12:08_

You can also consider to add `lto = true` for published builds.
As far as i know it is not a default for release builds.
But it requires testing.
It didn't give much difference on msvc toolchain for me

On other hand you can have debug symbols separately (cargo build even produces `pdb` for binaries), but i'm not sure if it possible to tell cargo to produce them separately instead of bundling together?
Since `cargo build` is capable (need confirmation on linux) it should be possible?
Most profiling tools accept loading of symbols in the same directory as your binary.

---

_Comment by @kbknapp on 2017-04-04 15:34_

Using `lto = true` was discussed in BurntSushi/ripgrep#325 with the problem being compile times suffer for little gain (performance). However, quoting @BurntSushi 

> If someone wanted this then I'd be open to adding it only in CI when the final release artifact is produced.

---

_Comment by @retep998 on 2017-04-18 03:09_

Enabling debuginfo for msvc during `cargo install` provides no benefit because the .pdb is thrown away so you couldn't get a sane backtrace anyway. If you manually distribute compiled binaries with the .pdb then the debuginfo becomes valuable.

>It didn't give much difference on msvc toolchain for me

This is because what you're seeing is LTO stripping unreferenced functions which ld is actually *really* bad at while link.exe is amazing at it. As a result LTO is just bringing the size of gnu binaries *closer* to where msvc binaries already are. LTO is still useful to maximize inlining potential though.

---

_Comment by @scottchiefbaker on 2017-04-18 23:53_

I didn't know debug info was enabled and while I was doing a casual comparison of `ag` vs `rg` and I noticed the file sizes were **drastically** different.

`ag` is 244KB
`rg` is 25**MB**

I think it send the wrong message if you're 100x larger than the competition. The majority of people using ripgrep will never use the debug information so it's wasteful in both bandwidth and disk space.

---

_Comment by @BurntSushi on 2017-04-19 00:40_

Why does it matter? If you care about a few megs of disk space, then I'm willing to bet that's a pretty specialized use case. In that case, just strip the binary yourself.

I don't know what "wrong message" you are referring to. Could you please be more specific?

---

_Comment by @retep998 on 2017-04-19 03:28_

Wow, the difference between msvc and gnu is pretty huge. For the latest release x86_64-pc-windows-gnu is 24,228,086 bytes while x86_64-pc-windows-msvc is 2,445,312 bytes, almost exactly an order of magnitude difference.

---

_Comment by @kaushalmodi on 2017-05-09 21:28_

[Setting `debug` to `false`][1] reduced built `rg` binary size on RHEL 6.6 from 25 MB to 5.8 MB.

[1]: https://gist.github.com/17f03efe2e57265d90c48d39f5dabc27

---

_Comment by @luser on 2017-05-22 15:32_

One thing that might be feasible in the future is using ELF compressed sections. For example:
```
$ ls -l /tmp/rg
-rwxr-xr-x 1 luser docker 21233872 May 22 11:23 /tmp/rg
$ objcopy --compress-debug-sections /tmp/rg
$ ls -l /tmp/rg
-rwxr-xr-x 1 luser docker 6196568 May 22 11:24 /tmp/rg
```
Unfortunately libbacktrace doesn't support reading them yet, so this breaks their usefulness in Rust backtraces. If that got fixed it'd be a nice easy win (DWARF is pretty verbose).

---

_Closed by @BurntSushi on 2017-08-23 22:07_

---

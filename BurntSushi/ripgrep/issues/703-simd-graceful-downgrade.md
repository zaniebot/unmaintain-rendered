```yaml
number: 703
title: SIMD Graceful Downgrade
type: issue
state: closed
author: hlieberman
labels:
  - duplicate
  - question
assignees: []
created_at: 2017-12-02T19:55:40Z
updated_at: 2018-03-13T03:22:10Z
url: https://github.com/BurntSushi/ripgrep/issues/703
synced_at: 2026-01-12T16:13:22Z
```

# SIMD Graceful Downgrade

---

_@hlieberman_

Hello @BurntSushi!

I'm interested in getting ripgrep added to Debian (where it will trickle through to Ubuntu)!  One of the things I've been watching for is support for SIMD landing in Rust's stable branch, as well as an ability to have a single binary that supports graceful downgrades to the minimal version of SIMD that they support.  (AVX -> SSE -> none.)

I know most of that is blocked on upstream Rust, or worse, upstream LLVM.  I could build a version that simply doesn't support SIMD at all, but I understand a lot of the benefit of ripgrep comes from there.  What are your thoughts?

---

_Comment by @BurntSushi on 2017-12-02 21:58_

TL;DR ripgrep without SIMD should be just fine for now.

Overall, I'd say this is a dupe of #135.

Graceful downgrades with a single binary is indeed the intended course of action here. If you're forced to use stable Rust (which I suspect you are), then that is blocked on Rust and there's really nothing we can do about it on the ripgrep end of things. (Note that I'm [helping that effort upstream](https://github.com/rust-lang-nursery/stdsimd).)

Today, however, it would be possible for me to put up release artifacts with graceful downgrade, but that requires switching the regex crate (and `bytecount`) over to `stdsimd`, which I haven't gotten to yet. But of course, that doesn't help you because I produce the release artifacts using nightly Rust. :-)

AFAIK, the ripgrep in other package repos is compiled without SIMD (e.g. Archlinux or Homebrew).

---

_Label `duplicate` added by @BurntSushi on 2017-12-02 21:58_

---

_Label `question` added by @BurntSushi on 2017-12-02 21:58_

---

_Comment by @BurntSushi on 2017-12-02 22:01_

> but I understand a lot of the benefit of ripgrep comes from there

Broadly speaking, there are two cases where SIMD in ripgrep helps:

1. When searching for a small number of short patterns simultaneously. e.g., `foo|bar|quux` will be faster with SIMD than without. Note that it's not like this becomes dog slow without SIMD, it's still going to use a heavily optimized DFA. :-)
2. When searching a large file while also enabling line numbers. This might be surprising, but ripgrep uses the `bytecount` crate to count lines, which has heavily optimized SSE and AVX routines. Again, even without SIMD here (which was the case in the benchmarks on the introductory blog post), ripgrep is still going to use an optimized routine to count lines.

Other than those two things, there shouldn't be a performance difference. ripgrep without SIMD still uses `memchr` (which uses SIMD by virtue of `libc` on Linux), parallel search and heavily optimized support for processing `gitignore` files quickly.

So I'd say "ripgrep gets faster in some cases with SIMD" today. Note that over time, I'd expect the number of cases to rise as I add more SIMD based optimizations, but I'd measure that on a scale of years. Hopefully SIMD will be in stable Rust before then. :-)

---

_Comment by @hlieberman on 2017-12-02 23:17_

Sounds good.  A follow-up question: what are your thoughts about potentially having to support dependencies at versions greater than the ones that you have specified in the cargo file?

---

_Comment by @BurntSushi on 2017-12-03 00:05_

@hlieberman I think the answer there is just keep upgrading the `Cargo.toml`/`Cargo.lock`. I try to be careful about upgrading deps that increase the minimum Rust version so that such things can be managed, but otherwise I'm typically happy to just keeping moving the ball forward.

---

_Closed by @BurntSushi on 2017-12-06 12:13_

---

_Comment by @BurntSushi on 2018-03-13 03:22_

This should be fixed by https://github.com/BurntSushi/ripgrep/pull/857 in the next release.

---

```yaml
number: 265
title: detect simd and avx features during runtime?
type: issue
state: closed
author: vladon
labels: []
assignees: []
created_at: 2016-12-02T18:15:13Z
updated_at: 2016-12-02T23:43:23Z
url: https://github.com/BurntSushi/ripgrep/issues/265
synced_at: 2026-01-12T16:13:21Z
```

# detect simd and avx features during runtime?

---

_@vladon_

I'm building deb package (for myself) of ripgrep, and want to distribute it to machines with and without SIMD and AVX.

Is it possible that ripgrep will detect SIMD and AVX features at runtime and use or not use them accordingly?

---

_Comment by @BurntSushi on 2016-12-02 23:29_

Nope. Two reasons:

1. Firstly, I'm assuming you need to compile ripgrep with stable Rust. If so, SIMD isn't supported so you couldn't enabled it even if you wanted to.
2. Rust doesn't have runtime CPU feature detection yet. We're working on a proposal to get that (and various other SIMD things) stabilized, but it will be months (or more) before it makes its way to stable Rust.

I'm going to close this because it should eventually happen automatically.

---

_Closed by @BurntSushi on 2016-12-02 23:29_

---

_Comment by @vladon on 2016-12-02 23:40_

OK, it was just a question.
In Rust, can we write CPU detection (actually running `cpuid` instruction) like in C++?

---

_Comment by @BurntSushi on 2016-12-02 23:42_

@vladon You would have to write Assembly to check the cpuid and link it into your Rust binary (Rust doesn't have an otherwise stable way of accessing `cpuid` information). Even if we did that, it's not sufficient, because you need compiler support for emitting SIMD intrinsics without telling the compiler to explicitly compile with, say, AVX2 support. (In Clang and gcc, this is the `__attribute__(__target__)` feature.) We don't have that yet either. Soon though!

---

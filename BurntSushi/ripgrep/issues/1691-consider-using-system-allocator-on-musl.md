```yaml
number: 1691
title: Consider using system allocator on musl
type: issue
state: closed
author: Logarithmus
labels: []
assignees: []
created_at: 2020-09-29T00:21:35Z
updated_at: 2021-07-11T04:34:37Z
url: https://github.com/BurntSushi/ripgrep/issues/1691
synced_at: 2026-01-12T16:13:24Z
```

# Consider using system allocator on musl

---

_@Logarithmus_

Recently `musl-1.2.1` has been released. Among other improvements, this version features new implementation of allocator called *mallocng*. My proposal is to rerun benchmarks with this new version of `musl`. Maybe we can use musl's allocator now instead of `jemalloc`.
More info: https://musl.libc.org/releases.html


---

_Comment by @wendajiang on 2021-02-25 02:45_

In readme, there is no declare that exec command "cargo build --release --target x86_64-unknown-linux-musl" need to install musl-gcc?

---

_Comment by @BurntSushi on 2021-02-25 08:00_

@wendajiang please don't hijack issues with unrelated problems. Please file a new issue. And please complete the issue template.

---

_Comment by @BurntSushi on 2021-05-31 23:46_

Using musl with the system allocator on a checkout of https://github.com/BurntSushi/linux:

```
$ hyperfine -i 'rg-musl-sys zqzqzqzqzq'
Benchmark #1: rg-musl-sys zqzqzqzqzq
  Time (mean ± σ):     286.1 ms ±   5.5 ms    [User: 1.582 s, System: 0.520 s]
  Range (min … max):   282.2 ms … 300.7 ms    10 runs
```

And now with jemalloc:

```
$ hyperfine -i 'rg-musl-jemalloc zqzqzqzqzq'
Benchmark #1: rg-musl-jemalloc zqzqzqzqzq
  Time (mean ± σ):     161.0 ms ±   2.0 ms    [User: 665.2 ms, System: 524.7 ms]
  Range (min … max):   157.4 ms … 164.5 ms    18 runs
```

IIRC, this is an improvement, but jemalloc still has a noticeable leg up.

You might wonder why ripgrep has such an allocation heavy workload. A lot of it is amortized, but a lot the allocations come from interactions with filesystem APIs. e.g., Converting strings to NUL-terminated strings to work on POSIX APIs. There is no real way to amortize those without building our own file system layer, which I'm not particularly inclined to do. (I started doing it in a branch on https://github.com/BurntSushi/walkdir, but it's a nightmare.)

So I think for now, I'm going to leave it as-is.

---

_Closed by @BurntSushi on 2021-05-31 23:46_

---

_Comment by @Piping on 2021-07-11 04:34_

mimalloc seems to be an awesome allocator. And it also has a crate to replace the rust default allocator.  is anyone interested in seeing the benchmark number?

---

```yaml
number: 9184
title: "Migrate to `zlib-rs`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/zlib-rs
created_at: 2024-11-18T01:51:21Z
updated_at: 2024-11-18T15:45:15Z
url: https://github.com/astral-sh/uv/pull/9184
synced_at: 2026-01-10T12:00:00Z
```

# Migrate to `zlib-rs`

---

_Pull request opened by @charliermarsh on 2024-11-18 01:51_

## Summary

I've tried this a few times; just curious if it passes tests.


---

_Comment by @charliermarsh on 2024-11-18 04:18_

I'm somewhat inclined to make this change? It doesn't look like `zlib-rs` is widely used yet, but rustls does use it for certificate decompression. And in local benchmarking, I can't find a clear difference between `zlib-rs` and `zlib-ng`...  Like, sometimes one is faster, sometimes the other is faster (though they're generally both faster than the default flate2 backend). It's really hard to benchmark this, since it only applies to remote wheels that we unzip as we stream, so network IO is also mixed in.


---

_Comment by @charliermarsh on 2024-11-18 04:22_

Ok, running against a local HTTP server.

First run:

```
Benchmark 1: ../../target/release/main (install-cold)
  Time (mean ± σ):      1.880 s ±  0.075 s    [User: 1.004 s, System: 0.803 s]
  Range (min … max):    1.806 s …  2.015 s    10 runs

Benchmark 2: ../../target/release/zlib-rs (install-cold)
  Time (mean ± σ):      1.816 s ±  0.176 s    [User: 0.885 s, System: 0.842 s]
  Range (min … max):    1.727 s …  2.310 s    10 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.

Benchmark 3: ../../target/release/default (install-cold)
  Time (mean ± σ):      2.035 s ±  0.171 s    [User: 1.100 s, System: 0.862 s]
  Range (min … max):    1.949 s …  2.517 s    10 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.

Summary
  ../../target/release/zlib-rs (install-cold) ran
    1.04 ± 0.11 times faster than ../../target/release/main (install-cold)
    1.12 ± 0.14 times faster than ../../target/release/default (install-cold)
```

Second run:

```
Benchmark 1: ../../target/release/main (install-cold)
  Time (mean ± σ):      1.869 s ±  0.058 s    [User: 0.996 s, System: 0.796 s]
  Range (min … max):    1.816 s …  1.982 s    10 runs

Benchmark 2: ../../target/release/zlib-rs (install-cold)
  Time (mean ± σ):      1.887 s ±  0.427 s    [User: 0.889 s, System: 0.919 s]
  Range (min … max):    1.718 s …  3.097 s    10 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.

Benchmark 3: ../../target/release/default (install-cold)
  Time (mean ± σ):      2.238 s ±  0.541 s    [User: 1.114 s, System: 1.036 s]
  Range (min … max):    1.945 s …  3.277 s    10 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.

Summary
  ../../target/release/main (install-cold) ran
    1.01 ± 0.23 times faster than ../../target/release/zlib-rs (install-cold)
    1.20 ± 0.29 times faster than ../../target/release/default (install-cold)
```

`main` is `libz-ng`, `zlib-rs` is this branch, and `default` is the default `flate2` backend.

So, anyway, this seems like a good change? And we get to remove a dependency that has caused a lot of trouble.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-11-18 04:22_

---

_Review requested from @zanieb by @charliermarsh on 2024-11-18 04:22_

---

_Review requested from @konstin by @charliermarsh on 2024-11-18 04:22_

---

_Label `internal` added by @charliermarsh on 2024-11-18 04:22_

---

_Marked ready for review by @charliermarsh on 2024-11-18 04:22_

---

_Comment by @charliermarsh on 2024-11-18 04:23_

I think the biggest counterargument is just that it isn't widely adopted right now.

---

_@konstin approved on 2024-11-18 09:49_

---

_Comment by @konstin on 2024-11-18 15:25_

I've confirmed that this removes the cmake build dep on windows

---

_Comment by @charliermarsh on 2024-11-18 15:28_

That's a big win.

---

_Comment by @zanieb on 2024-11-18 15:32_

I'm for it, easy to revert if we need to.

---

_@BurntSushi approved on 2024-11-18 15:43_

SGTM

---

_Merged by @charliermarsh on 2024-11-18 15:45_

---

_Closed by @charliermarsh on 2024-11-18 15:45_

---

_Branch deleted on 2024-11-18 15:45_

---

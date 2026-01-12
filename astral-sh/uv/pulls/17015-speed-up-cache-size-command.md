```yaml
number: 17015
title: Speed up cache size command
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - enhancement
  - performance
assignees: []
merged: true
base: main
head: cache-size-speed-up
created_at: 2025-12-06T14:20:33Z
updated_at: 2025-12-11T17:11:02Z
url: https://github.com/astral-sh/uv/pull/17015
synced_at: 2026-01-12T16:12:34Z
```

# Speed up cache size command

---

_@MatthewMckee4_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

`uv cache size` can be quite slow. Here i use https://github.com/sharkdp/diskus to walk the cache directory with in multiple threads.

Add cli option to set the number of threads and default to ` std::thread::available_parallelism()` or 1.

## Test Plan

Added cli statement with info log test.

I believe this is a fair test, where i set cache dir to a large directory.

```bash
matthew@matthew-main ~/develop/personal/uv                                                                                                                                                                                                                 [14:17:50]                                                                                                                                                                                                                                       [±cache-size-speed-up ✓▴]
> $ uv cache size --preview-features cache-size -H --cache-dir ~/develop/                                                                                                                                                                   [±cache-size-speed-up ✓▴]
75.7GiB

matthew@matthew-main ~/develop/personal/uv                                                                                                                                                                                                                 [14:18:24]
> $ hyperfine 'uv cache size --preview-features cache-size -H --cache-dir ~/develop/' 'target/debug/uv cache size --preview-features cache-size -H --cache-dir ~/develop/'                                                                  [±cache-size-speed-up ✓▴]
Benchmark 1: uv cache size --preview-features cache-size -H --cache-dir ~/develop/
  Time (mean ± σ):      1.059 s ±  0.014 s    [User: 0.171 s, System: 0.884 s]
  Range (min … max):    1.048 s …  1.097 s    10 runs

Benchmark 2: target/debug/uv cache size --preview-features cache-size -H --cache-dir ~/develop/
  Time (mean ± σ):     413.8 ms ±  17.1 ms    [User: 5789.2 ms, System: 1682.0 ms]
  Range (min … max):   386.3 ms … 441.6 ms    10 runs

Summary
  target/debug/uv cache size --preview-features cache-size -H --cache-dir ~/develop/ ran
    2.56 ± 0.11 times faster than uv cache size --preview-features cache-size -H --cache-dir ~/develop/  
```


---

_@zanieb reviewed on 2025-12-06 14:22_

---

_Review comment by @zanieb on `Cargo.toml`:206 on 2025-12-06 14:22_

We can't add Git dependencies, it'll break our crates.io publish

---

_@MatthewMckee4 reviewed on 2025-12-06 14:22_

---

_Review comment by @MatthewMckee4 on `Cargo.toml`:206 on 2025-12-06 14:22_

Ah, okay, happy for me to copy most of David's code over then?

---

_Converted to draft by @MatthewMckee4 on 2025-12-06 14:22_

---

_@zanieb reviewed on 2025-12-06 14:25_

---

_Review comment by @zanieb on `Cargo.toml`:206 on 2025-12-06 14:25_

I'm not sure yet.

@sharkdp are you interested in publishing the crate? Do you think we should just vendor the parts we need?

---

_@charliermarsh reviewed on 2025-12-06 14:35_

---

_Review comment by @charliermarsh on `Cargo.toml`:206 on 2025-12-06 14:35_

I think we should just vendor any speed-ups because this is pulling in a bunch of new dependencies (e.g., a second version of Clap).

---

_Marked ready for review by @MatthewMckee4 on 2025-12-06 16:30_

---

_@sharkdp reviewed on 2025-12-07 00:01_

---

_Review comment by @sharkdp on `Cargo.toml`:206 on 2025-12-07 00:01_

I [updated diskus](https://github.com/sharkdp/diskus/releases/tag/v0.9.0) today (it has always been published on crates.io, so no need for Git dependencies). You should now be able to pull it in as a relatively lightweight dependency using `default-features = false`. I also made an update specifically for counting *apparent file size* (which is what uv did here before: sum up `metadata.len()`), to make that more consistent with what `du -sb` does (exclude the size of directory entries themselves). I also cleaned up the diskus API a bit. If you want to depend on it here, the code should be something like:
```rs
let result = DiskUsage::new(&[cache.root()]).apparent_size().count();
let total_bytes = result.ignore_errors().size_in_bytes();
```
(or leave out the call to `.apparent_size()` if you'd rather want to count disk usage).

Note that diskus uses [a heuristic for the default number of workers](https://github.com/sharkdp/diskus/blob/9f33dfce45d5a819ab314f3c4bd586d27be7ae24/src/walk.rs#L162-L175) which should ideally not be overwritten for the best possible performance (at least according to [benchmarks](https://github.com/sharkdp/diskus/issues/38#issuecomment-612772867) I did years ago).

I am also completely fine with uv vendoring diskus. It's not a lot of code. In that case, you should potentially make those two updates mentioned above, though.

---

_@samypr100 reviewed on 2025-12-08 04:11_

---

_Review comment by @samypr100 on `crates/uv/src/commands/cache_size.rs`:41 on 2025-12-08 04:11_

```suggestion
    let disk_usage = if let Some(n) = threads {
        tracing::info!("Using {} threads to calculate cache size", n);
        DiskUsage::new(vec![cache.root().to_path_buf()]).num_workers(n)
    } else {
        DiskUsage::new(vec![cache.root().to_path_buf()])
    };
```

nit: avoid the mut

---

_@samypr100 reviewed on 2025-12-08 04:16_

---

_Review comment by @samypr100 on `crates/uv-cli/src/lib.rs`:941 on 2025-12-08 04:16_

suggestion(non-blocking): Assuming this flag does get added, I believe this be should be named something like `concurrency` to align with other similarly named sibling settings (e.g. concurrent-installs).

---

_Review comment by @samypr100 on `crates/uv/Cargo.toml`:118 on 2025-12-08 04:16_

nit: sort

---

_@samypr100 reviewed on 2025-12-08 04:16_

---

_Label `enhancement` added by @konstin on 2025-12-08 10:40_

---

_Label `performance` added by @konstin on 2025-12-08 10:40_

---

_@charliermarsh reviewed on 2025-12-11 15:28_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:941 on 2025-12-11 15:28_

Does it auto-detect by default? (If so, I think we should just remove.)

---

_@charliermarsh reviewed on 2025-12-11 15:31_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:941 on 2025-12-11 15:31_

Yeah let's omit this, then we can merge.

---

_@charliermarsh approved on 2025-12-11 15:47_

---

_@charliermarsh approved on 2025-12-11 17:04_

---

_@konstin approved on 2025-12-11 17:06_

---

_Merged by @charliermarsh on 2025-12-11 17:11_

---

_Closed by @charliermarsh on 2025-12-11 17:11_

---

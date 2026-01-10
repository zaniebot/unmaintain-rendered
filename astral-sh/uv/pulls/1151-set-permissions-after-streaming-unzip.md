```yaml
number: 1151
title: Set permissions after streaming unzip
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/perms
created_at: 2024-01-28T00:10:42Z
updated_at: 2024-01-28T00:22:45Z
url: https://github.com/astral-sh/uv/pull/1151
synced_at: 2026-01-10T15:39:03Z
```

# Set permissions after streaming unzip

---

_Pull request opened by @charliermarsh on 2024-01-28 00:10_

## Summary

When we migrated to an "unzip while we stream" solution, we lost the logic to set permissions on the extracted files, so executables in wheels were no longer executable. It turns out this is a little tricky, since the permissions metadata is in the central directory at the _end_ of the zip file, and the async ZIP reader explicitly stops iteration once it hits the central directory. (Specifically, it goes 4 bytes into the central directory, since it sees the 4-byte signature header and then stops.)

So, to solve that, I've added a `CentralDirectoryReader` that continues where that iterator left off. This required forking the async zip crate: https://github.com/charliermarsh/rs-async-zip/pull/1. It took a lot of fiddling but I'm quite confident in the code now, especially since the async zip crate validates the signature kind on every read.

The central directory is typically quite small (even for the Zig wheel, which is enormous, it's just around 1MB), so I don't expect this to have a high cost.

Closes https://github.com/astral-sh/puffin/issues/1148.


---

_@charliermarsh reviewed on 2024-01-28 00:11_

---

_Review comment by @charliermarsh on `Cargo.toml`:23 on 2024-01-28 00:11_

I also tagged this commit (as `d76801d`). It's `main` in that fork.

---

_Comment by @charliermarsh on 2024-01-28 00:13_

To ensure no fatal regression, I did benchmark this with:

```
python -m scripts.bench --puffin-path ./target/release/main --puffin-path ./target/release/puffin scripts/requirements/compiled/trio.txt --min-runs 50 --benchmark install-cold && python -m scripts.bench --puffin-path ./target/release/main --puffin-path ./target/release/puffin scripts/requirements/compiled/black.txt --min-runs 50 --benchmark install-cold && python -m scripts.bench --puffin-path ./target/release/main --puffin-path ./target/release/puffin scripts/requirements/compiled/pdm_2193.txt --min-runs 50 --benchmark install-cold
```

It's pretty noisy. On the second iteration, this branch was faster once, and `main` was faster twice:

```
Benchmark 1: ./target/release/main (install-cold)
  Time (mean ± σ):      1.297 s ±  0.323 s    [User: 0.372 s, System: 0.718 s]
  Range (min … max):    1.003 s …  2.214 s    50 runs

Benchmark 2: ./target/release/puffin (install-cold)
  Time (mean ± σ):      1.685 s ±  0.463 s    [User: 0.397 s, System: 0.813 s]
  Range (min … max):    1.012 s …  3.404 s    50 runs

Summary
  './target/release/main (install-cold)' ran
    1.30 ± 0.48 times faster than './target/release/puffin (install-cold)'
Benchmark 1: ./target/release/main (install-cold)
  Time (mean ± σ):     278.5 ms ±  10.7 ms    [User: 68.9 ms, System: 73.0 ms]
  Range (min … max):   259.7 ms … 303.5 ms    50 runs

Benchmark 2: ./target/release/puffin (install-cold)
  Time (mean ± σ):     313.1 ms ±  85.8 ms    [User: 70.2 ms, System: 78.9 ms]
  Range (min … max):   263.1 ms … 660.0 ms    50 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

Summary
  './target/release/main (install-cold)' ran
    1.12 ± 0.31 times faster than './target/release/puffin (install-cold)'
Benchmark 1: ./target/release/main (install-cold)
  Time (mean ± σ):      1.519 s ±  0.473 s    [User: 0.663 s, System: 0.569 s]
  Range (min … max):    1.167 s …  3.643 s    50 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

Benchmark 2: ./target/release/puffin (install-cold)
  Time (mean ± σ):      1.419 s ±  0.095 s    [User: 0.654 s, System: 0.624 s]
  Range (min … max):    1.149 s …  1.599 s    50 runs
```

On a previous benchmark run, it was a wash on all three.

---

_Label `bug` added by @charliermarsh on 2024-01-28 00:13_

---

_Merged by @charliermarsh on 2024-01-28 00:22_

---

_Closed by @charliermarsh on 2024-01-28 00:22_

---

_Branch deleted on 2024-01-28 00:22_

---

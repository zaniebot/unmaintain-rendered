```yaml
number: 1037
title: Use symlinks for directories entries in cache
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/symlink-dir
created_at: 2024-01-22T17:02:54Z
updated_at: 2024-01-24T02:11:36Z
url: https://github.com/astral-sh/uv/pull/1037
synced_at: 2026-01-10T15:39:03Z
```

# Use symlinks for directories entries in cache

---

_Pull request opened by @charliermarsh on 2024-01-22 17:02_

## Summary

One problem we have in the cache today is that we can't overwrite entries atomically, because we store unzipped _directories_ in the cache (which makes installation _much_ faster than storing zipped directories). So, if you ignore the existing contents of the cache when writing, you might run into an error, because you might attempt to write a directory where a directory already exists.

This is especially annoying for cache refresh, because in order to refresh the cache, we have to purge it (i.e., delete a bunch of stuff), which is also highly unsafe if Puffin is running across multiple threads or multiple processes.

The solution I'm proposing here is that whenever we persist a _directory_ to the cache, we persist it to a special "archive" bucket. Then, within the other buckets, directory entries are actually symlinks into that "archive" bucket. With symlinks, we can atomically replace, which means we can easily overwrite cache entries without having to delete from the cache.

The main downside is that we'll now accumulate dangling entries in the "archive" bucket, and so we'll need to implement some form of garbage collection to ensure that we remove entries with no symlinks. Another downside is that cache reads and writes will be a bit slower, since we need to deal with creating and resolving these symlinks.

As an example... after this change, the cache entry for this unzipped wheel is actually a symlink:

![Screenshot 2024-01-22 at 11 56 18 AM](https://github.com/astral-sh/puffin/assets/1309177/99ff6940-5096-4246-8d16-2a7bdcdd8d4b)

Then, within the archive directory, we actually have two unique entries (since I intentionally ran the command twice to ensure overwrites were safe):

![Screenshot 2024-01-22 at 11 56 22 AM](https://github.com/astral-sh/puffin/assets/1309177/717d04e2-25d9-4225-b190-bad1441868c6)


---

_@charliermarsh reviewed on 2024-01-22 17:10_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/distribution_database.rs`:173 on 2024-01-22 17:10_

This does mean that if a wheel at a certain URL gets updated, we'll store two copies of it in the archive directory, without deleting the old one. But I think we _cannot_ delete the old one if we want Puffin to be safe across processes. It needs to be deleted separately, out of band.


---

_Comment by @konstin on 2024-01-22 17:10_

In general, i like it!

On windows, symlinking is often not available, but if this also works with hardlinks it's fine.

I worry a bit about how large `archives` will become, is installing 10k wheels a realistic thing?

An alternative design would be using random names in the correct directory and use the link as in-dir redirect. We can GC by looking for randomly named dirs with no link pointing to them.

---

_Comment by @charliermarsh on 2024-01-22 17:12_

> On windows, symlinking is often not available, but if this also works with hardlinks it's fine.

I think hardlinks don't work with directories, hmm...


---

_Comment by @charliermarsh on 2024-01-22 17:14_

Another option (which is what I think I'll have to do for source distributions, for reasons that won't be clear until a future PR) is that we store the name of the current artifact in a file, and then read the file to lookup the right artifact. Kind of like a manual symlink. It's so inefficient though.

---

_Comment by @BurntSushi on 2024-01-22 17:39_

Yeah Windows makes the symlinking idea more difficult. I don't know enough about symlinking on Windows to know whether it's a problem we can solve, but I've definitely run into the "symlinking not allowed" problem when testing symlink-related functionality (e.g., for crates like `ignore` and `walkdir`). In particular, the symlink function you used is [only available on Unix](https://docs.rs/tokio/1.34.0/tokio/fs/fn.symlink.html).

> This is especially annoying for cache refresh, because in order to refresh the cache, we have to purge it (i.e., delete a bunch of stuff), which is also highly unsafe if Puffin is running across multiple threads or multiple processes.

I thought we were using file locking? Can we use locking to synchronize on actions like this?

Another primitive that _might_ work here (although I'm not certain of it) is that [directory creation](https://doc.rust-lang.org/std/fs/fn.create_dir.html) should be atomic on all platforms we care about. I wonder if you can utilize this by treating purge as "move a directory" instead of "delete a directory." But I believe in order for this to work, we'd need to make use of directory handles and the various `at` APIs. Which... is probably a nightmare because Rust's standard library doesn't support it. We'd have to use something like [`cap-std`](https://docs.rs/cap-std/latest/cap_std/). (I also kind of wonder whether you'd need `cap-std` for the symlink approach in this PR to work as well.)

---

_Marked ready for review by @charliermarsh on 2024-01-23 01:36_

---

_Review requested from @konstin by @charliermarsh on 2024-01-23 01:37_

---

_Comment by @charliermarsh on 2024-01-23 02:07_

Including a bunch of installation benchmarks. It's `main` vs. the three branches in this stack (with `symlink-dir` being this branch).

First, `trio.txt`:

```
❯ python -m scripts.bench \
 --puffin-path ./target/release/main        --puffin-path ./target/release/id \
        --puffin-path ./target/release/manifest --puffin-path ./target/release/symlink-dir  \
        scripts/requirements/compiled/trio.txt --min-runs 50
Benchmark 1: ./target/release/main (install-cold)
  Time (mean ± σ):     750.5 ms ±  54.1 ms    [User: 303.6 ms, System: 701.7 ms]
  Range (min … max):   647.4 ms … 884.9 ms    50 runs

Benchmark 2: ./target/release/id (install-cold)
  Time (mean ± σ):     716.8 ms ±  47.7 ms    [User: 300.9 ms, System: 698.5 ms]
  Range (min … max):   630.2 ms … 869.1 ms    50 runs

Benchmark 3: ./target/release/manifest (install-cold)
  Time (mean ± σ):     709.1 ms ±  62.1 ms    [User: 301.3 ms, System: 698.1 ms]
  Range (min … max):   605.9 ms … 887.9 ms    50 runs

Benchmark 4: ./target/release/symlink-dir (install-cold)
  Time (mean ± σ):     725.8 ms ±  57.8 ms    [User: 300.6 ms, System: 674.5 ms]
  Range (min … max):   622.2 ms … 955.0 ms    50 runs

Summary
  './target/release/manifest (install-cold)' ran
    1.01 ± 0.11 times faster than './target/release/id (install-cold)'
    1.02 ± 0.12 times faster than './target/release/symlink-dir (install-cold)'
    1.06 ± 0.12 times faster than './target/release/main (install-cold)'
Benchmark 1: ./target/release/main (install-warm)
  Time (mean ± σ):      52.8 ms ±  10.0 ms    [User: 12.1 ms, System: 96.9 ms]
  Range (min … max):    47.4 ms …  97.5 ms    50 runs

  Warning: The first benchmarking run for this command was significantly slower than the rest (71.7 ms). This could be caused by (filesystem) caches that were not filled until after the first run. You should consider using the '--warmup' option to fill those caches before the actual benchmark. Alternatively, use the '--prepare' option to clear the caches before each timing run.

Benchmark 2: ./target/release/id (install-warm)
  Time (mean ± σ):      51.9 ms ±   8.0 ms    [User: 12.1 ms, System: 100.8 ms]
  Range (min … max):    48.2 ms …  92.8 ms    50 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

Benchmark 3: ./target/release/manifest (install-warm)
  Time (mean ± σ):      54.5 ms ±  11.2 ms    [User: 12.0 ms, System: 102.5 ms]
  Range (min … max):    47.8 ms …  96.6 ms    50 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

Benchmark 4: ./target/release/symlink-dir (install-warm)
  Time (mean ± σ):      57.4 ms ±  10.5 ms    [User: 12.2 ms, System: 100.1 ms]
  Range (min … max):    46.5 ms … 100.5 ms    50 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

Summary
  './target/release/id (install-warm)' ran
    1.02 ± 0.25 times faster than './target/release/main (install-warm)'
    1.05 ± 0.27 times faster than './target/release/manifest (install-warm)'
    1.11 ± 0.27 times faster than './target/release/symlink-dir (install-warm)'
```

---

_Comment by @charliermarsh on 2024-01-23 02:07_

Second, PDM:

```
❯ python -m scripts.bench \
 --puffin-path ./target/release/main        --puffin-path ./target/release/id \
        --puffin-path ./target/release/manifest --puffin-path ./target/release/symlink-dir  \
        scripts/requirements/compiled/pdm_2193.txt --min-runs 50
Benchmark 1: ./target/release/main (install-cold)
  Time (mean ± σ):     949.5 ms ±  78.6 ms    [User: 556.0 ms, System: 610.3 ms]
  Range (min … max):   834.8 ms … 1227.1 ms    50 runs

Benchmark 2: ./target/release/id (install-cold)
  Time (mean ± σ):     961.9 ms ± 118.3 ms    [User: 559.2 ms, System: 612.0 ms]
  Range (min … max):   817.3 ms … 1330.5 ms    50 runs

Benchmark 3: ./target/release/manifest (install-cold)
  Time (mean ± σ):     966.1 ms ± 116.4 ms    [User: 559.0 ms, System: 615.5 ms]
  Range (min … max):   822.9 ms … 1277.8 ms    50 runs

Benchmark 4: ./target/release/symlink-dir (install-cold)
  Time (mean ± σ):     939.1 ms ±  76.3 ms    [User: 557.2 ms, System: 611.7 ms]
  Range (min … max):   847.3 ms … 1179.1 ms    50 runs

Summary
  './target/release/symlink-dir (install-cold)' ran
    1.01 ± 0.12 times faster than './target/release/main (install-cold)'
    1.02 ± 0.15 times faster than './target/release/id (install-cold)'
    1.03 ± 0.15 times faster than './target/release/manifest (install-cold)'
Benchmark 1: ./target/release/main (install-warm)
  Time (mean ± σ):      40.9 ms ±   7.2 ms    [User: 8.7 ms, System: 56.8 ms]
  Range (min … max):    36.8 ms …  74.0 ms    50 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

Benchmark 2: ./target/release/id (install-warm)
  Time (mean ± σ):      43.8 ms ±  12.7 ms    [User: 8.7 ms, System: 56.3 ms]
  Range (min … max):    36.5 ms …  96.8 ms    50 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

Benchmark 3: ./target/release/manifest (install-warm)
  Time (mean ± σ):      42.8 ms ±  11.6 ms    [User: 8.6 ms, System: 60.2 ms]
  Range (min … max):    36.5 ms …  89.4 ms    50 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

Benchmark 4: ./target/release/symlink-dir (install-warm)
  Time (mean ± σ):      41.7 ms ±   8.0 ms    [User: 8.6 ms, System: 60.2 ms]
  Range (min … max):    37.0 ms …  72.1 ms    50 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

Summary
  './target/release/main (install-warm)' ran
    1.02 ± 0.26 times faster than './target/release/symlink-dir (install-warm)'
    1.04 ± 0.34 times faster than './target/release/manifest (install-warm)'
    1.07 ± 0.36 times faster than './target/release/id (install-warm)'
```

---

_Comment by @charliermarsh on 2024-01-23 02:07_

Third, Black:

```
❯ python -m scripts.bench \
 --puffin-path ./target/release/main        --puffin-path ./target/release/id \
        --puffin-path ./target/release/manifest --puffin-path ./target/release/symlink-dir  \
        scripts/requirements/compiled/black.txt --min-runs 50
Benchmark 1: ./target/release/main (install-cold)
  Time (mean ± σ):     224.8 ms ±  15.7 ms    [User: 60.9 ms, System: 75.0 ms]
  Range (min … max):   201.3 ms … 275.4 ms    50 runs

Benchmark 2: ./target/release/id (install-cold)
  Time (mean ± σ):     224.0 ms ±  16.6 ms    [User: 60.5 ms, System: 75.8 ms]
  Range (min … max):   196.0 ms … 273.1 ms    50 runs

Benchmark 3: ./target/release/manifest (install-cold)
  Time (mean ± σ):     226.5 ms ±  13.9 ms    [User: 61.0 ms, System: 76.1 ms]
  Range (min … max):   200.9 ms … 266.2 ms    50 runs

Benchmark 4: ./target/release/symlink-dir (install-cold)
  Time (mean ± σ):     226.5 ms ±  25.8 ms    [User: 60.3 ms, System: 75.7 ms]
  Range (min … max):   194.2 ms … 367.6 ms    50 runs

Summary
  './target/release/id (install-cold)' ran
    1.00 ± 0.10 times faster than './target/release/main (install-cold)'
    1.01 ± 0.10 times faster than './target/release/manifest (install-cold)'
    1.01 ± 0.14 times faster than './target/release/symlink-dir (install-cold)'
Benchmark 1: ./target/release/main (install-warm)
  Time (mean ± σ):      13.9 ms ±   0.3 ms    [User: 5.6 ms, System: 18.5 ms]
  Range (min … max):    13.1 ms …  14.7 ms    50 runs

Benchmark 2: ./target/release/id (install-warm)
  Time (mean ± σ):      14.4 ms ±   1.5 ms    [User: 5.6 ms, System: 18.8 ms]
  Range (min … max):    13.3 ms …  22.7 ms    50 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

Benchmark 3: ./target/release/manifest (install-warm)
  Time (mean ± σ):      14.2 ms ±   1.0 ms    [User: 5.6 ms, System: 18.5 ms]
  Range (min … max):    13.2 ms …  18.0 ms    50 runs

Benchmark 4: ./target/release/symlink-dir (install-warm)
  Time (mean ± σ):      14.1 ms ±   1.3 ms    [User: 5.6 ms, System: 18.5 ms]
  Range (min … max):    13.0 ms …  22.3 ms    50 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

Summary
  './target/release/main (install-warm)' ran
    1.02 ± 0.10 times faster than './target/release/symlink-dir (install-warm)'
    1.02 ± 0.08 times faster than './target/release/manifest (install-warm)'
    1.03 ± 0.11 times faster than './target/release/id (install-warm)'
```

---

_Comment by @charliermarsh on 2024-01-23 02:08_

The benchmarks are kind of just all over the place, which makes me think that it's all noise.

---

_Merged by @charliermarsh on 2024-01-23 19:52_

---

_Closed by @charliermarsh on 2024-01-23 19:52_

---

_Branch deleted on 2024-01-23 19:52_

---

_@BurntSushi reviewed on 2024-01-24 01:37_

---

_Review comment by @BurntSushi on `crates/puffin-fs/src/lib.rs`:14 on 2024-01-24 01:37_

How did you work around permission issues on Windows? My understanding is that junctions don't require extra permissions on Windows, but symlinks do.

---

_@charliermarsh reviewed on 2024-01-24 02:11_

---

_Review comment by @charliermarsh on `crates/puffin-fs/src/lib.rs`:14 on 2024-01-24 02:11_

That's a TODO to solve. I think we'll need to use junctions. Or we can find another way to implement the primitive we need (safe replacement), it's well-isolated in the cache.

---

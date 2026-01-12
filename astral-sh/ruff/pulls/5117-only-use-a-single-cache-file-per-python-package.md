```yaml
number: 5117
title: Only use a single cache file per Python package
type: pull_request
state: merged
author: Thomasdezeeuw
labels: []
assignees: []
merged: true
base: main
head: thomas/cache
created_at: 2023-06-15T09:43:11Z
updated_at: 2023-06-19T15:46:15Z
url: https://github.com/astral-sh/ruff/pull/5117
synced_at: 2026-01-12T03:43:30Z
```

# Only use a single cache file per Python package

---

_Pull request opened by @Thomasdezeeuw on 2023-06-15 09:43_

## Summary

This changes the caching design from one cache file per source file, to
one cache file per package. This greatly reduces the amount of cache
files that are opened and written, while maintaining roughly the same
(combined) size as bincode is very compact.

Below are some very much not scientific performance tests. It uses
projects/sources to check:

* small.py: single, 31 bytes Python file with 2 errors.
* test.py: single, 43k Python file with 8 errors.
* fastapi: FastAPI repo, 1134 files checked, 0 errors.

Source   | Before # files | After # files | Before size | After size
-------|-------|-------|-------|-------
small.py | 1              | 1             | 20 K        | 20 K
test.py  | 1              | 1             | 60 K        | 60 K
fastapi  | 1134           | 518           | 4.5 M       | 2.3 M

One question that might come up is why fastapi still has 518 cache files
and not 1? That is because this is using the existing package
resolution, which sees examples, docs, etc. as separate from the "main"
source code (in the fastapi directory in the repo). In this future it
might be worth consider switching to a one cache file per repo strategy.

This new design is not perfect and does have a number of known issues.
First, like the old design it doesn't remove the cache for a source file
that has been (re)moved until `ruff clean` is called.

Second, this currently uses a large mutex around the mutation of the
package cache (e.g. inserting result). This could be (or become) a
bottleneck. It's future work to test and improve this (if needed).

Third, currently the packages and opened and stored in a sequential
loop, this could be done parallel. This is also future work.


## Test Plan

Run `ruff check` (with caching enabled) twice on any Python source code and it should produce the same results.

After this general direction is approved I'll add some tests.

---

_Comment by @github-actions[bot] on 2023-06-15 09:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.3±0.02ms     6.5 MB/sec    1.00      6.3±0.02ms     6.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   1328.0±6.19µs    12.5 MB/sec    1.00   1329.4±2.50µs    12.5 MB/sec
formatter/numpy/globals.py                 1.00    128.6±0.30µs    23.0 MB/sec    1.00    128.7±0.51µs    22.9 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.01ms     9.9 MB/sec    1.00      2.6±0.01ms     9.9 MB/sec
linter/all-rules/large/dataset.py          1.01     13.6±0.06ms     3.0 MB/sec    1.00     13.5±0.12ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    422.0±0.67µs     7.0 MB/sec    1.00    420.4±0.76µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.0 MB/sec    1.00      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1471.1±1.50µs    11.3 MB/sec    1.00   1464.2±3.88µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.8±0.62µs    18.0 MB/sec    1.00    163.5±0.36µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.02ms     8.3 MB/sec    1.00      3.1±0.00ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     12.2±0.64ms     3.3 MB/sec    1.00     12.1±0.59ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.18ms     7.3 MB/sec    1.01      2.3±0.15ms     7.2 MB/sec
formatter/numpy/globals.py                 1.00   226.3±27.99µs    13.0 MB/sec    1.00   227.3±17.42µs    13.0 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.24ms     5.5 MB/sec    1.00      4.7±0.24ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.00     25.1±0.84ms  1657.0 KB/sec    1.00     25.1±0.82ms  1658.6 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      6.5±0.32ms     2.6 MB/sec    1.00      6.4±0.28ms     2.6 MB/sec
linter/all-rules/numpy/globals.py          1.02   707.7±52.33µs     4.2 MB/sec    1.00   690.6±40.91µs     4.3 MB/sec
linter/all-rules/pydantic/types.py         1.00     10.7±0.45ms     2.4 MB/sec    1.02     10.8±0.49ms     2.4 MB/sec
linter/default-rules/large/dataset.py      1.00     12.9±0.39ms     3.2 MB/sec    1.00     12.9±0.42ms     3.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.5±0.10ms     6.8 MB/sec    1.00      2.4±0.09ms     6.8 MB/sec
linter/default-rules/numpy/globals.py      1.03   298.8±24.69µs     9.9 MB/sec    1.00   291.0±16.26µs    10.1 MB/sec
linter/default-rules/pydantic/types.py     1.05      5.7±0.28ms     4.4 MB/sec    1.00      5.5±0.25ms     4.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @konstin by @Thomasdezeeuw on 2023-06-15 11:52_

---

_Review requested from @charliermarsh by @Thomasdezeeuw on 2023-06-15 11:52_

---

_Review comment by @konstin on `crates/ruff_cli/src/cache.rs`:27 on 2023-06-15 12:16_

```suggestion
    /// Not stored to disk itself, just used as a storage location.
```

---

_Review comment by @konstin on `crates/ruff_cli/src/cache.rs`:141 on 2023-06-15 12:25_

That's helpful, i've already been wondering about this when reading the code above

---

_Review comment by @konstin on `crates/ruff_cli/src/cache.rs`:235 on 2023-06-15 12:30_

You inherited this code and it's fine as currently the difference between cache and no-cache still isn't so big, but i wonder if it makes sense to switch to something else, is there e.g. a library that could hash our type layout or something? 

---

_Review comment by @konstin on `crates/ruff_cli/src/diagnostics.rs`:214 on 2023-06-15 12:32_

i don't think we need to cache parse errors

---

_@konstin approved on 2023-06-15 12:33_

Can't judge the overall strategy, but the code looks good.

I know it has been missing before, but could you maybe at some module level docs or a readme explaining what information we cache?

---

_@Thomasdezeeuw reviewed on 2023-06-15 15:39_

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/cache.rs`:235 on 2023-06-15 15:39_

We could switch to some kind of "cache version" here and update it whenever the layout of one of related types changes, but that would be annoying to maintain.

I don't really know of anything of the top of my head that hashes the layout of the type. The closes is using something like a `TypeId`, but that is different across compilations, so not ideal.

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/diagnostics.rs`:214 on 2023-06-15 15:40_

:+1: will remove the TODO.

---

_@Thomasdezeeuw reviewed on 2023-06-15 15:40_

---

_Comment by @charliermarsh on 2023-06-15 16:16_

Since he's back on Monday anyway, would love to get @MichaReiser's eyes on the cache changes before we merge, since he's worked on the cache a bit in the past.

---

_Comment by @charliermarsh on 2023-06-15 16:19_

> Below are some very much not scientific performance tests.

Interesting! More-so than cache _size_, I think the metric I'd be most interested in is _speed_ (throughput?). Does using a single cache file per Python package make it faster to read from / write to the cache?


---

_@konstin reviewed on 2023-06-15 19:06_

---

_Review comment by @konstin on `crates/ruff_cli/src/cache.rs`:235 on 2023-06-15 19:06_

that's totally fair!

---

_@charliermarsh reviewed on 2023-06-15 22:02_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/cache.rs`:44 on 2023-06-15 22:02_

I think this approach solves a problem that exists with the cache today, but I'd like to confirm...

Today, we use the cache key as the filename. When file contents are invalidated, we write a new file to the cache (since the cache key has changed), but leave the old file around. So the cache, as it exists today, grows monotonically, and gets bigger and bigger over time.

As far as I can tell, this new approach will replace the cache entirely when contents change, so we're effectively bounding the size of the cache by the size of the package itself. Is that right? If so, it should completed solve the problem of the cache growing eternally as seen in #5132.

---

_@charliermarsh reviewed on 2023-06-15 22:05_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/cache.rs`:44 on 2023-06-15 22:05_

(The benefit of the current approach is that if you change a setting, then revert that change, we still have the cached results from the previous settings. But that's so rare as to be not at all worthwhile.)

---

_@charliermarsh reviewed on 2023-06-15 22:06_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/run.rs`:83 on 2023-06-15 22:06_

Nit: we generally use the `TODO(charlie)` style (here, `TODO(thomas)`). The author of the TODO isn't necessarily responsible for resolving it, but they are considered the most-knowledge person, and so we generally preserve that in the TODO itself.

---

_@charliermarsh reviewed on 2023-06-15 22:08_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/cache.rs`:117 on 2023-06-15 22:08_

Does this unnecessarily clone in the `file.last_modified != file_last_modified` case?

---

_@charliermarsh reviewed on 2023-06-15 22:13_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/run.rs`:110 on 2023-06-15 22:13_

It would be good to know how much overhead we're adding here, by creating this map (and then doing the lookups below).

---

_@charliermarsh reviewed on 2023-06-15 22:17_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:118 on 2023-06-15 22:17_

Is this identical to `FileTime::from_last_modification_time`? I can't remember why, but I feel like I had to change our choice of timestamp at least once, so it's worth double-checking what we used before and why, and if/how this differs.

---

_Comment by @charliermarsh on 2023-06-15 22:18_

The code here looks good! My main outstanding questions are just around runtime performance, i.e., does this perform better or worse than before for projects with various size and diagnostic characteristics?

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/cache.rs`:44 on 2023-06-16 08:22_

> As far as I can tell, this new approach will replace the cache entirely when contents change, so we're effectively bounding the size of the cache by the size of the package itself. Is that right? If so, it should completed solve the problem of the cache growing eternally as seen in #5132.

It almost solves it, but there are some limitations. This design doesn't remove the `files` hashmap entry for a source file that has been (re)moved. We could solve this my not reusing the read cache and instead recreate the `files` hashmap before storing it.

The second limitation is the hashkey generation. Because it uses various settings to ensure the cache is valid it doesn't actually remove old cache files if, e.g., a project changes the settings. I think this can be solved by splitting the file name into `$project_hash.$every_thing_else_hash` this way we can do a `rm $project_hash.*` periodically or something. But as discussed yesterday for now we can use `ruff clean` to "solve" both problems (by removing all cache files for the project, or after https://github.com/astral-sh/ruff/pull/5122 all cache files for all projects).

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/cache.rs`:117 on 2023-06-16 08:24_

Yes, I tried to reduce the locking time (initially I had some more code in here), but a single compare in the lock should be fine.

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/commands/run.rs`:110 on 2023-06-16 08:27_

I'll do some performance tests, but I've already addressed this in https://github.com/astral-sh/ruff/pull/5120.

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/diagnostics.rs`:118 on 2023-06-16 08:31_

`FileTime::from_last_modification_time` uses `mtime`
(https://github.com/alexcrichton/filetime/blob/05d09524fc4939b18851ddbec721b64ea1188d15/src/unix/mod.rs#L73-L78) and so does `Metadata::modified` per the docs (
https://doc.rust-lang.org/std/fs/struct.Metadata.html#method.modified).

---

_@Thomasdezeeuw reviewed on 2023-06-16 08:32_

---

_Comment by @Thomasdezeeuw on 2023-06-16 09:40_

# Setup

Using an nvme drive (~25GiB/s read).

Binary versions:

ruff_old: 89a8c313a15ceb771354faff8bc346e09d3531f3
ruff_new: 749f0ed6740b6980105dd01c719af4dbd3705605
ruff_parallel: 83d447517c865f504a3897b8a07d45a17dec4ac8 (https://github.com/astral-sh/ruff/pull/5120)

# Run Time (No Cache)

This tests the runs without a prepared cache.

## Test script

```bash
#!/bin/env bash

set -eu

cache_dir="./cache"
path="$1"

hyperfine \
	--warmup 3 \
	--prepare "rm -rf '$cache_dir'" \
	--parameter-list bin ruff_old,ruff_new,ruff_parallel \
	--ignore-failure \
	-N \
	"./bin/{bin} check --cache-dir $cache_dir $path"
```

## Results

Airflow:

```log
Benchmark 1: ./bin/ruff_old check --cache-dir ./cache repos/airflow/
  Time (mean ± σ):     123.0 ms ±   2.9 ms    [User: 2275.8 ms, System: 236.8 ms]
  Range (min … max):   118.8 ms … 128.4 ms    21 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ./bin/ruff_new check --cache-dir ./cache repos/airflow/
  Time (mean ± σ):     123.9 ms ±   4.3 ms    [User: 2209.9 ms, System: 118.2 ms]
  Range (min … max):   118.7 ms … 136.2 ms    23 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: ./bin/ruff_parallel check --cache-dir ./cache repos/airflow/
  Time (mean ± σ):     121.5 ms ±   2.7 ms    [User: 2275.6 ms, System: 130.9 ms]
  Range (min … max):   115.9 ms … 126.5 ms    24 runs

  Warning: Ignoring non-zero exit code.

Summary
  ./bin/ruff_parallel check --cache-dir ./cache repos/airflow/ ran
    1.01 ± 0.03 times faster than ./bin/ruff_old check --cache-dir ./cache repos/airflow/
    1.02 ± 0.04 times faster than ./bin/ruff_new check --cache-dir ./cache repos/airflow/
```

Bokeh:

```log
Benchmark 1: ./bin/ruff_old check --cache-dir ./cache repos/bokeh/
  Time (mean ± σ):      42.6 ms ±   1.7 ms    [User: 493.6 ms, System: 125.2 ms]
  Range (min … max):    40.3 ms …  52.8 ms    63 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ./bin/ruff_new check --cache-dir ./cache repos/bokeh/
  Time (mean ± σ):      46.7 ms ±   0.9 ms    [User: 484.9 ms, System: 69.1 ms]
  Range (min … max):    44.5 ms …  48.7 ms    59 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: ./bin/ruff_parallel check --cache-dir ./cache repos/bokeh/
  Time (mean ± σ):      41.0 ms ±   1.4 ms    [User: 507.5 ms, System: 78.6 ms]
  Range (min … max):    38.9 ms …  49.3 ms    72 runs

  Warning: Ignoring non-zero exit code.

Summary
  ./bin/ruff_parallel check --cache-dir ./cache repos/bokeh/ ran
    1.04 ± 0.05 times faster than ./bin/ruff_old check --cache-dir ./cache repos/bokeh/
    1.14 ± 0.04 times faster than ./bin/ruff_new check --cache-dir ./cache repos/bokeh/
```

CPython:

```log
Benchmark 1: ./bin/ruff_old check --cache-dir ./cache repos/cpython/
  Time (mean ± σ):     142.1 ms ±   2.5 ms    [User: 2686.7 ms, System: 161.0 ms]
  Range (min … max):   138.6 ms … 146.2 ms    20 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ./bin/ruff_new check --cache-dir ./cache repos/cpython/
  Time (mean ± σ):     154.6 ms ±   3.0 ms    [User: 2668.3 ms, System: 101.4 ms]
  Range (min … max):   151.6 ms … 160.8 ms    18 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: ./bin/ruff_parallel check --cache-dir ./cache repos/cpython/
  Time (mean ± σ):     152.6 ms ±   4.6 ms    [User: 2731.2 ms, System: 121.3 ms]
  Range (min … max):   145.6 ms … 163.1 ms    19 runs

  Warning: Ignoring non-zero exit code.

Summary
  ./bin/ruff_old check --cache-dir ./cache repos/cpython/ ran
    1.07 ± 0.04 times faster than ./bin/ruff_parallel check --cache-dir ./cache repos/cpython/
    1.09 ± 0.03 times faster than ./bin/ruff_new check --cache-dir ./cache repos/cpython/
```

FastAPI:

```log
Benchmark 1: ./bin/ruff_old check --cache-dir ./cache repos/fastapi/
  Time (mean ± σ):      41.6 ms ±   2.6 ms    [User: 252.0 ms, System: 178.3 ms]
  Range (min … max):    33.4 ms …  51.2 ms    69 runs

Benchmark 2: ./bin/ruff_new check --cache-dir ./cache repos/fastapi/
  Time (mean ± σ):      42.5 ms ±   1.2 ms    [User: 248.4 ms, System: 41.7 ms]
  Range (min … max):    40.0 ms …  46.7 ms    60 runs

Benchmark 3: ./bin/ruff_parallel check --cache-dir ./cache repos/fastapi/
  Time (mean ± σ):      38.0 ms ±   1.4 ms    [User: 253.2 ms, System: 60.4 ms]
  Range (min … max):    35.1 ms …  44.8 ms    75 runs

Summary
  ./bin/ruff_parallel check --cache-dir ./cache repos/fastapi/ ran
    1.10 ± 0.08 times faster than ./bin/ruff_old check --cache-dir ./cache repos/fastapi/
    1.12 ± 0.05 times faster than ./bin/ruff_new check --cache-dir ./cache repos/fastapi/
```

Jupyter server:

```log
Benchmark 1: ./bin/ruff_old check --cache-dir ./cache repos/jupyter_server/
  Time (mean ± σ):      25.0 ms ±   1.0 ms    [User: 134.1 ms, System: 38.3 ms]
  Range (min … max):    22.9 ms …  27.8 ms    117 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ./bin/ruff_new check --cache-dir ./cache repos/jupyter_server/
  Time (mean ± σ):      25.3 ms ±   1.1 ms    [User: 127.2 ms, System: 35.6 ms]
  Range (min … max):    23.1 ms …  34.3 ms    120 runs

  Warning: Ignoring non-zero exit code.
  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.

Benchmark 3: ./bin/ruff_parallel check --cache-dir ./cache repos/jupyter_server/
  Time (mean ± σ):      25.5 ms ±   1.3 ms    [User: 135.4 ms, System: 35.0 ms]
  Range (min … max):    23.0 ms …  33.3 ms    112 runs

  Warning: Ignoring non-zero exit code.

Summary
  ./bin/ruff_old check --cache-dir ./cache repos/jupyter_server/ ran
    1.01 ± 0.06 times faster than ./bin/ruff_new check --cache-dir ./cache repos/jupyter_server/
    1.02 ± 0.06 times faster than ./bin/ruff_parallel check --cache-dir ./cache repos/jupyter_server/
```





# Run Time (Cached)

This tests the runs with a prepared cache.

## Test Scripts

```bash
#!/bin/env bash

set -eu

cache_dir="./cache"
path="$1"

rm -rf "$cache_dir"

hyperfine \
	--warmup 3 \
	--parameter-list bin ruff_old,ruff_new,ruff_parallel \
	--ignore-failure \
	-N \
	"./bin/{bin} check --cache-dir $cache_dir $path"
```

## Results

Airflow:

```
Benchmark 1: ./bin/ruff_old check --cache-dir ./cache repos/airflow/
  Time (mean ± σ):      34.6 ms ±   0.7 ms    [User: 63.3 ms, System: 57.3 ms]
  Range (min … max):    32.3 ms …  36.4 ms    88 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ./bin/ruff_new check --cache-dir ./cache repos/airflow/
  Time (mean ± σ):      44.8 ms ±   0.7 ms    [User: 48.8 ms, System: 43.5 ms]
  Range (min … max):    41.9 ms …  46.3 ms    64 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: ./bin/ruff_parallel check --cache-dir ./cache repos/airflow/
  Time (mean ± σ):      36.8 ms ±   0.7 ms    [User: 62.8 ms, System: 67.1 ms]
  Range (min … max):    35.4 ms …  38.4 ms    81 runs

  Warning: Ignoring non-zero exit code.

Summary
  ./bin/ruff_old check --cache-dir ./cache repos/airflow/ ran
    1.06 ± 0.03 times faster than ./bin/ruff_parallel check --cache-dir ./cache repos/airflow/
    1.29 ± 0.03 times faster than ./bin/ruff_new check --cache-dir ./cache repos/airflow/
```

Bokeh:

```
Benchmark 1: ./bin/ruff_old check --cache-dir ./cache repos/bokeh/
  Time (mean ± σ):      20.2 ms ±   0.7 ms    [User: 23.3 ms, System: 34.7 ms]
  Range (min … max):    17.9 ms …  21.8 ms    151 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ./bin/ruff_new check --cache-dir ./cache repos/bokeh/
  Time (mean ± σ):      31.3 ms ±   0.9 ms    [User: 22.0 ms, System: 33.5 ms]
  Range (min … max):    28.2 ms …  33.0 ms    92 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: ./bin/ruff_parallel check --cache-dir ./cache repos/bokeh/
  Time (mean ± σ):      20.8 ms ±   0.6 ms    [User: 25.9 ms, System: 36.5 ms]
  Range (min … max):    19.0 ms …  22.6 ms    141 runs

  Warning: Ignoring non-zero exit code.

Summary
  ./bin/ruff_old check --cache-dir ./cache repos/bokeh/ ran
    1.03 ± 0.05 times faster than ./bin/ruff_parallel check --cache-dir ./cache repos/bokeh/
    1.55 ± 0.07 times faster than ./bin/ruff_new check --cache-dir ./cache repos/bokeh/
```

CPython:

```
Benchmark 1: ./bin/ruff_old check --cache-dir ./cache repos/cpython/
  Time (mean ± σ):      58.0 ms ±   1.1 ms    [User: 88.5 ms, System: 75.1 ms]
  Range (min … max):    55.8 ms …  60.9 ms    51 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ./bin/ruff_new check --cache-dir ./cache repos/cpython/
  Time (mean ± σ):      86.3 ms ±   2.6 ms    [User: 89.1 ms, System: 56.1 ms]
  Range (min … max):    80.5 ms …  95.1 ms    35 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: ./bin/ruff_parallel check --cache-dir ./cache repos/cpython/
  Time (mean ± σ):      64.5 ms ±   1.2 ms    [User: 105.2 ms, System: 58.8 ms]
  Range (min … max):    62.7 ms …  67.2 ms    47 runs

  Warning: Ignoring non-zero exit code.

Summary
  ./bin/ruff_old check --cache-dir ./cache repos/cpython/ ran
    1.11 ± 0.03 times faster than ./bin/ruff_parallel check --cache-dir ./cache repos/cpython/
    1.49 ± 0.05 times faster than ./bin/ruff_new check --cache-dir ./cache repos/cpython/
```

FastAPI:

```
Benchmark 1: ./bin/ruff_old check --cache-dir ./cache repos/fastapi/
  Time (mean ± σ):      17.2 ms ±   0.6 ms    [User: 18.6 ms, System: 32.6 ms]
  Range (min … max):    15.3 ms …  19.5 ms    169 runs

Benchmark 2: ./bin/ruff_new check --cache-dir ./cache repos/fastapi/
  Time (mean ± σ):      26.6 ms ±   0.7 ms    [User: 16.6 ms, System: 30.0 ms]
  Range (min … max):    24.4 ms …  28.1 ms    113 runs

Benchmark 3: ./bin/ruff_parallel check --cache-dir ./cache repos/fastapi/
  Time (mean ± σ):      17.6 ms ±   0.6 ms    [User: 19.5 ms, System: 35.5 ms]
  Range (min … max):    16.1 ms …  18.9 ms    173 runs

Summary
  ./bin/ruff_old check --cache-dir ./cache repos/fastapi/ ran
    1.03 ± 0.05 times faster than ./bin/ruff_parallel check --cache-dir ./cache repos/fastapi/
    1.55 ± 0.07 times faster than ./bin/ruff_new check --cache-dir ./cache repos/fastapi/
```

Jupyter server:

```
Benchmark 1: ./bin/ruff_old check --cache-dir ./cache repos/jupyter_server/
  Time (mean ± σ):      17.5 ms ±   0.5 ms    [User: 12.2 ms, System: 23.5 ms]
  Range (min … max):    16.1 ms …  18.9 ms    179 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ./bin/ruff_new check --cache-dir ./cache repos/jupyter_server/
  Time (mean ± σ):      18.9 ms ±   0.5 ms    [User: 11.8 ms, System: 21.0 ms]
  Range (min … max):    17.4 ms …  20.4 ms    160 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: ./bin/ruff_parallel check --cache-dir ./cache repos/jupyter_server/
  Time (mean ± σ):      18.1 ms ±   0.5 ms    [User: 14.3 ms, System: 25.9 ms]
  Range (min … max):    16.8 ms …  19.3 ms    163 runs

  Warning: Ignoring non-zero exit code.

Summary
  ./bin/ruff_old check --cache-dir ./cache repos/jupyter_server/ ran
    1.03 ± 0.04 times faster than ./bin/ruff_parallel check --cache-dir ./cache repos/jupyter_server/
    1.08 ± 0.04 times faster than ./bin/ruff_new check --cache-dir ./cache repos/jupyter_server/
```




# Cache Size

## Test script

```bash
#!/bin/env bash

set -eu

cache_dir="./cache"
path="$1"

echo "$path"
for cmd in ruff_old ruff_new ruff_parallel; do
	rm -rf "$cache_dir"
	./bin/$cmd check --cache-dir "$cache_dir" "$path" > /dev/null 2> /dev/null || true
	echo -n "$cmd "
	du -ch "$cache_dir" | tail -n 1
done
```

## Results

Bokeh:

```
ruff_old 5.3M   total
ruff_new 3.3M   total
ruff_parallel 3.3M      total
```

CPython:

```
ruff_old 32M    total
ruff_new 28M    total
ruff_parallel 28M       total
```

FastAPI:

```
ruff_old 4.5M   total
ruff_new 2.3M   total
ruff_parallel 2.3M      total
```

Jupyter server:

```
ruff_old 1.1M   total
ruff_new 476K   total
ruff_parallel 476K      total
```

## Conclusion

The runtime results without a prepared are very close, all within 10% of each other which is 1-15ms. Giving the amount files being read and written this is within the noise level. The same is true for the tests with a fully prepared cache. I'm really interested in running these tests on a slower drive, maybe SATA SSD or even an HHD, there the reduce writing of the cache might have a larger impact.

As for the cache size it's clear that the new design is smaller. Saving as much as 4MB for the CPython case.

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/cache.rs`:37 on 2023-06-16 12:58_

One other consideration here: is it actually a requirement that we store the file contents in the cache? We're making the assumption that the file hasn't changed, so couldn't we just read these back from disk to report the diagnostics?

---

_@charliermarsh reviewed on 2023-06-16 12:58_

---

_Comment by @charliermarsh on 2023-06-19 03:19_

> As for the cache size it's clear that the new design is smaller. Saving as much as 4MB for the CPython case.

As I mentioned in our weekly goal setting: apart from the cache size itself decreasing, it'd also be great to find a way to solve the "ever-increasing cache size" problem. How can we avoid the cache size increasingly monotonically over time?

---

_@MichaReiser reviewed on 2023-06-19 07:48_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:37 on 2023-06-19 07:48_

I considered that during my row/column refactor. One challenge I ran into (and why I deferred it) is that we perform transformations for jupyter notebooks. We would need to replicate that behavior. 

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:234 on 2023-06-19 07:56_

Just as a note (future improvement) because your project level caching now depends on it. Our `CacheKeyHasher` doesn't use a stable hash implementation. Meaning, the cache key could change between rust versions. We should replace the hasher with a stable (and ideally, platform independent) alternative.

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:49 on 2023-06-19 07:58_

Nit: Replace the comment with a custom assertion message. Using a custom assertion message provides additional context to developers seeing the error message and can serve as documentation as well.

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:52 on 2023-06-19 08:00_

What's your reasoning to depend on `itoa` for formatting a single `u64`? I assume that the performance overhead should be neglectable.

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:159 on 2023-06-19 08:05_

My long-term goal for `Message`'s is that they can include code frames pointing to other files. 

For example: You import a private symbol from another file. Ideally, we would create two to three code frames:

1. Code frame showing the import
2. Code frame in the imported file that defines the symbol 
3. (optional) Code frame setting the `__all__` symbol, showing that it does *not* include your import

The challenge with this is that a `Message` now needs access to more than one file. That's why the old implementation used a `Vec` to store the source files. 

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:183 on 2023-06-19 08:06_

I used custom serializers because cloning `kind` can be expensive (it contains up to three Strings). But it seems that this isn't impacting performance much?

---

_@MichaReiser reviewed on 2023-06-19 08:09_

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/cache.rs`:234 on 2023-06-19 09:07_

Make sense.

I'm not really changing the way we calculate the file name, it's just based on a per package basis instead of per source file, so I think the current implementation has the same issue.

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/cache.rs`:52 on 2023-06-19 09:09_

True. `itoa` is already a dependency (via serde-json) so I figured why not avoid the allocation.

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/cache.rs`:183 on 2023-06-19 09:13_

Because the cache is now shared between threads (also see https://github.com/astral-sh/ruff/pull/5120) we really need ownership of the values, dealing with the lifetime is quite painful. We could try using `Arc` or similar, but indeed cloning doesn't seem to impact performance that much.

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/cache.rs`:159 on 2023-06-19 09:23_

We can still support that I think. We can include a new `code_frames` vector field in `CacheMessage`, which holds a reference to the file and byte range. Maybe that would replace `range`, maybe not, not sure at this time.

---

_@Thomasdezeeuw reviewed on 2023-06-19 09:23_

---

_Comment by @Thomasdezeeuw on 2023-06-19 14:12_

I've updated the benchmarks in https://github.com/astral-sh/ruff/pull/5117#issuecomment-1594408844 and they now all with 10% of the old implementation (15ms max), while still saving quite a bit of disk space. 

---

_Comment by @Thomasdezeeuw on 2023-06-19 14:23_

Resolved problems mentioned in the reviews:
* [ ] The cache still grounds without bounds. (https://github.com/astral-sh/ruff/pull/5117#discussion_r1231579363 and https://github.com/astral-sh/ruff/pull/5117#issuecomment-1596434467)
* [ ] Do we need to include the entire source file? (https://github.com/astral-sh/ruff/pull/5117#discussion_r1232220820)
* [ ] Make `CacheKeyHasher` key output stable. (https://github.com/astral-sh/ruff/pull/5117#discussion_r1233669140)
* [ ] Support multiple code frames in a single message. (https://github.com/astral-sh/ruff/pull/5117#discussion_r1233679170)

---

_@MichaReiser reviewed on 2023-06-19 14:29_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:173 on 2023-06-19 14:29_

We should add at least an assertion (release build) that asserts that the source files of all messages are identical. I don't know if there are any diagnostics today that make use of the multi-file diagnostic source frames (I assume not) @charliermarsh ?

---

_@MichaReiser reviewed on 2023-06-19 14:32_

Would it be possible to add some tests for the caching or is this challenging because of the file system dependency?

---

_@MichaReiser reviewed on 2023-06-19 14:43_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:183 on 2023-06-19 14:43_

I guess one way to solve this would be to store a serialized message internally (so that it is only necessary to truncate all the file cache entries when serializing the overall cache) and deserialize the file cache when retrieving the content for a file. This would have the benefit that the cache remains lazy for most parts. But I can see how this complicates things and may not be worth the effort.

---

_Comment by @Thomasdezeeuw on 2023-06-19 14:45_

> Would it be possible to add some tests for the caching or is this challenging because of the file system dependency?

I have tests, but they are based on https://github.com/astral-sh/ruff/pull/5120, see https://github.com/astral-sh/ruff/tree/thomas/cache_testing.

---

_@Thomasdezeeuw reviewed on 2023-06-19 14:46_

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/cache.rs`:173 on 2023-06-19 14:46_

> We should add at least an assertion (release build) that asserts that the source files of all messages are identical.

Can add :+1: 

> I don't know if there are any diagnostics today that make use of the multi-file diagnostic source frames (I assume not) @charliermarsh ?

Last week @charliermarsh said not yet

---

_@MichaReiser approved on 2023-06-19 15:27_

LGTM. Feel free to merge once you added the assertion and I see you added tests in another PR.

---

_Merged by @Thomasdezeeuw on 2023-06-19 15:46_

---

_Closed by @Thomasdezeeuw on 2023-06-19 15:46_

---

_Branch deleted on 2023-06-19 15:46_

---

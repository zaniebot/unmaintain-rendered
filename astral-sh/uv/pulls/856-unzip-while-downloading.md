```yaml
number: 856
title: Unzip while downloading
type: pull_request
state: merged
author: bojanserafimov
labels:
  - performance
assignees: []
merged: true
base: main
head: bojan/unzip-async
created_at: 2024-01-09T18:45:43Z
updated_at: 2024-01-11T14:41:48Z
url: https://github.com/astral-sh/uv/pull/856
synced_at: 2026-01-10T15:39:02Z
```

# Unzip while downloading

---

_Pull request opened by @bojanserafimov on 2024-01-09 18:45_

Results:

```
torch==1.12.1:  1.13 ± 0.03 times faster
trio.txt:       1.10 ± 0.11 times faster
flyte.txt:      1.07 ± 0.02 times faster
boto3.txt:      1.09 ± 0.11 times faster
black.txt:      1.05 ± 0.24 times faster
boto3==1.34.14: 1.04 ± 0.16 times faster
pdm2193.txt:    1.03 ± 0.08 times faster
scispacy.txt:   1.01 ± 0.08 times faster
dtlssocket.txt: 1.00 ± 0.02 times faster
```

---

_Review requested from @zanieb by @bojanserafimov on 2024-01-09 19:03_

---

_Review requested from @charliermarsh by @bojanserafimov on 2024-01-09 19:03_

---

_Comment by @bojanserafimov on 2024-01-09 19:46_

Update: I just added the following line to the cargo.toml of my ad-hoc script pasted above: `flate2 = { version = "1.0.17", features = ["zlib-ng"], default-features = false }` and runtime dropped to 18s, from 27s!!! I will try the same in this branch and see what happens.

---

_Comment by @bojanserafimov on 2024-01-09 20:00_

Initial results on flyte.txt:

```
| branch            | as is | with zlib-ng |
|-------------------+-------+--------------|
| main              | 55s   | 45s          |
| bojan/unzip-async | 57s   | 42s          |
```

I'll focus on evaluating main [with](https://github.com/astral-sh/puffin/pull/859) zlib-ng vs main without it, and come back to this branch later.


---

_Comment by @bojanserafimov on 2024-01-09 21:45_

Results after merging from main:

```
scripts/bench/requirements.txt
main:  Downloaded 45 packages in 428ms
async: Downloaded 45 packages in 501ms

black.txt
main:  Downloaded 8 packages in 81ms
async: Downloaded 8 packages in 87ms

boto3.txt
main:  Downloaded 31 packages in 1.26s
async: Downloaded 31 packages in 1.16s

dtlssocket.txt
main:  Downloaded 2 packages in 4.94s
async: Downloaded 2 packages in 4.97s

flyte.txt
main:  Downloaded 107 packages in 45.64s
async: Downloaded 107 packages in 38.20s

pdm-2913.txt
main:  Downloaded 13 packages in 1.08s
async: Downloaded 13 packages in 1.15s

scispacy.txt
main:  Downloaded 84 packages in 6.15s
async: Downloaded 84 packages in 5.82s

trio.txt
main:  Downloaded 38 packages in 703ms
async: Downloaded 38 packages in 813ms

only boto3
main:  Downloaded 1 package in 45ms
async: Downloaded 1 package in 38ms

only torch
main:  Downloaded 1 package in 29.69s
async: Downloaded 1 package in 26.46s
```

---

_Comment by @konstin on 2024-01-09 22:26_

What command(s) did you use to get those numbers?

---

_Comment by @bojanserafimov on 2024-01-09 23:14_

> What command(s) did you use to get those numbers?

```
cargo run -p puffin-cli -- venv \
      && rm -rf debug/tmp \
      && cargo run -p puffin-cli -- pip-sync \
         --cache-dir debug/tmp \
         ./debug/compiled/pdm_2193.txt
```

`./debug/compiled/pdm_2193.txt` is compiled from `./scripts/requirements/pdm_2193.in` ahead of time.

Btw I'm using `./debug` because it's a gitignored directory :) I'm sure there's a better way. Maybe there's a better way to disable the cache too

---

_Comment by @konstin on 2024-01-10 00:06_

> Btw I'm using ./debug because it's a gitignored directory :) I'm sure there's a better way. 

I think we should check the resolved requirements in too so we get stable benchmarks, @charliermarsh what do you think?

> I'm sure there's a better way. Maybe there's a better way to disable the cache too

`--no-cache` :)

Did you use `--release`/`--profile profiling` for the benchmarks?

---

_Comment by @bojanserafimov on 2024-01-10 00:44_

> Did you use --release

You're not gonna believe this :S I forgot to use release (ty for checking!), so here's an update batch of results with release:

```
scripts/bench/requirements.txt
main:  Downloaded 45 packages in 337ms
async: Downloaded 45 packages in 343ms

black.txt
main:  Downloaded 8 packages in 71ms
async: Downloaded 8 packages in 57ms

boto3.txt
main:  Downloaded 31 packages in 1.13s
async: Downloaded 31 packages in 1.01s

dtlssocket.txt
main:  Downloaded 2 packages in 4.87s
async: Downloaded 2 packages in 4.68s

flyte.txt
main:  Downloaded 107 packages in 40.29s
async: Downloaded 107 packages in 37.67s

pdm-2913.txt
main:  Downloaded 13 packages in 983ms
async: Downloaded 13 packages in 856ms

scispacy.txt
main:  Downloaded 84 packages in 6.60s
async: Downloaded 84 packages in 6.01s

trio.txt
main:  Downloaded 38 packages in 685ms
async: Downloaded 38 packages in 658ms

only boto3
main:  Downloaded 1 package in 28ms
async: Downloaded 1 package in 30ms

only torch
main:  Downloaded 1 package in 23.37s
async: Downloaded 1 package in 21.27s
```

Summary:
- biggest improvement is 20% on black.txt
- small regression on "only boto3"

This makes sense. In theory, the only case when this should be slower than main is when there's more network throughput than we have unzipping troughput on 1 thread. Worth checking if that's the case for "only boto3"

If that's a real scenario, then we can combine the two approaches - add a separate download task that writes to a buffer, and if it gets too far ahead, stop the async unzipping and process the rest of the files in rayon. But that's complicated, hopefully there's no need for it, or there's another workaround.

Anyway, next steps:
1. I should re-test [this](https://github.com/astral-sh/puffin/pull/859) one since I was probably running a debug build there too
2. It seems worth investing a bit in test setup to make it less error prone. I'll integrate my workflow into the bench script and get statistically more significant data too. Once we have a script it would be easier to test on different machines so we're not making a decision based on my hardware only
3. We evaluate if "unzipping slower than downloading" is a real scenario to worry about

With that said, I'm only here until friday and if yall want me to do things in different order (or do something else) let me know :)

> I think we should check the resolved requirements in too so we get stable benchmarks

Seems like a good idea


---

_Comment by @bojanserafimov on 2024-01-10 01:34_

Testing all 4 versions ...
```
main:    main without zlib-ng
main-ng: main with zlib-ng
async:   this PR without zlib-ng
asy-ng:  this PR with zlib-ng
```

I'm only listing tests that run a long time because the short ones are noisy if not repeated.

```
boto3.txt
main:    Downloaded 31 packages in 1.12s
main-ng: Downloaded 31 packages in 1.13s
async:   Downloaded 31 packages in 1.01s
asy-ng:  Downloaded 31 packages in 1.04s

dtlssocket.txt
main:    Downloaded 2 packages in 4.63s
main-ng: Downloaded 2 packages in 4.87s
async:   Downloaded 2 packages in 4.68s
asy-ng:  Downloaded 2 packages in 4.61s

flyte.txt
main:    Downloaded 107 packages in 42.23s
main-ng: Downloaded 107 packages in 40.29s
async:   Downloaded 107 packages in 37.67s
asy-ng:  Downloaded 107 packages in 37.66s

pdm-2913.txt
main:    Downloaded 13 packages in 953ms
main-ng: Downloaded 13 packages in 983ms
async:   Downloaded 13 packages in 856ms
asy-ng:  Downloaded 13 packages in 852ms

scispacy.txt
main:    Downloaded 84 packages in 5.96s
main-ng: Downloaded 84 packages in 6.60s
async:   Downloaded 84 packages in 6.01s
asy-ng:  Downloaded 84 packages in 6.00s

only torch
main:    Downloaded 1 package in 23.65s
main-ng: Downloaded 1 package in 23.37s
async:   Downloaded 1 package in 21.27s
asy-ng:  Downloaded 1 package in 22.69s
```

So this PR is 0-10% improvement (on my machine, with low statistical confidence). The zlib-ng PR is noise, and maybe worth reverting because it adds a dependency on cmake.


---

_Comment by @charliermarsh on 2024-01-10 01:42_

> You're not gonna believe this :S I forgot to use release (ty for checking!)

Oh no! Well the good news is that this looks like a nice speedup over main? :)

---

_Comment by @konstin on 2024-01-10 10:19_

From the singular value i find it hard to decide if the changes are significant or noise (sorry - my bioinformatics background compels me to be pedantic here). Could you run with hyperfine or a criterion benchmark or some other way that gives us an average and standard error (or a range)? The short benchmarks might pair well with criterion, the longer ones with hyperfine, but would need an extra entrypoint i guess.

---

_Comment by @bojanserafimov on 2024-01-10 14:55_

Some early results (the rest of the tests will take a while to run):

### black.txt

```bash
python -m scripts.bench \
    --puffin-path ./target/release/puffin-main \
    --puffin-path ./target/release/puffin-async \
    --benchmark install-cold \
    --min-runs 100 \
    scripts/requirements/compiled/black.txt

Benchmark 1: ./target/release/puffin-main (install-cold)
  Time (mean ± σ):     132.4 ms ±  24.7 ms    [User: 48.8 ms, System: 55.8 ms]
  Range (min … max):   109.8 ms … 316.7 ms    100 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.

Benchmark 2: ./target/release/puffin-async (install-cold)
  Time (mean ± σ):     126.2 ms ±  16.3 ms    [User: 47.9 ms, System: 45.3 ms]
  Range (min … max):   100.0 ms … 174.9 ms    100 runs

Summary
  ./target/release/puffin-async (install-cold) ran
    1.05 ± 0.24 times faster than ./target/release/puffin-main (install-cold)
```

### boto3.txt

```bash
python -m scripts.bench \
    --puffin-path ./target/release/puffin-main \
    --puffin-path ./target/release/puffin-async \
    --benchmark install-cold \
    --min-runs 100 \
    scripts/requirements/compiled/boto3.txt

Benchmark 1: ./target/release/puffin-main (install-cold)
  Time (mean ± σ):      1.251 s ±  0.092 s    [User: 0.538 s, System: 0.723 s]
  Range (min … max):    1.172 s …  1.733 s    100 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.

Benchmark 2: ./target/release/puffin-async (install-cold)
  Time (mean ± σ):      1.144 s ±  0.076 s    [User: 0.583 s, System: 0.581 s]
  Range (min … max):    1.076 s …  1.532 s    100 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.

Summary
  ./target/release/puffin-async (install-cold) ran
    1.09 ± 0.11 times faster than ./target/release/puffin-main (install-cold)
```

This bench script is pretty nice


---

_Comment by @bojanserafimov on 2024-01-10 17:24_

Finished running all tests. Summary, sorted by biggest to smallest difference:

```
torch==1.12.1:  1.13 ± 0.03 times faster
trio.txt:       1.10 ± 0.11 times faster
flyte.txt:      1.07 ± 0.02 times faster
boto3.txt:      1.09 ± 0.11 times faster
black.txt:      1.05 ± 0.24 times faster
boto3==1.34.14: 1.04 ± 0.16 times faster
pdm2193.txt:    1.03 ± 0.08 times faster
scispacy.txt:   1.01 ± 0.08 times faster
dtlssocket.txt: 1.00 ± 0.02 times faster
```

# Detailed results

### black.txt

```bash
python -m scripts.bench \
    --puffin-path ./target/release/puffin-main \
    --puffin-path ./target/release/puffin-async \
    --benchmark install-cold \
    --min-runs 100 \
    scripts/requirements/compiled/black.txt

Benchmark 1: ./target/release/puffin-main (install-cold)
  Time (mean ± σ):     132.4 ms ±  24.7 ms    [User: 48.8 ms, System: 55.8 ms]
  Range (min … max):   109.8 ms … 316.7 ms    100 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.

Benchmark 2: ./target/release/puffin-async (install-cold)
  Time (mean ± σ):     126.2 ms ±  16.3 ms    [User: 47.9 ms, System: 45.3 ms]
  Range (min … max):   100.0 ms … 174.9 ms    100 runs

Summary
  ./target/release/puffin-async (install-cold) ran
    1.05 ± 0.24 times faster than ./target/release/puffin-main (install-cold)
```

### boto3.txt

```bash
python -m scripts.bench \
    --puffin-path ./target/release/puffin-main \
    --puffin-path ./target/release/puffin-async \
    --benchmark install-cold \
    --min-runs 100 \
    scripts/requirements/compiled/boto3.txt

Benchmark 1: ./target/release/puffin-main (install-cold)
  Time (mean ± σ):      1.251 s ±  0.092 s    [User: 0.538 s, System: 0.723 s]
  Range (min … max):    1.172 s …  1.733 s    100 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.

Benchmark 2: ./target/release/puffin-async (install-cold)
  Time (mean ± σ):      1.144 s ±  0.076 s    [User: 0.583 s, System: 0.581 s]
  Range (min … max):    1.076 s …  1.532 s    100 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.

Summary
  ./target/release/puffin-async (install-cold) ran
    1.09 ± 0.11 times faster than ./target/release/puffin-main (install-cold)
```

### dtlssocket.txt

```bash
python -m scripts.bench \
    --puffin-path ./target/release/puffin-main \
    --puffin-path ./target/release/puffin-async \
    --benchmark install-cold \
    --min-runs 100 \
    scripts/requirements/compiled/dtlssocket.txt

Benchmark 1: ./target/release/puffin-main (install-cold)
  Time (mean ± σ):      4.524 s ±  0.061 s    [User: 3.620 s, System: 0.924 s]
  Range (min … max):    4.439 s …  4.793 s    100 runs

Benchmark 2: ./target/release/puffin-async (install-cold)
  Time (mean ± σ):      4.528 s ±  0.058 s    [User: 3.631 s, System: 0.896 s]
  Range (min … max):    4.438 s …  4.780 s    100 runs

Summary
  ./target/release/puffin-main (install-cold) ran
    1.00 ± 0.02 times faster than ./target/release/puffin-async (install-cold)
```

### flyte.txt

```bash
python -m scripts.bench \
    --puffin-path ./target/release/puffin-main \
    --puffin-path ./target/release/puffin-async \
    --benchmark install-cold \
    --min-runs 10 \
    scripts/requirements/compiled/flyte.txt
    
Benchmark 1: ./target/release/puffin-main (install-cold)
  Time (mean ± σ):     41.163 s ±  0.453 s    [User: 11.127 s, System: 12.223 s]
  Range (min … max):   40.576 s … 41.852 s    10 runs

Benchmark 2: ./target/release/puffin-async (install-cold)
  Time (mean ± σ):     38.649 s ±  0.622 s    [User: 16.275 s, System: 11.752 s]
  Range (min … max):   37.950 s … 39.588 s    10 runs

Summary
  ./target/release/puffin-async (install-cold) ran
    1.07 ± 0.02 times faster than ./target/release/puffin-main (install-cold)
```


### pdm2193.txt

```bash
python -m scripts.bench \
    --puffin-path ./target/release/puffin-main \
    --puffin-path ./target/release/puffin-async \
    --benchmark install-cold \
    --min-runs 100 \
    scripts/requirements/compiled/pdm_2193.txt

Benchmark 1: ./target/release/puffin-main (install-cold)
  Time (mean ± σ):      1.033 s ±  0.053 s    [User: 0.632 s, System: 0.778 s]
  Range (min … max):    0.979 s …  1.265 s    100 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.

Benchmark 2: ./target/release/puffin-async (install-cold)
  Time (mean ± σ):      1.000 s ±  0.063 s    [User: 0.613 s, System: 0.578 s]
  Range (min … max):    0.942 s …  1.220 s    100 runs

Summary
  ./target/release/puffin-async (install-cold) ran
    1.03 ± 0.08 times faster than ./target/release/puffin-main (install-cold)
```

### scispacy.txt

```bash
python -m scripts.bench \
    --puffin-path ./target/release/puffin-main \
    --puffin-path ./target/release/puffin-async \
    --benchmark install-cold \
    --min-runs 100 \
    scripts/requirements/compiled/pdm_2193.txt
    
Benchmark 1: ./target/release/puffin-main (install-cold)
  Time (mean ± σ):      6.206 s ±  0.411 s    [User: 1.960 s, System: 3.014 s]
  Range (min … max):    6.051 s …  9.314 s    100 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.

Benchmark 2: ./target/release/puffin-async (install-cold)
  Time (mean ± σ):      6.148 s ±  0.254 s    [User: 2.224 s, System: 2.303 s]
  Range (min … max):    6.004 s …  7.747 s    100 runs

  Warning: The first benchmarking run for this command was significantly slower than the rest (7.402 s). This could be caused by (filesystem) caches that were not filled until after the first run. You are already using both the '--warmup' option as well as the '--prepare' option. Consider re-running the benchmark on a quiet system. Maybe it was a random outlier. Alternatively, consider increasing the warmup count.

Summary
  ./target/release/puffin-async (install-cold) ran
    1.01 ± 0.08 times faster than ./target/release/puffin-main (install-cold)
```

### trio.txt

```bash
python -m scripts.bench \
    --puffin-path ./target/release/puffin-main \
    --puffin-path ./target/release/puffin-async \
    --benchmark install-cold \
    --min-runs 100 \
    scripts/requirements/compiled/trio.txt

Benchmark 1: ./target/release/puffin-main (install-cold)
  Time (mean ± σ):     889.4 ms ±  74.7 ms    [User: 380.0 ms, System: 824.7 ms]
  Range (min … max):   809.8 ms … 1282.2 ms    100 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.

Benchmark 2: ./target/release/puffin-async (install-cold)
  Time (mean ± σ):     806.1 ms ±  41.2 ms    [User: 360.4 ms, System: 505.1 ms]
  Range (min … max):   756.9 ms … 988.0 ms    100 runs

Summary
  ./target/release/puffin-async (install-cold) ran
    1.10 ± 0.11 times faster than ./target/release/puffin-main (install-cold)
```

### boto3==1.34.14

```bash
python -m scripts.bench \
    --puffin-path ./target/release/puffin-main \
    --puffin-path ./target/release/puffin-async \
    --benchmark install-cold \
    --min-runs 100 \
    debug/boto3.txt
    
Benchmark 1: ./target/release/puffin-main (install-cold)
  Time (mean ± σ):     101.0 ms ±  11.4 ms    [User: 33.5 ms, System: 28.4 ms]
  Range (min … max):    82.6 ms … 144.3 ms    100 runs

Benchmark 2: ./target/release/puffin-async (install-cold)
  Time (mean ± σ):     105.4 ms ±  11.2 ms    [User: 30.0 ms, System: 27.6 ms]
  Range (min … max):    85.4 ms … 141.7 ms    100 runs

Summary
  ./target/release/puffin-main (install-cold) ran
    1.04 ± 0.16 times faster than ./target/release/puffin-async (install-cold)   
```

### torch==1.12.1

```bash
python -m scripts.bench \
    --puffin-path ./target/release/puffin-main \
    --puffin-path ./target/release/puffin-async \
    --benchmark install-cold \
    --min-runs 20 \
    debug/torch.txt
    
Benchmark 1: ./target/release/puffin-main (install-cold)
  Time (mean ± σ):     23.736 s ±  0.347 s    [User: 5.555 s, System: 5.496 s]
  Range (min … max):   23.483 s … 24.739 s    20 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.

Benchmark 2: ./target/release/puffin-async (install-cold)
  Time (mean ± σ):     20.996 s ±  0.413 s    [User: 8.371 s, System: 5.076 s]
  Range (min … max):   20.695 s … 22.545 s    20 runs

Summary
  ./target/release/puffin-async (install-cold) ran
    1.13 ± 0.03 times faster than ./target/release/puffin-main (install-cold)
```

---

_Comment by @charliermarsh on 2024-01-10 18:08_

Do you think this is worth productionizing based on the above benchmarks? I only see one regression (`boto3==1.34.14`), but otherwise seems like a clear win in those cases. I wonder if there's any sense in doing this only for larger zips, and otherwise fetching into memory and then unzipping to disk?

---

_Comment by @bojanserafimov on 2024-01-10 19:15_

The old solution would be faster if the wheel is a [zip bomb](https://en.wikipedia.org/wiki/Zip_bomb). It's possible that there exist very compressible packages that take more time to decompress than to download. In that case the "best of both worlds" solution would be to have separate downloader and unzipper tasks and a buffer between them that can grow and spill onto disk if necessary. If the downloader gets too far ahead and the unzipper is not keeping up, we stop the zipper, and unzip the remaining files using rayon. Though this is a complicated solution and I wouldn't code it without evidence that it matters.

IMO right now the more important thing is testing infrastructure:
1. More requirements.txt files
2. Tests with emulated bad network
3. Tests on different machines

Before that infrastructure exists we shouldn't get too attached to any particular implementation.

The +10% speedup from this PR is not huge, but productionizing isn't difficult so I'll finish it up. I'll leave the current unzipper there so that we can switch to it if needed, and benchmark again with better test infra in the future.

---

_Review comment by @BurntSushi on `crates/puffin-distribution/src/distribution_database.rs`:159 on 2024-01-10 20:51_

What is the thinking behind leaving the dead code below? Is it so the code can be changed back to the old strategy easily? I think I'd prefer just deleting the dead code. It can always be revised from source history if necessary.

An alternative would be to make a runtime option to toggle between the two strategies if we wanted to keep both around, but I'm not sure how valuable that is.

---

_Review comment by @BurntSushi on `crates/puffin-distribution/src/distribution_database.rs`:143 on 2024-01-10 21:06_

I think this should be `.await?`?

---

_@BurntSushi approved on 2024-01-10 21:06_

Nice work! Great patch.

---

_@bojanserafimov reviewed on 2024-01-10 21:16_

---

_Review comment by @bojanserafimov on `crates/puffin-distribution/src/distribution_database.rs`:159 on 2024-01-10 21:16_

There's a good chance we'd want this code just because we'll get more tests and it would make sense to compare both versions. Current tests are done on my machine only.

We could dig out this code from history, but there will be merge conflicts with changes that pile up. 

If we want to delete this now I'd still do it in a separate "refactor only" PR. There would be too much noise in this PR if I do it now.

---

_@bojanserafimov reviewed on 2024-01-10 21:32_

---

_Review comment by @bojanserafimov on `crates/puffin-distribution/src/distribution_database.rs`:159 on 2024-01-10 21:32_

Lmk what you prefer. I can send a PR deleting this on top of this one. There will be a cascade of related deletes, which is why I think bringing it back will involve merge conflicts

---

_Marked ready for review by @bojanserafimov on 2024-01-10 21:36_

---

_@charliermarsh reviewed on 2024-01-10 21:46_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/distribution_database.rs`:208 on 2024-01-10 21:46_

Oh hmm... I would say they should both use `.stem()`, yeah. Can you try this in a separate PR?

---

_@charliermarsh reviewed on 2024-01-10 21:53_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/distribution_database.rs`:159 on 2024-01-10 21:53_

IMO best practice would be to _either_: (1) remove, since we can always restore from source control; or (2) retain, but thread through a CLI flag (could be user-hidden) to let us toggle, and ensure there's test coverage. Otherwise, it's very likely to grow stale while still requiring some amount of maintenance, which is kind of the worst of both worlds.

---

_Review comment by @bojanserafimov on `crates/puffin-distribution/src/distribution_database.rs`:159 on 2024-01-10 23:05_

Ok I'll remove it

---

_@bojanserafimov reviewed on 2024-01-10 23:05_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/distribution_database.rs`:159 on 2024-01-10 23:33_

Sounds good, you can do it in a separate PR with this branch marked as upstream if you like.

---

_@charliermarsh reviewed on 2024-01-10 23:33_

---

_@bojanserafimov reviewed on 2024-01-11 00:10_

---

_Review comment by @bojanserafimov on `crates/puffin-distribution/src/distribution_database.rs`:208 on 2024-01-11 00:10_

This code will be deleted

---

_@charliermarsh approved on 2024-01-11 00:45_

Awesome. I ran a few of these locally and the differences held up on my machine too -- I actually saw a 14% improvement for `boto3.txt` and a 9% improvement for `trio.txt`!

---

_Comment by @charliermarsh on 2024-01-11 00:46_

@bojanserafimov - I recommend merging this, then rebasing #879 on `main`, then merging #879 into `main`, so that each PR shows up as a single commit on `main`.

---

_Label `performance` added by @charliermarsh on 2024-01-11 00:46_

---

_@charliermarsh reviewed on 2024-01-11 00:49_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/distribution_database.rs`:145 on 2024-01-11 00:49_

I think we should unzip into a temporary directory, then rename the directory to `target`, to avoid leaving incomplete directories in the cache.

---

_@charliermarsh reviewed on 2024-01-11 00:50_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/distribution_database.rs`:113 on 2024-01-11 00:50_

Should we be applying this same logic to the next branch in the match, which deals with direct URL wheels, like `werkzeug @ https://files.pythonhosted.org/packages/ff/1d/960bb4017c68674a1cb099534840f18d3def3ce44aed12b5ed8b78e0153e/Werkzeug-2.0.0-py3-none-any.whl`?

If so, we can probably then remove the entire `Unzip` trait.

---

_Comment by @charliermarsh on 2024-01-11 14:26_

LGTM! @bojanserafimov - LMK if you want an additional follow-up review, otherwise feel free to merge.

---

_Merged by @bojanserafimov on 2024-01-11 14:41_

---

_Closed by @bojanserafimov on 2024-01-11 14:41_

---

_Branch deleted on 2024-01-11 14:41_

---

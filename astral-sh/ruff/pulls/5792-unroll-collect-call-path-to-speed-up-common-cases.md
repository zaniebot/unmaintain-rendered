```yaml
number: 5792
title: "Unroll `collect_call_path` to speed up common cases"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/unroll
created_at: 2023-07-16T00:49:23Z
updated_at: 2023-07-18T15:30:08Z
url: https://github.com/astral-sh/ruff/pull/5792
synced_at: 2026-01-12T03:30:21Z
```

# Unroll `collect_call_path` to speed up common cases

---

_Pull request opened by @charliermarsh on 2023-07-16 00:49_

## Summary

This PR just naively unrolls `collect_call_path` to handle attribute resolutions of up to eight segments. In profiling via Instruments, it seems to be about 4x faster for a very hot code path (4% of total execution time on `main`, 1% here).

Profiling by running `RAYON_NUM_THREADS=1 cargo instruments -t time --profile release-debug --time-limit 10000 -p ruff_cli -o FromSlice.trace -- check crates/ruff/resources/test/cpython --silent -e --no-cache --select ALL`, and modifying the linter to loop infinitely up to the specified time (10 seconds) to increase sample size.

Before:

<img width="1792" alt="Screen Shot 2023-07-15 at 5 13 34 PM" src="https://github.com/astral-sh/ruff/assets/1309177/4a8b0b45-8b67-43e9-af5e-65b326928a8e">

After:

<img width="1792" alt="Screen Shot 2023-07-15 at 8 38 51 PM" src="https://github.com/astral-sh/ruff/assets/1309177/d8829159-2c79-4a49-ab3c-9e4e86f5b2b1">




---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-16 00:49_

---

_Review requested from @konstin by @charliermarsh on 2023-07-16 00:49_

---

_@charliermarsh reviewed on 2023-07-16 00:49_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/physical_lines.rs`:118 on 2023-07-16 00:49_

Empty `else`, not related.

---

_Comment by @github-actions[bot] on 2023-07-16 01:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.02ms     4.2 MB/sec    1.00      9.8±0.03ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1886.4±1.80µs     8.8 MB/sec    1.00  1888.0±12.67µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    205.5±1.79µs    14.4 MB/sec    1.00    205.3±0.37µs    14.4 MB/sec
formatter/pydantic/types.py                1.01      4.2±0.01ms     6.0 MB/sec    1.00      4.2±0.02ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.03     13.8±0.07ms     3.0 MB/sec    1.00     13.3±0.11ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.5±0.03ms     4.8 MB/sec    1.00      3.4±0.02ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    373.7±1.20µs     7.9 MB/sec    1.00    369.4±2.89µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.1±0.01ms     4.1 MB/sec    1.00      6.0±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.01ms     5.8 MB/sec    1.00      6.9±0.01ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1413.3±2.96µs    11.8 MB/sec    1.00   1415.5±1.69µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    147.4±0.29µs    20.0 MB/sec    1.00    147.6±0.67µs    20.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     14.4±0.61ms     2.8 MB/sec    1.27     18.3±0.58ms     2.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.9±0.17ms     5.8 MB/sec    1.17      3.3±0.13ms     5.0 MB/sec
formatter/numpy/globals.py                 1.00   311.8±14.48µs     9.5 MB/sec    1.14   354.2±17.15µs     8.3 MB/sec
formatter/pydantic/types.py                1.00      6.5±0.44ms     3.9 MB/sec    1.13      7.3±0.26ms     3.5 MB/sec
linter/all-rules/large/dataset.py          1.00     20.3±0.72ms     2.0 MB/sec    1.00     20.4±0.62ms  2044.0 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.21ms     3.2 MB/sec    1.02      5.3±0.24ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   625.4±27.38µs     4.7 MB/sec    1.02   640.1±32.57µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.0±0.35ms     2.8 MB/sec    1.03      9.3±0.47ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.00     10.4±0.44ms     3.9 MB/sec    1.07     11.1±0.33ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.11ms     7.7 MB/sec    1.02      2.2±0.08ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   263.3±11.99µs    11.2 MB/sec    1.01   266.7±15.57µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.14ms     5.5 MB/sec    1.00      4.7±0.21ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @sbrugman on 2023-07-16 19:19_

> and modifying the linter to loop infinitely

@charliermarsh Could you please share the exact diff? Was inspired by this PR to experiment with some of the benchmarking in https://github.com/astral-sh/ruff/pull/5811, but couldn't reproduce this setup (it ended up giving irrelevant profiles) so ended up with running over CPython

---

_Comment by @konstin on 2023-07-18 13:58_

@sbrugman You can get the same result with `cargo run --bin ruff_dev -- repeat --no-cache --repeat 10000 crates/ruff/resources/test/cpython` (it's `ruff check` with an additional `--repeat` option)

---

_Comment by @charliermarsh on 2023-07-18 14:00_

Oh that seems a lot better than what I did. I will post the diff regardless in a bit.

---

_Comment by @konstin on 2023-07-18 14:53_

The return type of `collect_call_path` also seems like an opportunity for optimization: We generally either want to check against a specific length (e.g. 2) and don't need to save anything otherwise or iterate over the results (e.g. the widely used `resolve_call_path`), where we only want to iterate over elements from left to write (we could do this with an Iterator that is implemented recursively, but i'm not sure if the extra function calls aren't making this slower)

---

_@konstin approved on 2023-07-18 14:54_

That's a great speedup

---

_Comment by @charliermarsh on 2023-07-18 15:29_

> The return type of collect_call_path also seems like an opportunity for optimization...

Agreed.

---

_Merged by @charliermarsh on 2023-07-18 15:30_

---

_Closed by @charliermarsh on 2023-07-18 15:30_

---

_Branch deleted on 2023-07-18 15:30_

---

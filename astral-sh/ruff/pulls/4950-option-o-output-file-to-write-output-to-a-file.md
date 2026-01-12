```yaml
number: 4950
title: "Option (`-o`/`--output-file`) to write output to a file"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: cli/output-file-option
created_at: 2023-06-08T04:53:54Z
updated_at: 2023-06-20T20:03:46Z
url: https://github.com/astral-sh/ruff/pull/4950
synced_at: 2026-01-12T15:55:17Z
```

# Option (`-o`/`--output-file`) to write output to a file

---

_@dhruvmanila_

## Summary

A new CLI option (`-o`/`--output-file`) to write output to a file instead of stdout.

Major change is to remove the lock acquired on stdout. The argument is that the output is buffered and thus the lock is acquired only when writing a block (8kb). As per the benchmark below there is a slight performance penalty.

Reference: https://rustmagazine.org/issue-3/javascript-compiler/#printing-is-slow

## Benchmarks

_Output is truncated to only contain useful information:_

Command: `check --isolated --no-cache --select=ALL --show-source ./test-repos/cpython"`

Latest HEAD (361d45f2b2f7e69d37d4e6d8270e448f72cae9a7) with and without the manual lock on stdout:

```console
Benchmark 1: With lock
  Time (mean ± σ):      5.687 s ±  0.075 s    [User: 17.110 s, System: 0.486 s]
  Range (min … max):    5.615 s …  5.860 s    10 runs

Benchmark 2: Without lock
  Time (mean ± σ):      5.719 s ±  0.064 s    [User: 17.095 s, System: 0.491 s]
  Range (min … max):    5.640 s …  5.865 s    10 runs

Summary
  (1) ran 1.01 ± 0.02 times faster than (2)
```

This PR:

```console
Benchmark 1: This PR
  Time (mean ± σ):      5.855 s ±  0.058 s    [User: 17.197 s, System: 0.491 s]
  Range (min … max):    5.786 s …  5.987 s    10 runs
 
Benchmark 2: Latest HEAD with lock
  Time (mean ± σ):      5.645 s ±  0.033 s    [User: 16.922 s, System: 0.495 s]
  Range (min … max):    5.600 s …  5.712 s    10 runs
 
Summary
  (2) ran 1.04 ± 0.01 times faster than (1)
```

## Test Plan

Run all of the commands which gives output with and without the `--output-file=ruff.out` option:
* `--show-settings`
* `--show-files`
* `--show-fixes`
* `--diff`
* `--select=ALL`
* `--select=All --show-source`
* `--watch` (only stdout allowed)

resolves: #4754 


---

_Comment by @github-actions[bot] on 2023-06-08 05:18_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.02ms     6.0 MB/sec    1.03      6.9±0.02ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1369.1±2.92µs    12.2 MB/sec    1.03   1406.0±1.71µs    11.8 MB/sec
formatter/numpy/globals.py                 1.00    133.3±0.29µs    22.1 MB/sec    1.04    138.3±0.43µs    21.3 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.2 MB/sec    1.02      2.8±0.01ms     9.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.9±0.06ms     2.9 MB/sec    1.04     14.4±0.08ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.8 MB/sec    1.03      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    364.0±1.40µs     8.1 MB/sec    1.01    367.0±1.12µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.02ms     4.2 MB/sec    1.01      6.2±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.02ms     5.7 MB/sec    1.04      7.4±0.02ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1486.4±2.80µs    11.2 MB/sec    1.04   1540.7±3.16µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.5±0.21µs    18.5 MB/sec    1.04    165.2±0.12µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     7.9 MB/sec    1.04      3.3±0.02ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04     10.6±0.47ms     3.8 MB/sec    1.00     10.2±0.54ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.05      2.1±0.09ms     7.8 MB/sec    1.00      2.0±0.09ms     8.2 MB/sec
formatter/numpy/globals.py                 1.05   227.7±15.42µs    13.0 MB/sec    1.00   217.8±14.80µs    13.5 MB/sec
formatter/pydantic/types.py                1.02      4.4±0.19ms     5.8 MB/sec    1.00      4.3±0.18ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     20.4±0.81ms  2047.1 KB/sec    1.03     20.9±0.74ms  1990.0 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.5±0.21ms     3.0 MB/sec    1.00      5.5±0.20ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.04   691.0±48.25µs     4.3 MB/sec    1.00   663.1±46.81µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.06      9.6±0.38ms     2.7 MB/sec    1.00      9.1±0.40ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.01     11.2±0.32ms     3.6 MB/sec    1.00     11.1±0.43ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.21ms     7.1 MB/sec    1.00      2.3±0.10ms     7.1 MB/sec
linter/default-rules/numpy/globals.py      1.05   285.8±13.64µs    10.3 MB/sec    1.00   271.0±14.19µs    10.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      5.0±0.53ms     5.1 MB/sec    1.00      4.9±0.15ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `crates/ruff_cli/src/args.rs`:110 on 2023-06-08 09:49_

This should make clear that the output means lint output, not e.g. the fixed code output

---

_@konstin approved on 2023-06-08 09:52_

---

_@dhruvmanila reviewed on 2023-06-08 10:49_

---

_Review comment by @dhruvmanila on `crates/ruff_cli/src/args.rs`:110 on 2023-06-08 10:49_

Can you expand on "fixed code outout"? Are you referring to the updated files on disk?

Here, output comprises of everything that goes to stdout which includes the diagnostics, `--show-*` output, `--diff` output, statistics.

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/lib.rs`:181 on 2023-06-08 11:11_

The output is now locked significantly longer than before. This can lead to unexpected behavior, e.g using `dbg` during this time won't work. 

We also need to be careful to drop this `writer` at the right time (e.g. only when entering the watch). 

What are your thoughts on instead defining our own `enum` that implements `Write` that has a `lock` operation (which is a no-op for files)?

---

_@MichaReiser reviewed on 2023-06-08 11:11_

---

_@konstin reviewed on 2023-06-08 11:31_

---

_Review comment by @konstin on `crates/ruff_cli/src/args.rs`:110 on 2023-06-08 11:31_

Yep, someone could assume `ruff --fix my_file.py -o my_file_fixed.py` would work

---

_Review comment by @dhruvmanila on `crates/ruff_cli/src/lib.rs`:181 on 2023-06-08 18:14_

Good point, I missed that. Defining our own `enum` is a good idea although I'm unable to understand what would the `lock` operation be like. Would it be an additional method implemented on the said `enum`? Acquiring a lock on every write seems unnecessary.

---

_@dhruvmanila reviewed on 2023-06-08 18:14_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/lib.rs`:181 on 2023-06-08 20:18_

The idea would be to have aon object similar to stout that has a lock object that then aquires the lock and releases it on drop. 

I can try to find the source in Rome tomorrow. I believe it used something similar 

---

_@MichaReiser reviewed on 2023-06-08 20:18_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/lib.rs`:181 on 2023-06-14 20:41_

Where did we land on this @dhruvmanila?

---

_@charliermarsh reviewed on 2023-06-14 20:41_

---

_@dhruvmanila reviewed on 2023-06-15 02:36_

---

_Review comment by @dhruvmanila on `crates/ruff_cli/src/lib.rs`:181 on 2023-06-15 02:36_

Sorry for letting this sit, I'll get on it today.

As per our benchmarks the lock is giving us performance benefits so will be moving with Micha's idea.

---

_Review requested from @MichaReiser by @dhruvmanila on 2023-06-19 19:01_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/lib.rs`:86 on 2023-06-20 07:28_

Do I understand the change correctly that you decided to remove the lock (I'm fine with that)? 

I think this solution is overkill without the lock. Instead, we can pass a `&dyn Writer` to `Printer` because `Writer` already exposes all methods we need. Changing Printer to accept a `&dyn Writer` avoids Rust generating two implementations of Printer, one for a file writer and one for the stdout writer (that would result in a lot of duplicated code). I doubt that this creates a significant performance hit.



---

_@MichaReiser reviewed on 2023-06-20 07:28_

---

_@dhruvmanila reviewed on 2023-06-20 10:14_

---

_Review comment by @dhruvmanila on `crates/ruff_cli/src/lib.rs`:86 on 2023-06-20 10:14_

> Do I understand the change correctly that you decided to remove the lock (I'm fine with that)?

Yes.

> I think this solution is overkill without the lock. Instead, we can pass a `&dyn Writer` to `Printer` because `Writer` already exposes all methods we need. Changing Printer to accept a `&dyn Writer` avoids Rust generating two implementations of Printer, one for a file writer and one for the stdout writer (that would result in a lot of duplicated code). I doubt that this creates a significant performance hit.

Yes, that makes sense. I'll update it.

---

_Review requested from @MichaReiser by @dhruvmanila on 2023-06-20 16:12_

---

_@MichaReiser approved on 2023-06-20 16:17_

---

_Comment by @MichaReiser on 2023-06-20 16:18_

Sorry for the back and forth and thanks for working on this.

---

_Comment by @dhruvmanila on 2023-06-20 16:25_

> Sorry for the back and forth and thanks for working on this.

No problem at all! Thanks for being patient and answering all of my questions ;)

---

_Merged by @dhruvmanila on 2023-06-20 16:46_

---

_Closed by @dhruvmanila on 2023-06-20 16:46_

---

_Branch deleted on 2023-06-20 16:47_

---

_Comment by @TalAmuyal on 2023-06-20 20:03_

Thanks!

---

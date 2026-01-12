```yaml
number: 4624
title: "Improve `Message` sorting performance"
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: perf-silent
created_at: 2023-05-24T11:22:09Z
updated_at: 2023-05-24T14:34:49Z
url: https://github.com/astral-sh/ruff/pull/4624
synced_at: 2026-01-12T03:50:03Z
```

# Improve `Message` sorting performance

---

_Pull request opened by @MichaReiser on 2023-05-24 11:22_

## Summary

I included the rule name in the `Message` `Ord` implementation to fix non-deterministic ordering of messages in the CLI output (which messed up my diffs when comparing the diagnostics before/after) in #3931. This regressed performance significantly for two reasons:

1. We need to compare the name
2. more importantly, we now call `AsRule::rule` which performs a lookup of the rule by name (involves plenty of string comparisons). 

This PR fixes the performance regression by:

1. Using `sort()` instead of `sort_unstable()` because we care about stable order. 
2. Remove `rule` from the comparison key because, in my view, a rule should never emit two diagnostics at the exact same location. We can include `kind.name` which is significantly cheaper, but using stable sorting and removing the rule name proved to be fastest
3. Compare the file and add a fast path to `SourceFile` that returns `Ordering::Equal` if the files point to the same address. I'm very surprised that Rust doesn't implement this optimisation by default for Arcs. They have such a fast path for the `PartialEq` implementation. 

Fixes #4606

## Test Plan

The performance is now roughly the same as before #3931, even though we've added new rules since. 

> *Note*: Measured with release builds that include debug information (for profiling). The release builds should be slightly faster. 


### Before #3931(using `sort_unstable`)

<pre><b>Benchmark 1</b>: ./target/release-debug/ruff --no-cache -e  --select=ALL ./crates/ruff/resources/test/cpython/
  Time (<span style="color:#26A269"><b>mean</b></span> ± <span style="color:#26A269">σ</span>):     <span style="color:#26A269"><b>712.1 ms</b></span> ± <span style="color:#26A269"> 19.9 ms</span>    [User: <span style="color:#12488B">7890.2 ms</span>, System: <span style="color:#12488B">222.2 ms</span>]
  Range (<span style="color:#2AA1B3">min</span> … <span style="color:#A347BA">max</span>):   <span style="color:#2AA1B3">688.7 ms</span> … <span style="color:#A347BA">749.0 ms</span>    10 runs
</pre>

### Before #3931 patched using `sort`

<pre><b>Benchmark 1</b>: ./target/release-debug/ruff --no-cache -e  --select=ALL ./crates/ruff/resources/test/cpython/
  Time (<span style="color:#26A269"><b>mean</b></span> ± <span style="color:#26A269">σ</span>):     <span style="color:#26A269"><b>725.6 ms</b></span> ± <span style="color:#26A269"> 17.8 ms</span>    [User: <span style="color:#12488B">7948.2 ms</span>, System: <span style="color:#12488B">227.7 ms</span>]
  Range (<span style="color:#2AA1B3">min</span> … <span style="color:#A347BA">max</span>):   <span style="color:#2AA1B3">708.6 ms</span> … <span style="color:#A347BA">760.5 ms</span>    10 runs
</pre>

### Main

<pre><b>Benchmark 1</b>: ./target/release-debug/ruff --no-cache -e  --select=ALL ./crates/ruff/resources/test/cpython/
  Time (<span style="color:#26A269"><b>mean</b></span> ± <span style="color:#26A269">σ</span>):     <span style="color:#26A269"><b> 1.638 s</b></span> ± <span style="color:#26A269"> 0.024 s</span>    [User: <span style="color:#12488B">8.997 s</span>, System: <span style="color:#12488B">0.238 s</span>]
  Range (<span style="color:#2AA1B3">min</span> … <span style="color:#A347BA">max</span>):   <span style="color:#2AA1B3"> 1.605 s</span> … <span style="color:#A347BA"> 1.676 s</span>    10 runs
</pre>

### Now

<pre><b>Benchmark 1</b>: ./target/release-debug/ruff --no-cache -e  --select=ALL ./crates/ruff/resources/test/cpython/
  Time (<span style="color:#26A269"><b>mean</b></span> ± <span style="color:#26A269">σ</span>):     <span style="color:#26A269"><b>734.9 ms</b></span> ± <span style="color:#26A269"> 10.0 ms</span>    [User: <span style="color:#12488B">8133.6 ms</span>, System: <span style="color:#12488B">211.3 ms</span>]
  Range (<span style="color:#2AA1B3">min</span> … <span style="color:#A347BA">max</span>):   <span style="color:#2AA1B3">720.9 ms</span> … <span style="color:#A347BA">752.5 ms</span>    10 runs
</pre>

## Alternatives
Ideally, we wouldn't even sort the `Message`s when passing `--silent`. I considered moving the logic into the `Printer,` but it only gets passed a read-only slice. We could consider changing the slice to be mutable, but that feels odd. 

---

_Comment by @MichaReiser on 2023-05-24 11:22_

Current dependencies on/for this PR:
* main
  * **PR #4624** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/4624" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/charliermarsh/ruff/4624?utm_source=stack-comment).

---

_Comment by @github-actions[bot] on 2023-05-24 11:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.05ms     2.9 MB/sec    1.00     14.0±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    425.6±0.93µs     6.9 MB/sec    1.00    425.6±1.15µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1502.8±6.62µs    11.1 MB/sec    1.00   1486.9±2.32µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.8±0.35µs    17.4 MB/sec    1.00    169.0±0.22µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.02      5.3±0.02ms     7.7 MB/sec    1.00      5.2±0.01ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1031.4±0.43µs    16.1 MB/sec    1.00   1028.0±0.59µs    16.2 MB/sec
parser/numpy/globals.py                    1.01    105.7±0.49µs    27.9 MB/sec    1.00    105.1±0.28µs    28.1 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.01ms    11.3 MB/sec    1.00      2.2±0.00ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     21.0±0.47ms  1988.4 KB/sec    1.00     20.9±0.34ms  1997.0 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.10ms     3.1 MB/sec    1.00      5.3±0.11ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   640.1±28.67µs     4.6 MB/sec    1.00   637.9±16.18µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.8±0.24ms     2.9 MB/sec    1.00      8.8±0.18ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.1±0.17ms     4.0 MB/sec    1.00     10.1±0.28ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.04ms     7.8 MB/sec    1.01      2.2±0.09ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   262.9±10.04µs    11.2 MB/sec    1.00    264.2±8.21µs    11.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.12ms     5.6 MB/sec    1.01      4.6±0.12ms     5.5 MB/sec
parser/large/dataset.py                    1.00      8.3±0.19ms     4.9 MB/sec    1.00      8.3±0.11ms     4.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1590.2±52.04µs    10.5 MB/sec    1.00  1585.2±29.25µs    10.5 MB/sec
parser/numpy/globals.py                    1.01    161.7±5.18µs    18.2 MB/sec    1.00    160.8±5.01µs    18.3 MB/sec
parser/pydantic/types.py                   1.01      3.6±0.08ms     7.1 MB/sec    1.00      3.5±0.06ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Improve message sorting performance" to "Improve `Message` sorting performance" by @MichaReiser on 2023-05-24 11:36_

---

_@konstin approved on 2023-05-24 11:52_

---

_@charliermarsh approved on 2023-05-24 13:36_

Thanks!

---

_Merged by @MichaReiser on 2023-05-24 14:34_

---

_Closed by @MichaReiser on 2023-05-24 14:34_

---

_Branch deleted on 2023-05-24 14:34_

---

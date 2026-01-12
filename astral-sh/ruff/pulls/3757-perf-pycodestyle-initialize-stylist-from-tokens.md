```yaml
number: 3757
title: "perf(pycodestyle): Initialize Stylist from tokens"
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: stylist-tokens
created_at: 2023-03-27T10:34:55Z
updated_at: 2023-03-28T11:47:14Z
url: https://github.com/astral-sh/ruff/pull/3757
synced_at: 2026-01-12T15:55:13Z
```

# perf(pycodestyle): Initialize Stylist from tokens

---

_@MichaReiser_

This PR changes `Stylist` to initialize from tokens instead of re-lexing a string, which, in the worst case, requires re-lexing the whole program. 

This was the reason why `numpy/globals.py` regressed by close to 15% compared to when pydocstyle is disabled. 

## Benchmarks

* `non-logical`: `main` with logical_lines disabled 
* `pr3737`: Base reference with `logical_lines` enabled
* pr3575: This PR with `logical_lines` enabled
* `pr3575-non-logical`: This PR with `logical_lines` disabled

```
group                                      no-logical                             pr3737                                 pr3757                                 pr3757-non-logical
-----                                      ----------                             ------                                 ------                                 -----------
linter/all-rules/large/dataset.py          1.02      8.5±0.01ms     4.8 MB/sec    1.05      8.8±0.09ms     4.6 MB/sec    1.05      8.8±0.06ms     4.6 MB/sec    1.00      8.4±0.16ms     4.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      2.1±0.01ms     7.8 MB/sec    1.11      2.3±0.06ms     7.3 MB/sec    1.07      2.2±0.01ms     7.5 MB/sec    1.00      2.1±0.01ms     8.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    247.9±3.25µs    11.9 MB/sec    1.10    271.5±1.72µs    10.9 MB/sec    1.00    248.0±2.34µs    11.9 MB/sec    1.00    248.7±5.94µs    11.9 MB/sec
linter/all-rules/pydantic/types.py         1.03      3.7±0.04ms     7.0 MB/sec    1.09      3.9±0.02ms     6.6 MB/sec    1.06      3.8±0.04ms     6.7 MB/sec    1.00      3.6±0.04ms     7.2 MB/sec
linter/default-rules/large/dataset.py      1.00      4.7±0.01ms     8.7 MB/sec    1.07      5.0±0.02ms     8.1 MB/sec    1.08      5.1±0.02ms     8.0 MB/sec    1.00      4.7±0.07ms     8.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00    999.5±5.57µs    16.7 MB/sec    1.09  1088.3±28.69µs    15.3 MB/sec    1.08  1076.3±10.24µs    15.5 MB/sec    1.00  1000.5±12.04µs    16.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    101.0±0.41µs    29.2 MB/sec    1.20    121.7±2.16µs    24.2 MB/sec    1.12    113.5±2.92µs    26.0 MB/sec    1.05    106.4±3.06µs    27.7 MB/sec
linter/default-rules/pydantic/types.py     1.03      2.1±0.01ms    11.9 MB/sec    1.11      2.3±0.02ms    11.0 MB/sec    1.11      2.3±0.04ms    11.0 MB/sec    1.00      2.1±0.04ms    12.2 MB/sec
```

This shows that the performance compared to main is nearly unchanged, but performance improves when enabling `logical_lines` (worst case is now about 10% of regression and no longer 20%)

## CPython Benchmark

This is a end-to-end benchmark comparing the performance between the logical lines implementation of this PR, the logical lines implementation at the start of this stack, and when logical lines is disabled. The results show that

* Reduces the slowdown of enabling logical lines from 1.71 to 1.26
* Caching performance depends on the number of enabled rules... :confused: 
* This stack improves the performance of running all rules by roughly 10%
   
    
### Default Rules - No cache

<pre>
<b>Summary</b>
  &apos;<span style="color:#2AA1B3">./no-logical-main ./crates/ruff/resources/test/cpython/ --no-cache</span>&apos; ran
<span style="color:#26A269"><b>    1.00</b></span> ± <span style="color:#26A269">0.02</span> times faster than &apos;<span style="color:#A347BA">./no-logical-pr ./crates/ruff/resources/test/cpython/ --no-cache</span>&apos;
<span style="color:#26A269"><b>    1.26</b></span> ± <span style="color:#26A269">0.03</span> times faster than &apos;<span style="color:#A347BA">./logical-pr ./crates/ruff/resources/test/cpython/ --no-cache</span>&apos;
<span style="color:#26A269"><b>    1.71</b></span> ± <span style="color:#26A269">0.05</span> times faster than &apos;<span style="color:#A347BA">./logical-main ./crates/ruff/resources/test/cpython/ --no-cache</span>&apos;</pre>

### Default Rules -- Cache

<pre> 
<b>Summary</b>
  &apos;<span style="color:#2AA1B3">./no-logical-main ./crates/ruff/resources/test/cpython/</span>&apos; ran
<span style="color:#26A269"><b>    1.00</b></span> ± <span style="color:#26A269">0.06</span> times faster than &apos;<span style="color:#A347BA">./no-logical-pr ./crates/ruff/resources/test/cpython/</span>&apos;
<span style="color:#26A269"><b>    2.07</b></span> ± <span style="color:#26A269">0.11</span> times faster than &apos;<span style="color:#A347BA">./logical-pr ./crates/ruff/resources/test/cpython/</span>&apos;
<span style="color:#26A269"><b>    2.08</b></span> ± <span style="color:#26A269">0.12</span> times faster than &apos;<span style="color:#A347BA">./logical-main ./crates/ruff/resources/test/cpython/</span>&apos;
</pre>

### All Rules - No cache

<pre>
<b>Summary</b>
  &apos;<span style="color:#2AA1B3">./no-logical-pr ./crates/ruff/resources/test/cpython/ --no-cache --select=ALL -e</span>&apos; ran
<span style="color:#26A269"><b>    1.06</b></span> ± <span style="color:#26A269">0.03</span> times faster than &apos;<span style="color:#A347BA">./no-logical-main ./crates/ruff/resources/test/cpython/ --no-cache --select=ALL -e</span>&apos;
<span style="color:#26A269"><b>    1.07</b></span> ± <span style="color:#26A269">0.03</span> times faster than &apos;<span style="color:#A347BA">./logical-pr ./crates/ruff/resources/test/cpython/ --no-cache --select=ALL -e</span>&apos;
<span style="color:#26A269"><b>    1.24</b></span> ± <span style="color:#26A269">0.03</span> times faster than &apos;<span style="color:#A347BA">./logical-main ./crates/ruff/resources/test/cpython/ --no-cache --select=ALL -e</span>&apos;
</pre>

### All Rules - Cache
<pre><b>Summary</b>
  &apos;<span style="color:#2AA1B3">./no-logical-pr check ./crates/ruff/resources/test/cpython/ --select=ALL -e</span>&apos; ran
<span style="color:#26A269"><b>    1.08</b></span> ± <span style="color:#26A269">0.04</span> times faster than &apos;<span style="color:#A347BA">./logical-pr check ./crates/ruff/resources/test/cpython/ --select=ALL -e</span>&apos;
<span style="color:#26A269"><b>    1.10</b></span> ± <span style="color:#26A269">0.04</span> times faster than &apos;<span style="color:#A347BA">./no-logical-main check ./crates/ruff/resources/test/cpython/ --select=ALL -e</span>&apos;
<span style="color:#26A269"><b>    1.22</b></span> ± <span style="color:#26A269">0.05</span> times faster than &apos;<span style="color:#A347BA">./logical-main check ./crates/ruff/resources/test/cpython/ --select=ALL -e</span>&apos;
</pre>


## Memory usage

The stack improves memory usage due to the removed `text` and `mappings`. Measuring the impact of logical lines vs main is difficult because the new lint generates a ton more violations than running the existing default set and all these Diagnostics must be stored in memory. 

But here are some rough numbers

- logical main: 380.71875MB
- logical pr: 356.84375MB
- main: 284.6171875MB

So this is a 24MB reduction of memory usage, roughly ~6% less. 

---

_Comment by @MichaReiser on 2023-03-27 10:35_

Current dependencies on/for this PR:
* main
  * **PR #3714** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3714" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #3715** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3715" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #3735** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3735" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #3736** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3736" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
          * **PR #3745** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3745" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
            * **PR #3757** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3757" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈
          * **PR #3737** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3737" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/charliermarsh/ruff/3757?utm_source=stack-comment).

---

_Comment by @github-actions[bot] on 2023-03-27 11:04_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+1, -1, 0 error(s))

<details><summary>bokeh (+1, -1)</summary>
<p>

```diff
+ src/bokeh/util/browser.py:120:5: SIM108 [*] Use ternary operator `url = location if location.startswith("http") else "file://" + abspath(location)` instead of `if`-`else`-block
- src/bokeh/util/browser.py:120:5: SIM108 [*] Use ternary operator `url = location if location.startswith('http') else 'file://' + abspath(location)` instead of `if`-`else`-block
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.7±0.04ms     3.0 MB/sec    1.00     13.7±0.05ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.01ms     4.6 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.06    494.2±0.92µs     6.0 MB/sec    1.00    464.9±3.31µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.0±0.03ms     4.2 MB/sec    1.00      5.9±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.03      7.3±0.02ms     5.6 MB/sec    1.00      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1652.1±3.22µs    10.1 MB/sec    1.00  1619.9±12.47µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    182.0±0.45µs    16.2 MB/sec    1.00    182.2±1.42µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.4±0.01ms     7.4 MB/sec    1.00      3.3±0.02ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.3±0.27ms     2.7 MB/sec    1.03     15.7±0.28ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.12ms     4.0 MB/sec    1.00      4.1±0.13ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.10   546.9±25.21µs     5.4 MB/sec    1.00   495.3±13.46µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.14ms     3.8 MB/sec    1.00      6.8±0.21ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.11ms     5.0 MB/sec    1.04      8.5±0.67ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1827.9±32.09µs     9.1 MB/sec    1.07  1946.9±322.25µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    205.0±7.41µs    14.4 MB/sec    1.01   207.7±24.27µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.9±0.12ms     6.5 MB/sec    1.00      3.8±0.36ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Initialize Stylist from tokens" to "perf(pydocstyle): Initialize Stylist from tokens" by @MichaReiser on 2023-03-27 11:45_

---

_@MichaReiser reviewed on 2023-03-27 11:48_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/source_code/stylist.rs`:79 on 2023-03-27 11:48_

Ideally, we wouldn't need to iterate here but that's not possible without cloning `tokens` because `parse_program` requires owned tokens. 

Iterating over some tokens here should be significantly cheaper than re-lexing all the tokens. 

---

_Marked ready for review by @MichaReiser on 2023-03-27 11:52_

---

_Comment by @MichaReiser on 2023-03-27 12:26_

> ## PR Check Results
> ### Ecosystem
> 
> information_source ecosystem check **detected changes**. (+1, -1, 0 error(s))
> bokeh (+1, -1)


Using `"` is correct (which it now detects from my understanding). Source file https://github.com/bokeh/bokeh/blob/branch-3.2/src/bokeh/util/browser.py

---

_Review requested from @charliermarsh by @MichaReiser on 2023-03-27 12:31_

---

_Renamed from "perf(pydocstyle): Initialize Stylist from tokens" to "perf(pycodestyle): Initialize Stylist from tokens" by @charliermarsh on 2023-03-27 13:09_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/stylist.rs`:79 on 2023-03-27 15:21_

If we're doing this already, would it make sense to remove the `OnceCell` entirely from `indentation` and `quote`?

---

_@charliermarsh reviewed on 2023-03-27 15:21_

---

_@charliermarsh approved on 2023-03-27 15:21_

---

_@MichaReiser reviewed on 2023-03-27 15:51_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/source_code/stylist.rs`:79 on 2023-03-27 15:51_

Not sure. The problem is that we'll use `Locator` which I, as far I understand, we should avoid because building the index is expensive 

---

_Comment by @charliermarsh on 2023-03-27 17:07_

> Using " is correct...

Agree.

---

_Comment by @charliermarsh on 2023-03-27 19:01_

(You might be able to merge this into `main` directly.)

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/source_code/stylist.rs`:79 on 2023-03-27 19:01_

Eh yeah, alright, let's leave it as-is. At some point we should get some data or otherwise develop intuition around how often `Locator` is actually getting initialized (i.e., is every file populating it anyway, despite doing so lazily? Or no?).

---

_@charliermarsh reviewed on 2023-03-27 19:01_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/source_code/stylist.rs`:79 on 2023-03-28 09:27_

I would favor storing the byte offsets on the nodes. This removes the whole overhead and shrinks the size of nodes by 16 bytes (reduces the storage for start/end from 32 bytes to 16 bytes)

---

_@MichaReiser reviewed on 2023-03-28 09:27_

---

_Merged by @MichaReiser on 2023-03-28 09:53_

---

_Closed by @MichaReiser on 2023-03-28 09:53_

---

_Branch deleted on 2023-03-28 09:53_

---

```yaml
number: 3901
title: Render code frame with context
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: source-frame-context
created_at: 2023-04-06T14:54:56Z
updated_at: 2023-04-11T08:32:35Z
url: https://github.com/astral-sh/ruff/pull/3901
synced_at: 2026-01-12T15:55:14Z
```

# Render code frame with context

---

_@MichaReiser_

This PR changes the `text` and `grouped` emitter to show up to two lines of context before and after the offending code. 

<pre>crates/ruff/resources/test/cpython/Tools/scripts/pysource.py:20:89: E501 Line too long (89 &gt; 88 characters)
<span style="color:#2A7BDE"><b>   |</b></span>
<span style="color:#2A7BDE"><b>20 |</b></span> __author__ = &quot;Oleg Broytmann, Georg Brandl&quot;
<span style="color:#2A7BDE"><b>21 |</b></span> 
<span style="color:#2A7BDE"><b>22 |</b></span> __all__ = [&quot;has_python_ext&quot;, &quot;looks_like_python&quot;, &quot;can_be_compiled&quot;, &quot;walk_python_files&quot;]
<span style="color:#2A7BDE"><b>   |</b></span><span style="color:#F66151"><b>                                                                                         ^</b></span> <span style="color:#F66151"><b>E501</b></span>
<span style="color:#2A7BDE"><b>   |</b></span>

crates/ruff/resources/test/cpython/Tools/scripts/pysource.py:23:1: E401 Multiple imports on one line
<span style="color:#2A7BDE"><b>   |</b></span>
<span style="color:#2A7BDE"><b>23 |</b></span> import os, re
<span style="color:#2A7BDE"><b>   |</b></span><span style="color:#F66151"><b> ^^^^^^^^^^^^^</b></span> <span style="color:#F66151"><b>E401</b></span>
<span style="color:#2A7BDE"><b>24 |</b></span> 
<span style="color:#2A7BDE"><b>25 |</b></span> binary_re = re.compile(br&apos;[\x00-\x08\x0E-\x1F\x7F]&apos;)
<span style="color:#2A7BDE"><b>   |</b></span>

crates/ruff/resources/test/cpython/Tools/scripts/pysource.py:30:13: E701 Multiple statements on one line (colon)
<span style="color:#2A7BDE"><b>   |</b></span>
<span style="color:#2A7BDE"><b>30 |</b></span> def print_debug(msg):
<span style="color:#2A7BDE"><b>31 |</b></span>     if debug: print(msg)
<span style="color:#2A7BDE"><b>   |</b></span><span style="color:#F66151"><b>             ^</b></span> <span style="color:#F66151"><b>E701</b></span>
<span style="color:#2A7BDE"><b>   |</b></span>
</pre>

## Performance

This change significantly impacts performance for projects that have many diagnostics (500k). Running ruff on cpython with all rules and `--show-source` regresses by about 50% when using a cache. I think this is fine. We can decide to limit the diagnostics to show at max 500 (or another arbitrary) number by default that users can override if they want to see more. It's also worth pointing out that the slowest part is still my terminal trying to render all diagnostics :smile: 

* `./target/release/ruff`: This version
* `./ruff-source-code`: base reference (main)

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `./target/release/ruff ./crates/ruff/resources/test/cpython/ -e   ` | 37.9 ± 1.5 | 34.6 | 41.5 | 1.00 |
| `./ruff-source-code ./crates/ruff/resources/test/cpython/ -e   ` | 37.9 ± 1.8 | 33.5 | 42.1 | 1.00 ± 0.06 |
| `./target/release/ruff ./crates/ruff/resources/test/cpython/ -e --no-cache  ` | 211.2 ± 6.6 | 202.9 | 226.7 | 5.58 ± 0.28 |
| `./ruff-source-code ./crates/ruff/resources/test/cpython/ -e --no-cache  ` | 214.4 ± 7.7 | 206.3 | 232.4 | 5.66 ± 0.30 |
| `./target/release/ruff ./crates/ruff/resources/test/cpython/ -e  --select=ALL ` | 433.3 ± 7.2 | 426.7 | 449.1 | 11.44 ± 0.50 |
| `./ruff-source-code ./crates/ruff/resources/test/cpython/ -e  --select=ALL ` | 437.9 ± 9.3 | 426.7 | 452.7 | 11.57 ± 0.52 |
| `./target/release/ruff ./crates/ruff/resources/test/cpython/ -e --no-cache --select=ALL ` | 757.8 ± 12.8 | 735.9 | 778.3 | 20.02 ± 0.87 |
| `./ruff-source-code ./crates/ruff/resources/test/cpython/ -e --no-cache --select=ALL ` | 765.9 ± 14.3 | 748.2 | 790.1 | 20.23 ± 0.89 |
| `./target/release/ruff ./crates/ruff/resources/test/cpython/ -e   --show-source` | 86.6 ± 2.3 | 83.5 | 94.5 | 2.29 ± 0.11 |
| `./ruff-source-code ./crates/ruff/resources/test/cpython/ -e   --show-source` | 68.0 ± 2.0 | 65.0 | 72.9 | 1.80 ± 0.09 |
| `./target/release/ruff ./crates/ruff/resources/test/cpython/ -e --no-cache  --show-source` | 254.8 ± 5.0 | 245.4 | 263.3 | 6.73 ± 0.30 |
| `./ruff-source-code ./crates/ruff/resources/test/cpython/ -e --no-cache  --show-source` | 244.6 ± 6.7 | 237.2 | 261.4 | 6.46 ± 0.31 |
| `./target/release/ruff ./crates/ruff/resources/test/cpython/ -e  --select=ALL --show-source` | 1549.5 ± 12.3 | 1529.1 | 1564.4 | 40.93 ± 1.67 |
| `./ruff-source-code ./crates/ruff/resources/test/cpython/ -e  --select=ALL --show-source` | 1079.5 ± 20.9 | 1061.8 | 1134.8 | 28.51 ± 1.27 |
| `./target/release/ruff ./crates/ruff/resources/test/cpython/ -e --no-cache --select=ALL --show-source` | 1896.6 ± 27.4 | 1857.3 | 1934.6 | 50.09 ± 2.13 |
| `./ruff-source-code ./crates/ruff/resources/test/cpython/ -e --no-cache --select=ALL --show-source` | 1439.7 ± 36.1 | 1401.9 | 1493.6 | 38.03 ± 1.80 |


---

_Comment by @MichaReiser on 2023-04-06 14:55_

Current dependencies on/for this PR:
* main
  * **PR #3895** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3895" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #3896** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3896" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #3897** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3897" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #3901** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3901" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈
          * **PR #3904** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3904" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
            * **PR #3905** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3905" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
              * **PR #3906** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3906" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/charliermarsh/ruff/3901?utm_source=stack-comment).

---

_Comment by @github-actions[bot] on 2023-04-06 15:11_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.18     19.2±0.38ms     2.1 MB/sec    1.00     16.2±0.66ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.16      4.7±0.11ms     3.5 MB/sec    1.00      4.0±0.21ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.19   602.7±18.35µs     4.9 MB/sec    1.00   504.5±22.32µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.17      8.2±0.23ms     3.1 MB/sec    1.00      7.0±0.54ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.17      9.6±0.10ms     4.3 MB/sec    1.00      8.2±0.41ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.17      2.1±0.03ms     7.9 MB/sec    1.00  1812.2±90.89µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.17    255.1±5.59µs    11.6 MB/sec    1.00   217.8±12.86µs    13.5 MB/sec
linter/default-rules/pydantic/types.py     1.15      4.4±0.07ms     5.8 MB/sec    1.00      3.8±0.29ms     6.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.8±0.87ms  2003.1 KB/sec    1.09     22.8±0.61ms  1830.4 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.21ms     3.2 MB/sec    1.12      5.8±0.31ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   623.2±47.78µs     4.7 MB/sec    1.09   679.3±25.84µs     4.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.6±0.32ms     3.0 MB/sec    1.10      9.5±0.34ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.00     10.2±0.29ms     4.0 MB/sec    1.11     11.3±0.27ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.3±0.08ms     7.1 MB/sec    1.00      2.3±0.10ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   263.6±12.51µs    11.2 MB/sec    1.08   285.2±16.86µs    10.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.0±0.21ms     5.1 MB/sec    1.00      5.0±0.24ms     5.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser reviewed on 2023-04-06 15:34_

---

_Review comment by @MichaReiser on `crates/ruff/src/message/snapshots/ruff__message__text__tests__default.snap`:1 on 2023-04-06 15:34_

This file nicely shows the change. You can see that the emitter now shows the additional lines 5-6 and 7-8 to give the user more context about where in the code the violation is. Mainly because `x = 1` doesn't give you much context. 

---

_@MichaReiser reviewed on 2023-04-06 16:06_

---

_Review comment by @MichaReiser on `crates/ruff/src/message/mod.rs`:135 on 2023-04-06 16:06_

I added an extra empty line here to demonstrate that it truncates trailing empty lines. I had to change the line number of the `x` diagnostic because of that and it propagated to all snapshots.

---

_Marked ready for review by @MichaReiser on 2023-04-06 16:07_

---

_@MichaReiser reviewed on 2023-04-06 16:12_

---

_Review comment by @MichaReiser on `crates/ruff/src/message/text.rs`:189 on 2023-04-06 16:12_

This is an odd formatting... :elephant: 

---

_@charliermarsh reviewed on 2023-04-06 22:08_

---

_Review comment by @charliermarsh on `crates/ruff/src/message/snapshots/ruff__message__text__tests__default.snap`:1 on 2023-04-06 22:08_

Yeah this looks great. Do you know what (e.g.) Rust's own rules are for this?

---

_@charliermarsh approved on 2023-04-06 22:08_

---

_@MichaReiser reviewed on 2023-04-07 10:27_

---

_Review comment by @MichaReiser on `crates/ruff/src/message/snapshots/ruff__message__text__tests__default.snap`:1 on 2023-04-07 10:27_

Rust seems to render the code span of the diagnostic without showing any context lines

```
warning: unused variable: `a`
   --> crates/ruff_cli/src/printer.rs:160:25
    |
160 |                     let a = 0usize as u32;
    |                         ^ help: if this is intentional, prefix it with an underscore: `_a`
    |
    = note: `#[warn(unused_variables)]` on by default
```

The same is true for clippy.

```
warning: casting `usize` to `u32` may truncate the value on targets with 64-bit wide pointers
   --> crates/ruff_cli/src/printer.rs:160:29
    |
160 |                     let a = 0usize as u32;
    |                             ^^^^^^^^^^^^^
    |
    = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#cast_possible_truncation
    = note: `-W clippy::cast-possible-truncation` implied by `-W clippy::pedantic`
```

I guess there are reasons for doing either. Not showing additional context lines is faster and leads to smaller output. Showing the additional lines can help users to understand the error better. E.g that the variable is unused: What's the context? What if I use the same variable name multiple times. 

---

_Merged by @MichaReiser on 2023-04-11 08:22_

---

_Closed by @MichaReiser on 2023-04-11 08:22_

---

_Branch deleted on 2023-04-11 08:22_

---

```yaml
number: 3895
title: Extract message emitters from Printer
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: extract-message-emitters
created_at: 2023-04-06T08:02:47Z
updated_at: 2023-04-11T07:56:51Z
url: https://github.com/astral-sh/ruff/pull/3895
synced_at: 2026-01-12T04:28:19Z
```

# Extract message emitters from Printer

---

_Pull request opened by @MichaReiser on 2023-04-06 08:02_

This PR extracts the emitters for serializing `Messages` from the `Printer` in the CLI to `ruff`. The reason for moving the implementations to ruff is so that I use the text emitter to write the linter snapshots instead of serializing the `Diagnostic` to YAML. Using the rendered text output has the advantage that we can do refactors like replacing `Location` with `TextSize` without affecting the snapshot outputs, which gives us confidence in the refactor. 


I used this chance to:

* Add tests for all emitters
* Rewrite some emitters to use `write` directly rather than using `format` and then writing the formatted string. 

## Considerations

* Introducing a shared trait for the emitters isn't necessary from an implementation point of view because the `Printer` calls `emit` statically on all emitters (it isn't using dynamic dispatching). However, it helped to share logic between tests and it ensures that all emitters are structured similarly.  
* Implementing all emitters in the `ruff` crate isn't strictly necessary. For example, I don't plan to use the `junit` emitter for the test infrastructure. I still decided to put them all into the same module for better discoverability. 


## Performance

Avoiding `format` and `to_string` improves performance a bit

<pre><span style="color:#005FD7">hyperfine</span> <span style="color:#00AFFF">--warmup</span> <span style="color:#00AFFF">10</span> \
          <span style="color:#999900">&quot;./target/release/ruff ./crates/ruff/resources/test/cpython/ -e --select=ALL&quot;</span> \
          <span style="color:#999900">&quot;./ruff-main ./crates/ruff/resources/test/cpython/ -e --select=ALL&quot;</span>
<b>Benchmark 1</b>: ./target/release/ruff ./crates/ruff/resources/test/cpython/ -e --select=ALL
  Time (<span style="color:#26A269"><b>mean</b></span> ± <span style="color:#26A269">σ</span>):     <span style="color:#26A269"><b>442.6 ms</b></span> ± <span style="color:#26A269"> 10.0 ms</span>    [User: <span style="color:#12488B">722.9 ms</span>, System: <span style="color:#12488B">509.2 ms</span>]
  Range (<span style="color:#2AA1B3">min</span> … <span style="color:#A347BA">max</span>):   <span style="color:#2AA1B3">422.4 ms</span> … <span style="color:#A347BA">456.9 ms</span>    10 runs
 
<b>Benchmark 2</b>: ./ruff-main ./crates/ruff/resources/test/cpython/ -e --select=ALL
  Time (<span style="color:#26A269"><b>mean</b></span> ± <span style="color:#26A269">σ</span>):     <span style="color:#26A269"><b>456.9 ms</b></span> ± <span style="color:#26A269">  7.0 ms</span>    [User: <span style="color:#12488B">724.0 ms</span>, System: <span style="color:#12488B">507.0 ms</span>]
  Range (<span style="color:#2AA1B3">min</span> … <span style="color:#A347BA">max</span>):   <span style="color:#2AA1B3">448.1 ms</span> … <span style="color:#A347BA">469.2 ms</span>    10 runs
 
<b>Summary</b>
  &apos;<span style="color:#2AA1B3">./target/release/ruff ./crates/ruff/resources/test/cpython/ -e --select=ALL</span>&apos; ran
<span style="color:#26A269"><b>    1.03</b></span> ± <span style="color:#26A269">0.03</span> times faster than &apos;<span style="color:#A347BA">./ruff-main ./crates/ruff/resources/test/cpython/ -e --select=ALL</span>&apos;
</pre>

---

_Comment by @MichaReiser on 2023-04-06 08:02_

Current dependencies on/for this PR:
* main
  * **PR #3895** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3895" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #3896** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3896" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #3897** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3897" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
        * **PR #3901** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3901" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
          * **PR #3904** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3904" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
            * **PR #3905** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3905" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 
              * **PR #3906** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/3906" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/charliermarsh/ruff/3895?utm_source=stack-comment).

---

_Review comment by @MichaReiser on `crates/ruff/src/message/mod.rs`:32 on 2023-04-06 08:31_

Copied from `src/message.rs`

---

_Review comment by @MichaReiser on `crates/ruff/src/message/mod.rs`:126 on 2023-04-06 08:31_

This is new

---

_Comment by @github-actions[bot] on 2023-04-06 08:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     21.0±0.59ms  1987.7 KB/sec    1.00     20.9±0.65ms  1994.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.1±0.18ms     3.3 MB/sec    1.00      5.0±0.19ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.02   639.2±27.48µs     4.6 MB/sec    1.00   627.7±22.28µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.7±0.32ms     2.9 MB/sec    1.01      8.8±0.36ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.01     10.4±0.51ms     3.9 MB/sec    1.00     10.3±0.31ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.07ms     7.4 MB/sec    1.00      2.2±0.08ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.01   271.0±12.66µs    10.9 MB/sec    1.00   267.6±13.81µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.8±0.26ms     5.3 MB/sec    1.00      4.7±0.18ms     5.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     22.9±1.10ms  1822.3 KB/sec    1.00     22.5±1.49ms  1850.1 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      6.2±0.49ms     2.7 MB/sec    1.00      6.0±0.46ms     2.8 MB/sec
linter/all-rules/numpy/globals.py          1.01   681.5±64.91µs     4.3 MB/sec    1.00   672.4±41.13µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.6±0.68ms     2.7 MB/sec    1.03      9.9±0.65ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.00     11.0±0.86ms     3.7 MB/sec    1.06     11.6±0.60ms     3.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.15ms     7.3 MB/sec    1.09      2.5±0.11ms     6.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   286.6±22.19µs    10.3 MB/sec    1.05   302.1±24.35µs     9.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.0±0.38ms     5.1 MB/sec    1.08      5.4±0.58ms     4.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser reviewed on 2023-04-06 08:58_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-04-06 08:58_

---

_Marked ready for review by @MichaReiser on 2023-04-06 08:59_

---

_@charliermarsh reviewed on 2023-04-06 22:05_

---

_Review comment by @charliermarsh on `crates/ruff/src/message/github.rs`:11 on 2023-04-06 22:05_

My read on the Rust naming rules is that this is correct (over `GitHubEmitter`).

---

_@charliermarsh approved on 2023-04-06 22:07_

---

_@MichaReiser reviewed on 2023-04-07 10:24_

---

_Review comment by @MichaReiser on `crates/ruff/src/message/github.rs`:11 on 2023-04-07 10:24_

I don't know. I chose this naming to keep it consistent with `SerializationFormat::Github`

---

_Merged by @MichaReiser on 2023-04-11 07:24_

---

_Closed by @MichaReiser on 2023-04-11 07:24_

---

_Branch deleted on 2023-04-11 07:24_

---

_Label `internal` added by @MichaReiser on 2023-04-11 07:56_

---

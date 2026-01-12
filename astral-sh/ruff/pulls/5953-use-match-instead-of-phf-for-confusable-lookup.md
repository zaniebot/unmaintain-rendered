```yaml
number: 5953
title: "Use `match` instead of `phf` for confusable lookup"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - performance
assignees: []
merged: true
base: main
head: charlie/match-phf
created_at: 2023-07-21T17:12:28Z
updated_at: 2023-07-24T02:50:03Z
url: https://github.com/astral-sh/ruff/pull/5953
synced_at: 2026-01-12T03:30:22Z
```

# Use `match` instead of `phf` for confusable lookup

---

_Pull request opened by @charliermarsh on 2023-07-21 17:12_

I don't know whether we want to make this change but here's some data...

Binary size:

- `main`: 30,384
- `charlie/match-phf`: 30,416

llvm-lines:

- `main`: 1,784,148
- `charlie/match-phf`: 1,789,877

llvm-lines and binary size are both unchanged (or, by < 5) when moving from `u8` to `u32` return types, and even when moving to `char` keys and values. I didn't expect this, but I'm not very knowledgable on this topic.

Performance:

```
Confusables/match/src   time:   [4.9102 µs 4.9352 µs 4.9777 µs]
                        change: [+1.7469% +2.2421% +2.8710%] (p = 0.00 < 0.05)
                        Performance has regressed.
Found 12 outliers among 100 measurements (12.00%)
  2 (2.00%) low mild
  4 (4.00%) high mild
  6 (6.00%) high severe
Confusables/match-with-skip/src
                        time:   [2.0676 µs 2.0945 µs 2.1317 µs]
                        change: [+0.9384% +1.6000% +2.3920%] (p = 0.00 < 0.05)
                        Change within noise threshold.
Found 8 outliers among 100 measurements (8.00%)
  3 (3.00%) high mild
  5 (5.00%) high severe
Confusables/phf/src     time:   [31.087 µs 31.188 µs 31.305 µs]
                        change: [+1.9262% +2.2188% +2.5496%] (p = 0.00 < 0.05)
                        Performance has regressed.
Found 15 outliers among 100 measurements (15.00%)
  3 (3.00%) low mild
  6 (6.00%) high mild
  6 (6.00%) high severe
Confusables/phf-with-skip/src
                        time:   [2.0470 µs 2.0486 µs 2.0502 µs]
                        change: [-0.3093% -0.1446% +0.0106%] (p = 0.08 > 0.05)
                        No change in performance detected.
Found 4 outliers among 100 measurements (4.00%)
  2 (2.00%) high mild
  2 (2.00%) high severe
```

The `-with-skip` variants add our optimization which first checks whether the character is ASCII. So `match` is way, way faster than PHF, but it tends not to matter since almost all source code is ASCII anyway.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-21 17:32_

---

_Review requested from @konstin by @charliermarsh on 2023-07-21 17:32_

---

_Comment by @github-actions[bot] on 2023-07-21 17:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.08     11.9±0.41ms     3.4 MB/sec    1.00     11.1±0.60ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.06      2.3±0.11ms     7.2 MB/sec    1.00      2.2±0.11ms     7.6 MB/sec
formatter/numpy/globals.py                 1.06   269.3±17.33µs    11.0 MB/sec    1.00   253.4±19.79µs    11.6 MB/sec
formatter/pydantic/types.py                1.08      5.3±0.23ms     4.8 MB/sec    1.00      4.9±0.22ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.02     17.2±0.84ms     2.4 MB/sec    1.00     16.9±0.57ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.08      4.4±0.23ms     3.8 MB/sec    1.00      4.1±0.22ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.06   563.7±20.58µs     5.2 MB/sec    1.00   533.6±23.56µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.07      7.9±0.31ms     3.2 MB/sec    1.00      7.4±0.28ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.04      8.6±0.34ms     4.7 MB/sec    1.00      8.3±0.37ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.08  1773.6±54.05µs     9.4 MB/sec    1.00  1647.8±83.12µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.06   218.4±13.82µs    13.5 MB/sec    1.00   206.7±13.64µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.05      3.9±0.17ms     6.5 MB/sec    1.00      3.7±0.17ms     6.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.0±0.08ms     3.7 MB/sec    1.00     11.0±0.08ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.02ms     7.8 MB/sec    1.00      2.1±0.03ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    234.7±2.75µs    12.6 MB/sec    1.01    236.1±6.92µs    12.5 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.03ms     5.4 MB/sec    1.01      4.8±0.06ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.10ms     2.6 MB/sec    1.00     15.4±0.16ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.02ms     4.1 MB/sec    1.00      4.1±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    419.4±7.10µs     7.0 MB/sec    1.00   416.5±14.08µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.1±0.05ms     3.6 MB/sec    1.00      7.0±0.06ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.10ms     5.0 MB/sec    1.00      8.1±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1656.5±18.57µs    10.1 MB/sec    1.00  1653.0±18.69µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    174.1±2.02µs    16.9 MB/sec    1.00    173.4±1.50µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.0 MB/sec    1.00      3.6±0.05ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @MichaReiser on 2023-07-21 18:41_

What input source files did you use for the benchmark? Did any file contain non-ascii text

---

_Comment by @charliermarsh on 2023-07-21 18:42_

No, but most files don't contain any non-ascii text, so I think the benchmarks without `skip` are most useful, since they check every character regardless of whether it's ascii. 

---

_Comment by @MichaReiser on 2023-07-21 18:50_

> No, but most files don't contain any non-ascii text, so I think the benchmarks without `skip` are most useful, since they check every character regardless of whether it's ascii.

I don't think we should take this as a given, especially considering how many reports we got for confusable units (non ascii) and line width. I also worked in many companies where the practice was to write comments in German, but everything else in English. We should probably add a non-ascii test case. Also to make sure our other infra (like LineIndex) work correctly with non-ascii text

---

_Comment by @charliermarsh on 2023-07-21 18:52_

If we care about performance on non-ASCII code, then we should almost certainly use the `match`, I think?

---

_Comment by @charliermarsh on 2023-07-21 18:52_

It's more than 6x faster.

---

_Comment by @MichaReiser on 2023-07-21 19:15_

I'm leaning towards the match:
* Only small binary and LLVM code regression
* Fewer dependencies, easier to understand
* faster

---

_@MichaReiser reviewed on 2023-07-21 19:17_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/confusables.rs`:5 on 2023-07-21 19:17_

Can we use `char` instead of `u32`. I have no idea what I'm looking at :sweat_smile: 

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/confusables.rs`:5 on 2023-07-21 19:46_

Yeah easily. I assume the LHS should remain u32 though?

---

_@charliermarsh reviewed on 2023-07-21 19:46_

---

_@MichaReiser reviewed on 2023-07-21 20:11_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/confusables.rs`:5 on 2023-07-21 20:11_

What would be the reason to prefer the left side to be a `u32` instead of a `char`? I could also think of using the unicode names, but that's probably harder (maybe rustfmt does that for you)

---

_@charliermarsh reviewed on 2023-07-21 20:16_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/confusables.rs`:5 on 2023-07-21 20:16_

We end up with a bunch of confusing Unicode characters in the source, some of which my editor can't even render I think, some of which are bidirectional...

---

_Comment by @charliermarsh on 2023-07-22 14:03_

Yeah agreed -- that makes sense. @konstin, ok with you?

---

_@konstin approved on 2023-07-23 10:56_

i like phf but i see the motivation for this change

---

_Marked ready for review by @charliermarsh on 2023-07-24 01:51_

---

_Renamed from "Use match instead of phf" to "Use `match` instead of `phf` for confusable lookup" by @charliermarsh on 2023-07-24 01:52_

---

_Label `performance` added by @charliermarsh on 2023-07-24 01:52_

---

_Label `internal` added by @charliermarsh on 2023-07-24 01:52_

---

_Merged by @charliermarsh on 2023-07-24 02:23_

---

_Closed by @charliermarsh on 2023-07-24 02:23_

---

_Branch deleted on 2023-07-24 02:23_

---

```yaml
number: 6417
title: "Fix name of rule in example of `extend-per-file-ignores` in options.rs"
type: pull_request
state: merged
author: piotrgredowski
labels: []
assignees: []
merged: true
base: main
head: fix-docs-typo
created_at: 2023-08-08T08:36:07Z
updated_at: 2023-08-08T09:24:41Z
url: https://github.com/astral-sh/ruff/pull/6417
synced_at: 2026-01-12T15:55:21Z
```

# Fix name of rule in example of `extend-per-file-ignores` in options.rs

---

_@piotrgredowski_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?

-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fix name of rule in example of `extend-per-file-ignores` in `options.rs` file.

It was `E401` but in configuration example `E402` was listed. Just a tiny mismatch.

## Test Plan

<!-- How was it tested? -->

Just by my eyes :).

---

_Comment by @github-actions[bot] on 2023-08-08 08:50_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.2±0.02ms     4.9 MB/sec    1.00      8.3±0.03ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1603.2±21.63µs    10.4 MB/sec    1.00  1599.2±14.69µs    10.4 MB/sec
formatter/numpy/globals.py                 1.00    168.6±0.33µs    17.5 MB/sec    1.00    167.8±0.31µs    17.6 MB/sec
formatter/pydantic/types.py                1.01      3.6±0.08ms     7.1 MB/sec    1.00      3.5±0.05ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.01     10.5±0.03ms     3.9 MB/sec    1.00     10.5±0.04ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.01ms     5.8 MB/sec    1.00      2.9±0.01ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    314.1±1.88µs     9.4 MB/sec    1.00    313.6±3.42µs     9.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      4.9±0.01ms     5.2 MB/sec    1.00      4.8±0.01ms     5.3 MB/sec
linter/default-rules/large/dataset.py      1.02      5.5±0.04ms     7.4 MB/sec    1.00      5.4±0.01ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1136.9±3.01µs    14.6 MB/sec    1.00   1116.3±9.48µs    14.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    116.8±1.65µs    25.3 MB/sec    1.00    115.3±1.82µs    25.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.4±0.01ms    10.4 MB/sec    1.00      2.4±0.01ms    10.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.0±0.13ms     4.1 MB/sec    1.00      9.9±0.22ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1884.8±23.46µs     8.8 MB/sec    1.00  1881.6±17.94µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    206.6±5.17µs    14.3 MB/sec    1.01    208.5±7.47µs    14.2 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.04ms     6.2 MB/sec    1.02      4.2±0.07ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     12.5±0.15ms     3.3 MB/sec    1.01     12.6±0.16ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.6±0.16ms     4.7 MB/sec    1.00      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.2±7.26µs     7.0 MB/sec    1.00    423.2±7.25µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.09      6.3±0.12ms     4.1 MB/sec    1.00      5.7±0.06ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.07ms     6.0 MB/sec    1.00      6.8±0.07ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1390.3±15.54µs    12.0 MB/sec    1.01  1404.7±19.89µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    159.0±3.26µs    18.6 MB/sec    1.00    157.7±2.48µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.03ms     8.5 MB/sec    1.00      3.0±0.03ms     8.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-08-08 09:24_

---

_Merged by @konstin on 2023-08-08 09:24_

---

_Closed by @konstin on 2023-08-08 09:24_

---

```yaml
number: 4936
title: Test autofix prompting
type: pull_request
state: closed
author: sladyn98
labels: []
assignees: []
draft: true
base: main
head: autofix-experimental
created_at: 2023-06-07T19:45:27Z
updated_at: 2023-06-12T18:32:24Z
url: https://github.com/astral-sh/ruff/pull/4936
synced_at: 2026-01-12T15:55:17Z
```

# Test autofix prompting

---

_@sladyn98_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-06-07 20:24_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.5±0.01ms     7.4 MB/sec  
formatter/numpy/ctypeslib.py               1.00   1085.1±8.18µs    15.3 MB/sec  
formatter/numpy/globals.py                 1.00    119.4±0.79µs    24.7 MB/sec  
formatter/pydantic/types.py                1.00      2.4±0.02ms    10.7 MB/sec  
linter/all-rules/large/dataset.py          1.00     14.3±0.19ms     2.8 MB/sec    1.01     14.4±0.18ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.01      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    410.7±1.14µs     7.2 MB/sec    1.03    421.1±0.50µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     6.0 MB/sec    1.00      6.8±0.03ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1441.5±2.51µs    11.6 MB/sec    1.04   1493.5±9.67µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.2±0.32µs    18.7 MB/sec    1.07    169.3±0.41µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.00ms     8.4 MB/sec    1.02      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                                                           1.00      5.2±0.00ms     7.9 MB/sec
parser/numpy/ctypeslib.py                                                         1.00   1008.6±1.89µs    16.5 MB/sec
parser/numpy/globals.py                                                           1.00    104.5±0.30µs    28.2 MB/sec
parser/pydantic/types.py                                                          1.00      2.2±0.00ms    11.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.7±0.27ms     6.1 MB/sec  
formatter/numpy/ctypeslib.py               1.00  1277.0±40.71µs    13.0 MB/sec  
formatter/numpy/globals.py                 1.00    149.1±7.58µs    19.8 MB/sec  
formatter/pydantic/types.py                1.00      2.9±0.11ms     8.9 MB/sec  
linter/all-rules/large/dataset.py          1.00     16.3±0.31ms     2.5 MB/sec    1.00     16.4±0.47ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.13ms     4.0 MB/sec    1.08      4.5±0.31ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   491.3±18.06µs     6.0 MB/sec    1.04   508.9±19.14µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.16ms     3.7 MB/sec    1.00      7.0±0.22ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.15ms     5.2 MB/sec    1.08      8.5±0.29ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1680.2±47.72µs     9.9 MB/sec    1.07  1795.2±59.65µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.8±7.47µs    14.9 MB/sec    1.13   223.0±13.12µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.07ms     7.0 MB/sec    1.06      3.8±0.12ms     6.6 MB/sec
parser/large/dataset.py                                                           1.00      6.6±0.22ms     6.2 MB/sec
parser/numpy/ctypeslib.py                                                         1.00  1250.2±58.04µs    13.3 MB/sec
parser/numpy/globals.py                                                           1.00    126.5±5.27µs    23.3 MB/sec
parser/pydantic/types.py                                                          1.00      2.8±0.08ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-06-12 18:32_

Closing as I'd like to keep the PR queue actionable where we can!

---

_Closed by @charliermarsh on 2023-06-12 18:32_

---

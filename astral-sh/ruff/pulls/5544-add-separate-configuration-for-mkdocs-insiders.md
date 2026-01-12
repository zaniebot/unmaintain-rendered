```yaml
number: 5544
title: Add separate configuration for MkDocs Insiders plugins
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/insiders
created_at: 2023-07-05T22:28:20Z
updated_at: 2023-07-05T22:55:36Z
url: https://github.com/astral-sh/ruff/pull/5544
synced_at: 2026-01-12T03:36:55Z
```

# Add separate configuration for MkDocs Insiders plugins

---

_Pull request opened by @charliermarsh on 2023-07-05 22:28_

## Summary

This PR adds a separate configuration file to enable us to turn on [Insiders-only plugins](https://squidfunk.github.io/mkdocs-material/insiders/getting-started/#built-in-plugins).

I've turned on the `typeset` plugin which ensures that the settings on the left-hand navigation pane render as code:

<img width="1792" alt="Screen Shot 2023-07-05 at 6 27 20 PM" src="https://github.com/astral-sh/ruff/assets/1309177/c93676dd-bb48-417a-9d3b-528bf001e9b7">


---

_Label `documentation` added by @charliermarsh on 2023-07-05 22:28_

---

_Merged by @charliermarsh on 2023-07-05 22:40_

---

_Closed by @charliermarsh on 2023-07-05 22:40_

---

_Branch deleted on 2023-07-05 22:40_

---

_Comment by @github-actions[bot] on 2023-07-05 22:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.42ms     4.1 MB/sec    1.01     10.0±0.42ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.11ms     7.5 MB/sec    1.00      2.2±0.09ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00   246.5±12.74µs    12.0 MB/sec    1.03   253.9±16.52µs    11.6 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.18ms     5.5 MB/sec    1.02      4.7±0.18ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.01     17.3±0.56ms     2.4 MB/sec    1.00     17.1±0.56ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.16ms     3.9 MB/sec    1.01      4.3±0.21ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.03   573.0±31.91µs     5.1 MB/sec    1.00   557.2±31.85µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.8±0.32ms     3.3 MB/sec    1.00      7.5±0.25ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.01      8.5±0.23ms     4.8 MB/sec    1.00      8.4±0.22ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1817.8±51.56µs     9.2 MB/sec    1.02  1862.6±66.80µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   215.3±13.95µs    13.7 MB/sec    1.00   215.0±15.78µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.15ms     6.6 MB/sec    1.00      3.8±0.14ms     6.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.04     13.7±0.53ms     3.0 MB/sec    1.00     13.1±0.48ms     3.1 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.9±0.15ms     5.8 MB/sec    1.00      2.9±0.17ms     5.8 MB/sec
formatter/numpy/globals.py                 1.00   331.2±15.21µs     8.9 MB/sec    1.00   330.5±19.62µs     8.9 MB/sec
formatter/pydantic/types.py                1.01      6.2±0.24ms     4.1 MB/sec    1.00      6.2±0.27ms     4.1 MB/sec
linter/all-rules/large/dataset.py          1.02     23.1±0.64ms  1805.7 KB/sec    1.00     22.6±0.65ms  1844.0 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.9±0.21ms     2.8 MB/sec    1.00      5.8±0.21ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   718.4±40.68µs     4.1 MB/sec    1.00   717.3±41.04µs     4.1 MB/sec
linter/all-rules/pydantic/types.py         1.01     10.1±0.42ms     2.5 MB/sec    1.00     10.1±0.37ms     2.5 MB/sec
linter/default-rules/large/dataset.py      1.02     11.8±0.37ms     3.5 MB/sec    1.00     11.6±0.65ms     3.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.5±0.08ms     6.7 MB/sec    1.00      2.4±0.10ms     6.9 MB/sec
linter/default-rules/numpy/globals.py      1.01   300.4±18.94µs     9.8 MB/sec    1.00   297.9±16.06µs     9.9 MB/sec
linter/default-rules/pydantic/types.py     1.04      5.2±0.17ms     4.9 MB/sec    1.00      5.0±0.20ms     5.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

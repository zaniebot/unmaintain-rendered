```yaml
number: 5356
title: Run Ruff on Markdown code blocks
type: pull_request
state: closed
author: evanrittenhouse
labels: []
assignees: []
draft: true
base: main
head: 3792_markdown
created_at: 2023-06-25T21:36:40Z
updated_at: 2023-06-29T18:14:48Z
url: https://github.com/astral-sh/ruff/pull/5356
synced_at: 2026-01-12T15:55:18Z
```

# Run Ruff on Markdown code blocks

---

_@evanrittenhouse_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Closes #3792.

This is a draft PR tracking the development of Ruff's support for Markdown. I'm opening it now to get feedback from maintainers on if the proposed design makes sense. 

The next step will involve changing `lint_path` and subsequent calls to work over `str` slices. Python/Jupyter notebooks will always be slices of length 1 - Markdown code blocks will be variable length. We can't concatenate the Markdown source code since the code blocks are independent, unlike Jupyter code blocks. I'd like to get the design solidified before I undertake that work.

There are still some abstractions that could be made, like converting our Jupyter implementation's mapping over to a CodeBlock-based one, as well as standardizing an interface for parsing files that don't solely contain Python. Same as above - I'd like to align on an initial CodeBlock design first, since that will likely be foundational to future embedded-language tasks. 

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-06-25 22:09_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.4±0.23ms     4.8 MB/sec    1.01      8.5±0.27ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.0±0.07ms     8.3 MB/sec    1.00  1975.1±51.02µs     8.4 MB/sec
formatter/numpy/globals.py                 1.05   251.8±28.61µs    11.7 MB/sec    1.00    239.1±7.52µs    12.3 MB/sec
formatter/pydantic/types.py                1.01      4.5±0.13ms     5.7 MB/sec    1.00      4.4±0.10ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     17.1±0.44ms     2.4 MB/sec    1.00     17.1±0.45ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.16ms     4.0 MB/sec    1.00      4.2±0.12ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.04   565.6±18.78µs     5.2 MB/sec    1.00   541.8±15.75µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.6±0.20ms     3.4 MB/sec    1.00      7.5±0.18ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.02      8.5±0.14ms     4.8 MB/sec    1.00      8.4±0.21ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1816.8±37.48µs     9.2 MB/sec    1.00  1815.6±55.72µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    216.3±6.97µs    13.6 MB/sec    1.02   221.2±11.37µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.08ms     6.6 MB/sec    1.02      4.0±0.19ms     6.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.0±0.14ms     5.1 MB/sec    1.01      8.1±0.12ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.01  1856.8±53.15µs     9.0 MB/sec    1.00  1832.8±33.08µs     9.1 MB/sec
formatter/numpy/globals.py                 1.01    224.2±7.09µs    13.2 MB/sec    1.00   222.2±11.35µs    13.3 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.06ms     6.2 MB/sec    1.01      4.1±0.08ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.01     16.0±0.23ms     2.5 MB/sec    1.00     15.9±0.30ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.06ms     4.0 MB/sec    1.00      4.2±0.10ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    509.1±9.10µs     5.8 MB/sec    1.07  545.6±156.86µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.1±0.15ms     3.6 MB/sec    1.00      7.0±0.14ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.13ms     4.9 MB/sec    1.06      8.9±0.67ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1778.1±27.74µs     9.4 MB/sec    1.09  1943.8±711.33µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.8±4.76µs    14.5 MB/sec    1.01    205.1±4.30µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.05ms     6.9 MB/sec    1.01      3.8±0.06ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @evanrittenhouse on 2023-06-26 21:09_

Tagging @MichaReiser for design review - no rush on it as Charlie said it's deprioritized, but it's a fun project that I enjoy working on

This is obviously incredibly rough right now, currently looking to see if the `CodeBlock` design is solid as it'll be used as the foundational unit for linting rather than a `Path`

---

_Closed by @evanrittenhouse on 2023-06-29 18:14_

---

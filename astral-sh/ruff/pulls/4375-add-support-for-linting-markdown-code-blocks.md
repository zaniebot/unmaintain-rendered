```yaml
number: 4375
title: Add support for linting Markdown code blocks
type: pull_request
state: closed
author: evanrittenhouse
labels: []
assignees: []
draft: true
base: main
head: 3792_markdown
created_at: 2023-05-11T13:48:00Z
updated_at: 2023-06-15T17:01:40Z
url: https://github.com/astral-sh/ruff/pull/4375
synced_at: 2026-01-12T03:43:29Z
```

# Add support for linting Markdown code blocks

---

_Pull request opened by @evanrittenhouse on 2023-05-11 13:48_

Adds support for Markdown code blocks.

I'm trying to keep all traits as generic as possible so that we can re-use them for embedded languages or other lintable languages down the road. 

Closes #3792 when done, though I expect that this will have a lot of iteration on design and such before it's done.

---

_Comment by @github-actions[bot] on 2023-05-13 17:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.0±0.02ms     2.9 MB/sec    1.00     13.9±0.05ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.02    424.9±0.57µs     6.9 MB/sec    1.00    416.3±0.51µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.8±0.01ms     6.0 MB/sec    1.00      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1461.7±5.55µs    11.4 MB/sec    1.00   1444.7±4.37µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.02    163.7±0.83µs    18.0 MB/sec    1.00    160.3±2.37µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.00ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
parser/large/dataset.py                    1.01      5.5±0.00ms     7.4 MB/sec    1.00      5.4±0.03ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.03   1078.7±2.32µs    15.4 MB/sec    1.00   1051.5±3.56µs    15.8 MB/sec
parser/numpy/globals.py                    1.03    110.3±0.24µs    26.7 MB/sec    1.00    106.9±0.20µs    27.6 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.00ms    10.9 MB/sec    1.00      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     16.9±0.18ms     2.4 MB/sec    1.00     16.5±0.22ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.03ms     3.9 MB/sec    1.00      4.3±0.02ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.02    441.3±4.88µs     6.7 MB/sec    1.00    431.0±4.25µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.1±0.03ms     3.6 MB/sec    1.00      7.1±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      8.5±0.03ms     4.8 MB/sec    1.00      8.4±0.04ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1760.7±7.95µs     9.5 MB/sec    1.00   1743.6±7.48µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    189.2±1.38µs    15.6 MB/sec    1.00    188.0±5.16µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.01ms     6.7 MB/sec    1.00      3.8±0.07ms     6.7 MB/sec
parser/large/dataset.py                    1.05      7.0±0.02ms     5.8 MB/sec    1.00      6.7±0.02ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.05   1324.9±8.05µs    12.6 MB/sec    1.00   1267.3±7.60µs    13.1 MB/sec
parser/numpy/globals.py                    1.04    136.8±0.72µs    21.6 MB/sec    1.00    131.6±1.87µs    22.4 MB/sec
parser/pydantic/types.py                   1.04      2.9±0.02ms     8.7 MB/sec    1.00      2.8±0.01ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @evanrittenhouse on 2023-05-17 04:02_

A local test file shows it's currently at least working and picking up Python code blocks as expected. Next issue is figuring out why the BoundedCodeBlock's offset isn't applying in the diagnostic -> message conversion.

---

_Closed by @evanrittenhouse on 2023-06-15 17:01_

---

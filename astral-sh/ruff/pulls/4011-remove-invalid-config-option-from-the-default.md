```yaml
number: 4011
title: (📚) Remove invalid config option from the default config example
type: pull_request
state: closed
author: KotlinIsland
labels: []
assignees: []
draft: true
base: main
head: docs/fix_default
created_at: 2023-04-18T22:25:09Z
updated_at: 2023-04-18T23:07:33Z
url: https://github.com/astral-sh/ruff/pull/4011
synced_at: 2026-01-12T04:28:19Z
```

# (📚) Remove invalid config option from the default config example

---

_Pull request opened by @KotlinIsland on 2023-04-18 22:25_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-04-18 22:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.3±0.05ms     2.8 MB/sec    1.01     14.4±0.08ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    462.6±1.71µs     6.4 MB/sec    1.00    461.7±1.05µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.02ms     5.6 MB/sec    1.01      7.3±0.02ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1632.5±2.80µs    10.2 MB/sec    1.00   1629.5±3.27µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    177.8±0.40µs    16.6 MB/sec    1.01    179.6±0.58µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.6 MB/sec    1.00      3.3±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.01      5.8±0.00ms     7.1 MB/sec    1.00      5.7±0.00ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.01   1142.3±0.68µs    14.6 MB/sec    1.00   1133.7±2.22µs    14.7 MB/sec
parser/numpy/globals.py                    1.01    113.3±0.36µs    26.0 MB/sec    1.00    112.8±0.31µs    26.2 MB/sec
parser/pydantic/types.py                   1.01      2.5±0.00ms    10.2 MB/sec    1.00      2.5±0.00ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.2±0.08ms     2.5 MB/sec    1.01     16.3±0.07ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.03ms     3.9 MB/sec    1.01      4.3±0.03ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    436.5±6.48µs     6.8 MB/sec    1.00    438.4±5.46µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.07ms     3.7 MB/sec    1.01      7.0±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.03ms     5.0 MB/sec    1.02      8.3±0.05ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1774.7±10.96µs     9.4 MB/sec    1.03  1824.1±11.00µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.9±4.31µs    15.9 MB/sec    1.02    189.6±3.94µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     6.8 MB/sec    1.02      3.8±0.03ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.7±0.03ms     6.1 MB/sec    1.01      6.7±0.02ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1277.3±12.62µs    13.0 MB/sec    1.00  1277.7±10.42µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    127.1±1.06µs    23.2 MB/sec    1.01    128.0±1.19µs    23.1 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.02ms     9.0 MB/sec    1.00      2.8±0.02ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-04-18 22:40_

Ooh, are you seeing an error with this? I think it might be valid (or at least, it's intended to be valid).

---

_Comment by @KotlinIsland on 2023-04-18 22:41_

@charliermarsh hmmmmmmm, I'll investigate more then.

---

_Converted to draft by @KotlinIsland on 2023-04-18 22:41_

---

_Comment by @KotlinIsland on 2023-04-18 23:06_

Yeah, I have no idea what I was thinking...

---

_Closed by @KotlinIsland on 2023-04-18 23:06_

---

_Comment by @charliermarsh on 2023-04-18 23:07_

No worries, I always appreciate PRs to improve docs etc.

---

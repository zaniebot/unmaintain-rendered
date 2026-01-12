```yaml
number: 4745
title: Flag empty strings in flake8-errmsg rules
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/empty
created_at: 2023-05-31T04:18:01Z
updated_at: 2023-05-31T05:01:25Z
url: https://github.com/astral-sh/ruff/pull/4745
synced_at: 2026-01-12T03:50:03Z
```

# Flag empty strings in flake8-errmsg rules

---

_Pull request opened by @charliermarsh on 2023-05-31 04:18_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

It turns out that the check here should be `>=` per the [original plugin](https://github.com/henryiii/flake8-errmsg/blob/0a5dbc609bc2fe549f403b277375ef007414a462/src/flake8_errmsg/__init__.py#LL40C39-L40C53).

Closes #4740.


---

_Label `bug` added by @charliermarsh on 2023-05-31 04:18_

---

_Merged by @charliermarsh on 2023-05-31 04:37_

---

_Closed by @charliermarsh on 2023-05-31 04:37_

---

_Branch deleted on 2023-05-31 04:37_

---

_Comment by @github-actions[bot] on 2023-05-31 05:00_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+1, -0, 0 error(s))

<details><summary>bokeh (+1, -0)</summary>
<p>

```diff
+ src/bokeh/core/property/bases.py:554:30: EM101 [*] Exception must not use a string literal, assign to variable first
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| EM101 | 1 | 1 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.0±0.07ms     2.9 MB/sec    1.01     14.1±0.07ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    423.5±0.79µs     7.0 MB/sec    1.01    427.9±0.44µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.9±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     6.0 MB/sec    1.00      6.8±0.03ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1491.6±3.18µs    11.2 MB/sec    1.01   1506.1±4.69µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    168.7±0.31µs    17.5 MB/sec    1.03    173.1±0.81µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.2±0.00ms     7.9 MB/sec    1.00      5.1±0.00ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1022.9±3.64µs    16.3 MB/sec    1.00   1018.4±0.48µs    16.4 MB/sec
parser/numpy/globals.py                    1.01    106.0±0.12µs    27.8 MB/sec    1.00    105.5±0.27µs    28.0 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.4 MB/sec    1.00      2.2±0.00ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.20ms     2.4 MB/sec    1.00     16.6±0.20ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.01      4.2±0.06ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    499.7±8.20µs     5.9 MB/sec    1.00    500.7±7.29µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.11ms     3.6 MB/sec    1.00      7.0±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.10ms     5.0 MB/sec    1.00      8.2±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1757.2±28.31µs     9.5 MB/sec    1.00  1751.7±16.80µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.6±4.47µs    14.6 MB/sec    1.02    206.0±9.11µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.00      3.7±0.04ms     6.8 MB/sec
parser/large/dataset.py                    1.08      6.9±0.04ms     5.9 MB/sec    1.00      6.4±0.04ms     6.4 MB/sec
parser/numpy/ctypeslib.py                  1.07  1305.1±25.50µs    12.8 MB/sec    1.00  1221.6±13.08µs    13.6 MB/sec
parser/numpy/globals.py                    1.04    130.6±2.00µs    22.6 MB/sec    1.00    125.1±2.08µs    23.6 MB/sec
parser/pydantic/types.py                   1.07      2.9±0.04ms     8.7 MB/sec    1.00      2.7±0.03ms     9.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

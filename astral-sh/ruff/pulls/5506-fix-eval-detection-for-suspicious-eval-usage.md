```yaml
number: 5506
title: Fix eval detection for suspicious-eval-usage
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/eval
created_at: 2023-07-04T17:53:41Z
updated_at: 2023-07-04T18:31:05Z
url: https://github.com/astral-sh/ruff/pull/5506
synced_at: 2026-01-12T15:55:18Z
```

# Fix eval detection for suspicious-eval-usage

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/5505.

---

_Label `bug` added by @charliermarsh on 2023-07-04 17:53_

---

_Merged by @charliermarsh on 2023-07-04 18:01_

---

_Closed by @charliermarsh on 2023-07-04 18:01_

---

_Branch deleted on 2023-07-04 18:01_

---

_Comment by @github-actions[bot] on 2023-07-04 18:04_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+2, -0, 0 error(s))

<details><summary>airflow (+1, -0)</summary>
<p>

```diff
+ dev/stats/get_important_pr_candidates.py:143:27: S307 Use of possibly insecure function; consider using `ast.literal_eval`
```

</p>
</details>
<details><summary>disnake (+1, -0)</summary>
<p>

```diff
+ disnake/utils.py:1137:21: S307 Use of possibly insecure function; consider using `ast.literal_eval`
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| S307 | 2 | 2 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.5±0.02ms     4.3 MB/sec    1.00      9.4±0.04ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.1±0.00ms     7.9 MB/sec    1.00      2.1±0.01ms     8.0 MB/sec
formatter/numpy/globals.py                 1.01    236.5±1.39µs    12.5 MB/sec    1.00    234.5±0.54µs    12.6 MB/sec
formatter/pydantic/types.py                1.03      4.6±0.01ms     5.5 MB/sec    1.00      4.5±0.01ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.01     16.3±0.06ms     2.5 MB/sec    1.00     16.1±0.04ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.01ms     4.1 MB/sec    1.00      4.0±0.01ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    519.7±0.63µs     5.7 MB/sec    1.00    512.3±1.19µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.2±0.01ms     3.6 MB/sec    1.00      7.1±0.01ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.03      8.3±0.02ms     4.9 MB/sec    1.00      8.1±0.01ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03   1798.8±4.79µs     9.3 MB/sec    1.00   1753.6±1.84µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.03    205.1±1.78µs    14.4 MB/sec    1.00    198.9±0.53µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.7±0.01ms     6.8 MB/sec    1.00      3.6±0.01ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.14     14.8±0.48ms     2.7 MB/sec    1.00     13.0±0.53ms     3.1 MB/sec
formatter/numpy/ctypeslib.py               1.11      3.0±0.09ms     5.6 MB/sec    1.00      2.7±0.09ms     6.2 MB/sec
formatter/numpy/globals.py                 1.04   327.9±17.70µs     9.0 MB/sec    1.00   316.0±34.09µs     9.3 MB/sec
formatter/pydantic/types.py                1.13      6.7±0.27ms     3.8 MB/sec    1.00      5.9±0.22ms     4.3 MB/sec
linter/all-rules/large/dataset.py          1.01     22.4±0.56ms  1857.2 KB/sec    1.00     22.2±0.56ms  1877.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      5.8±0.33ms     2.9 MB/sec    1.00      5.6±0.25ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   668.2±19.63µs     4.4 MB/sec    1.00   668.2±22.41µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.7±0.36ms     2.6 MB/sec    1.01      9.7±0.38ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.00     11.3±0.39ms     3.6 MB/sec    1.00     11.4±0.36ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.3±0.06ms     7.1 MB/sec    1.00      2.3±0.07ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.01   283.5±11.19µs    10.4 MB/sec    1.00   281.9±11.16µs    10.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.0±0.15ms     5.1 MB/sec    1.01      5.0±0.18ms     5.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

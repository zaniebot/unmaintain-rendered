```yaml
number: 3878
title: "Consider logger candidate from `logging` module only"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: fix/logger-candidate-module
created_at: 2023-04-04T17:52:24Z
updated_at: 2023-04-05T01:28:55Z
url: https://github.com/astral-sh/ruff/pull/3878
synced_at: 2026-01-12T04:28:19Z
```

# Consider logger candidate from `logging` module only

---

_Pull request opened by @dhruvmanila on 2023-04-04 17:52_

Refer: https://github.com/charliermarsh/ruff/pull/3718#issuecomment-1496105349

cc @charliermarsh The code seems to be a bit messy (lot of branches). If you can take a look, it would be useful :)

---

_Comment by @github-actions[bot] on 2023-04-04 18:11_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -1, 0 error(s))

<details><summary>scikit-build (+0, -1)</summary>
<p>

```diff
- skbuild/utils/__init__.py:54:27: G010 [*] Logging statement uses `warn` instead of `warning`
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.1±0.05ms     2.7 MB/sec    1.00     15.1±0.05ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.00ms     4.4 MB/sec    1.00      3.8±0.01ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    409.9±2.09µs     7.2 MB/sec    1.00    409.4±1.28µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.01ms     3.9 MB/sec    1.00      6.5±0.01ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.01ms     5.2 MB/sec    1.01      7.9±0.02ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1732.8±2.30µs     9.6 MB/sec    1.01   1748.7±3.40µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.8±0.53µs    16.2 MB/sec    1.01    184.1±0.54µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.00ms     7.0 MB/sec    1.00      3.6±0.01ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.2±0.17ms     2.5 MB/sec    1.00     16.1±0.09ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.03ms     4.0 MB/sec    1.00      4.2±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    433.7±9.46µs     6.8 MB/sec    1.00    433.5±7.21µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.05ms     3.7 MB/sec    1.02      7.0±0.13ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.10ms     4.9 MB/sec    1.02      8.4±0.30ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1771.0±11.24µs     9.4 MB/sec    1.33      2.3±0.64ms     7.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.5±1.92µs    15.9 MB/sec    1.03   190.6±13.58µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.02ms     6.8 MB/sec    1.03      3.9±0.26ms     6.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @dhruvmanila on 2023-04-04 18:16_

I think we found the reason it didn't show up in the ecosystem check. The artifact contained that false positive which when diffed with the updated code still produced that warning, thus cancelling each other out.

---

_Merged by @charliermarsh on 2023-04-04 19:52_

---

_Closed by @charliermarsh on 2023-04-04 19:52_

---

_Branch deleted on 2023-04-05 01:28_

---

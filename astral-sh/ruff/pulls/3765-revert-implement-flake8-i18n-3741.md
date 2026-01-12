```yaml
number: 3765
title: "Revert \"Implement `flake8-i18n` (#3741)\""
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/revert
created_at: 2023-03-27T21:01:12Z
updated_at: 2023-03-27T21:32:57Z
url: https://github.com/astral-sh/ruff/pull/3765
synced_at: 2026-01-12T04:39:45Z
```

# Revert "Implement `flake8-i18n` (#3741)"

---

_Pull request opened by @charliermarsh on 2023-03-27 21:01_

Reverting #3741 while we figure out licensing questions.


---

_Merged by @charliermarsh on 2023-03-27 21:14_

---

_Closed by @charliermarsh on 2023-03-27 21:14_

---

_Branch deleted on 2023-03-27 21:14_

---

_Comment by @github-actions[bot] on 2023-03-27 21:32_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -1, 0 error(s))

<details><summary>zulip (+0, -1)</summary>
<p>

```diff
- corporate/lib/stripe.py:1010:17: INT001 f-string is resolved before function call; consider `_("string %s") % arg`
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.7±0.69ms     2.2 MB/sec    1.02     19.1±0.56ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.25ms     3.5 MB/sec    1.01      4.8±0.18ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.04   671.2±40.12µs     4.4 MB/sec    1.00   644.8±34.44µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.1±0.26ms     3.2 MB/sec    1.05      8.4±0.36ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.0±0.35ms     4.1 MB/sec    1.02     10.1±0.61ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.09ms     7.7 MB/sec    1.01      2.2±0.10ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.01   264.3±13.07µs    11.2 MB/sec    1.00   261.3±13.04µs    11.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.18ms     5.6 MB/sec    1.02      4.7±0.35ms     5.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.9±0.64ms     2.1 MB/sec    1.01     19.1±0.47ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.15ms     3.3 MB/sec    1.01      5.1±0.21ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.01   662.8±36.64µs     4.5 MB/sec    1.00   656.0±25.93µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.4±0.24ms     3.0 MB/sec    1.00      8.3±0.30ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00     10.1±0.50ms     4.0 MB/sec    1.02     10.3±0.55ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.09ms     7.4 MB/sec    1.01      2.3±0.11ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   258.9±12.14µs    11.4 MB/sec    1.03   266.1±15.77µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.29ms     5.3 MB/sec    1.00      4.8±0.17ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

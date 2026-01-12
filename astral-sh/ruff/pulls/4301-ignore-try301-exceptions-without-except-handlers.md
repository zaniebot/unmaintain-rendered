```yaml
number: 4301
title: "Ignore `TRY301` exceptions without except handlers"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/try301
created_at: 2023-05-09T03:33:19Z
updated_at: 2023-05-09T04:06:53Z
url: https://github.com/astral-sh/ruff/pull/4301
synced_at: 2026-01-12T03:56:39Z
```

# Ignore `TRY301` exceptions without except handlers

---

_Pull request opened by @charliermarsh on 2023-05-09 03:33_

Closes #4287.

---

_Renamed from "Ignore TRY301 exceptions without except handlers" to "Ignore `TRY301` exceptions without except handlers" by @charliermarsh on 2023-05-09 03:33_

---

_Merged by @charliermarsh on 2023-05-09 03:38_

---

_Closed by @charliermarsh on 2023-05-09 03:38_

---

_Branch deleted on 2023-05-09 03:38_

---

_Comment by @github-actions[bot] on 2023-05-09 03:42_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -9, 0 error(s))

<details><summary>airflow (+0, -7)</summary>
<p>

```diff
- airflow/jobs/local_task_job_runner.py:210:21: TRY301 Abstract `raise` to an inner function
- airflow/providers/amazon/aws/sensors/glue.py:80:17: TRY301 Abstract `raise` to an inner function
- airflow/providers/cncf/kubernetes/operators/pod.py:630:17: TRY301 Abstract `raise` to an inner function
- airflow/providers/docker/operators/docker.py:397:17: TRY301 Abstract `raise` to an inner function
- airflow/providers/docker/operators/docker.py:402:17: TRY301 Abstract `raise` to an inner function
- airflow/providers/microsoft/psrp/hooks/psrp.py:213:17: TRY301 Abstract `raise` to an inner function
- airflow/settings.py:251:17: TRY301 Abstract `raise` to an inner function
```

</p>
</details>
<details><summary>bokeh (+0, -1)</summary>
<p>

```diff
- src/bokeh/server/session.py:90:21: TRY301 Abstract `raise` to an inner function
```

</p>
</details>
<details><summary>zulip (+0, -1)</summary>
<p>

```diff
- tools/lib/test_server.py:91:21: TRY301 Abstract `raise` to an inner function
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.7±0.02ms     2.8 MB/sec    1.01     14.8±0.03ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    364.1±1.48µs     8.1 MB/sec    1.01    366.7±0.82µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.01      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.02ms     5.4 MB/sec    1.01      7.5±0.04ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1539.3±3.35µs    10.8 MB/sec    1.01   1554.5±4.67µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.2±0.82µs    17.9 MB/sec    1.01    167.1±1.11µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.06ms     7.7 MB/sec    1.01      3.3±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.00      5.9±0.01ms     6.9 MB/sec    1.00      5.9±0.01ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1150.2±1.71µs    14.5 MB/sec    1.00  1151.7±14.29µs    14.5 MB/sec
parser/numpy/globals.py                    1.00    117.2±0.31µs    25.2 MB/sec    1.00    117.0±0.42µs    25.2 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.01ms    10.1 MB/sec    1.00      2.5±0.00ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.3±0.15ms     2.5 MB/sec    1.00     16.3±0.98ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.07ms     4.0 MB/sec    1.00      4.1±0.08ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   489.3±10.76µs     6.0 MB/sec    1.00   489.8±11.24µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.07ms     3.7 MB/sec    1.00      6.9±0.09ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.07ms     4.9 MB/sec    1.01      8.3±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1731.2±18.54µs     9.6 MB/sec    1.03  1778.7±34.48µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.5±4.29µs    15.2 MB/sec    1.04    201.7±5.10µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     7.0 MB/sec    1.02      3.7±0.05ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.8±0.05ms     6.0 MB/sec    1.00      6.8±0.04ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.01  1298.1±22.52µs    12.8 MB/sec    1.00  1290.7±13.62µs    12.9 MB/sec
parser/numpy/globals.py                    1.01    133.6±4.04µs    22.1 MB/sec    1.00    132.2±2.06µs    22.3 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.03ms     8.8 MB/sec    1.00      2.9±0.02ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

```yaml
number: 3764
title: "[flake8-bugbear] Add more immutable functions for `B008`"
type: pull_request
state: merged
author: rouge8
labels: []
assignees: []
merged: true
base: main
head: flake8-bugbear-more-immutable-funcs
created_at: 2023-03-27T20:55:15Z
updated_at: 2023-03-28T14:50:11Z
url: https://github.com/astral-sh/ruff/pull/3764
synced_at: 2026-01-12T04:39:45Z
```

# [flake8-bugbear] Add more immutable functions for `B008`

---

_Pull request opened by @rouge8 on 2023-03-27 20:55_

- [flake8-bugbear] Allow `decimal.Decimal` in a function argument defaults for B008
- [flake8-bugbear] Allow `datetime.date()`, `datetime.datetime()`, and `datetime.timedelta()` in a function argument defaults for B008

ref: #3762


---

_Comment by @github-actions[bot] on 2023-03-27 21:32_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -5, 0 error(s))

<details><summary>airflow (+0, -3)</summary>
<p>

```diff
- airflow/providers/amazon/aws/operators/ecs.py:440:45: B008 Do not perform function call `timedelta` in argument defaults
- airflow/timetables/trigger.py:48:56: B008 Do not perform function call `datetime.timedelta` in argument defaults
- tests/ti_deps/deps/test_not_in_retry_period_dep.py:32:68: B008 Do not perform function call `timedelta` in argument defaults
```

</p>
</details>
<details><summary>bokeh (+0, -1)</summary>
<p>

```diff
- src/bokeh/core/property/datetime.py:160:60: B008 Do not perform function call `datetime.timedelta` in argument defaults
```

</p>
</details>
<details><summary>zulip (+0, -1)</summary>
<p>

```diff
- zerver/lib/send_email.py:345:33: B008 Do not perform function call `datetime.timedelta` in argument defaults
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.0±0.42ms     2.4 MB/sec    1.03     17.5±0.55ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.11ms     3.8 MB/sec    1.02      4.5±0.11ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.01   625.5±44.19µs     4.7 MB/sec    1.00   619.5±36.55µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.5±0.21ms     3.4 MB/sec    1.01      7.6±0.21ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.9±0.28ms     4.6 MB/sec    1.02      9.1±0.24ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1990.7±66.51µs     8.4 MB/sec    1.02      2.0±0.06ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    236.2±8.46µs    12.5 MB/sec    1.04    244.5±9.31µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.12ms     6.1 MB/sec    1.03      4.3±0.23ms     5.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     21.0±1.22ms  1986.7 KB/sec    1.01     21.1±0.75ms  1970.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.5±0.22ms     3.0 MB/sec    1.00      5.4±0.19ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   674.9±26.44µs     4.4 MB/sec    1.08   727.0±58.88µs     4.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.3±0.29ms     2.7 MB/sec    1.01      9.4±0.25ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.00     10.9±0.25ms     3.7 MB/sec    1.10     12.0±0.30ms     3.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.4±0.11ms     7.0 MB/sec    1.03      2.4±0.08ms     6.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   276.8±13.01µs    10.7 MB/sec    1.04   288.0±15.49µs    10.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.1±0.22ms     5.0 MB/sec    1.03      5.3±0.20ms     4.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-03-28 09:56_

---

_Merged by @charliermarsh on 2023-03-28 14:50_

---

_Closed by @charliermarsh on 2023-03-28 14:50_

---

_Comment by @charliermarsh on 2023-03-28 14:50_

Thanks!

---

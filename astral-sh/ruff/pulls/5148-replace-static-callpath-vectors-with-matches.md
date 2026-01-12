```yaml
number: 5148
title: "Replace static `CallPath` vectors with `matches!` macros"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/matches-more
created_at: 2023-06-16T15:19:11Z
updated_at: 2023-06-16T18:41:02Z
url: https://github.com/astral-sh/ruff/pull/5148
synced_at: 2026-01-12T03:43:30Z
```

# Replace static `CallPath` vectors with `matches!` macros

---

_Pull request opened by @charliermarsh on 2023-06-16 15:19_

## Summary

After #5140, I audited the codebase for similar patterns (defining a list of `CallPath` entities in a static vector, then looping over them to pattern-match). This PR migrates all other such cases to use `match` and `matches!` where possible.

There are a few benefits to this:

1. It more clearly denotes the intended semantics (branches are exclusive).
2. The compiler can help deduplicate the patterns and detect unreachable branches.
3. Performance: in the benchmark below, the all-rules performance is increased by nearly 10%...

## Benchmarks

I decided to benchmark against a large file in the Airflow repository with a lot of type annotations ([`views.py`](https://raw.githubusercontent.com/apache/airflow/f03f73100e8a7d6019249889de567cb00e71e457/airflow/www/views.py)):

```
linter/default-rules/airflow/views.py
                        time:   [10.871 ms 10.882 ms 10.894 ms]
                        thrpt:  [19.739 MiB/s 19.761 MiB/s 19.781 MiB/s]
                 change:
                        time:   [-2.7182% -2.5687% -2.4204%] (p = 0.00 < 0.05)
                        thrpt:  [+2.4805% +2.6364% +2.7942%]
                        Performance has improved.

linter/all-rules/airflow/views.py
                        time:   [24.021 ms 24.038 ms 24.062 ms]
                        thrpt:  [8.9373 MiB/s 8.9461 MiB/s 8.9527 MiB/s]
                 change:
                        time:   [-8.9537% -8.8516% -8.7527%] (p = 0.00 < 0.05)
                        thrpt:  [+9.5923% +9.7112% +9.8342%]
                        Performance has improved.
Found 12 outliers among 100 measurements (12.00%)
  5 (5.00%) high mild
  7 (7.00%) high severe
```

The impact is dramatic -- nearly a 10% improvement for `all-rules`.


---

_Comment by @github-actions[bot] on 2023-06-16 15:56_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+26, -0, 0 error(s))

<details><summary>airflow (+9, -0)</summary>
<p>

```diff
+ airflow/models/dag.py:3787:41: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ airflow/models/dag.py:3788:56: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ airflow/www/fab_security/manager.py:98:36: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ airflow/www/fab_security/manager.py:99:36: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ tests/always/test_project_structure.py:163:31: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ tests/always/test_project_structure.py:166:25: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ tests/always/test_project_structure.py:169:41: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ tests/always/test_project_structure.py:214:32: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ tests/always/test_project_structure.py:217:39: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
```

</p>
</details>
<details><summary>bokeh (+2, -0)</summary>
<p>

```diff
+ src/bokeh/document/models.py:68:32: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ tests/unit/bokeh/core/property/test_descriptors.py:238:32: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
```

</p>
</details>
<details><summary>zulip (+15, -0)</summary>
<p>

```diff
+ zerver/lib/test_classes.py:525:24: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ zerver/lib/test_classes.py:543:20: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ zerver/lib/test_classes.py:549:21: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ zerver/lib/test_classes.py:555:23: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ zerver/lib/test_classes.py:566:38: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ zerver/models.py:1666:38: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ zerver/models.py:1668:31: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ zerver/models.py:1687:36: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ zerver/models.py:1712:23: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ zerver/models.py:1724:52: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ zerver/models.py:2398:17: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ zerver/models.py:2598:32: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ zerver/models.py:728:64: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ zerver/tests/test_openapi.py:205:35: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
+ zerver/tests/test_openapi.py:283:47: RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| RUF012 | 26 | 26 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.8±0.01ms     6.0 MB/sec    1.01      6.9±0.02ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1401.1±3.82µs    11.9 MB/sec    1.01   1411.5±2.11µs    11.8 MB/sec
formatter/numpy/globals.py                 1.00    138.1±0.75µs    21.4 MB/sec    1.00    138.8±5.33µs    21.3 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.1 MB/sec    1.00      2.8±0.01ms     9.1 MB/sec
linter/all-rules/large/dataset.py          1.01     14.2±0.03ms     2.9 MB/sec    1.00     14.1±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.8 MB/sec    1.01      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    365.8±0.97µs     8.1 MB/sec    1.00    366.5±1.66µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.02ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.01ms     5.5 MB/sec    1.01      7.4±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1547.6±6.79µs    10.8 MB/sec    1.00   1540.8±3.00µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    167.4±0.59µs    17.6 MB/sec    1.00    165.1±0.34µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06      8.0±0.47ms     5.1 MB/sec    1.00      7.6±0.09ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.04  1626.2±88.99µs    10.2 MB/sec    1.00  1566.3±53.82µs    10.6 MB/sec
formatter/numpy/globals.py                 1.00    147.6±2.97µs    20.0 MB/sec    1.01    149.4±4.86µs    19.8 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.06ms     8.2 MB/sec    1.00      3.1±0.05ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.00     15.6±0.32ms     2.6 MB/sec    1.05     16.5±0.69ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.15ms     4.1 MB/sec    1.01      4.1±0.06ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    484.3±7.55µs     6.1 MB/sec    1.02   491.7±11.01µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.18ms     3.7 MB/sec    1.03      7.0±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.20ms     5.0 MB/sec    1.06      8.7±0.09ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1720.0±17.29µs     9.7 MB/sec    1.07  1838.5±60.50µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.2±3.18µs    15.1 MB/sec    1.06   207.5±12.70µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.08ms     6.9 MB/sec    1.07      3.9±0.14ms     6.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @charliermarsh on 2023-06-16 17:10_

---

_Review requested from @konstin by @charliermarsh on 2023-06-16 17:26_

---

_Merged by @charliermarsh on 2023-06-16 17:34_

---

_Closed by @charliermarsh on 2023-06-16 17:34_

---

_Branch deleted on 2023-06-16 17:34_

---

_Comment by @konstin on 2023-06-16 18:12_

is it intended that this contains functional changes?

---

_Comment by @charliermarsh on 2023-06-16 18:33_

No, will revisit. I thought the ecosystem CI check passed without issue, but it may have been from a late amendment.

---

_Comment by @charliermarsh on 2023-06-16 18:41_

Wait, sorry, yes, these are correct. We expanded the definition of "mutable attribute" to include calls like `dict()`, rather than just dict literals.

---

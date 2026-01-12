```yaml
number: 3634
title: Fix TRY300 false positive
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: fix-try300-false-positive
created_at: 2023-03-20T20:20:31Z
updated_at: 2023-03-20T22:24:58Z
url: https://github.com/astral-sh/ruff/pull/3634
synced_at: 2026-01-12T15:55:13Z
```

# Fix TRY300 false positive

---

_@JonathanPlasse_

- Close #3632

---

_@charliermarsh reviewed on 2023-03-20 20:29_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/tryceratops/rules/try_consider_else.rs`:20 on 2023-03-20 20:29_

Nit: let's pass `handlers`, so that the arguments here roughly match the "shape" of the AST node (and the n check `handlers.is_empty()` in here.

---

_Comment by @github-actions[bot] on 2023-03-20 20:32_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -5, 0 error(s))

<details><summary>airflow (+0, -3)</summary>
<p>

```diff
- airflow/jobs/local_task_job.py:203:13: TRY300 Consider moving this statement to an `else` block
- airflow/providers/docker/operators/docker.py:397:13: TRY300 Consider moving this statement to an `else` block
- dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:1503:9: TRY300 Consider moving this statement to an `else` block
```

</p>
</details>
<details><summary>bokeh (+0, -1)</summary>
<p>

```diff
- src/bokeh/server/session.py:107:13: TRY300 Consider moving this statement to an `else` block
```

</p>
</details>
<details><summary>zulip (+0, -1)</summary>
<p>

```diff
- zerver/lib/logging_util.py:95:13: TRY300 Consider moving this statement to an `else` block
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.5±0.21ms     2.6 MB/sec    1.02     15.8±0.20ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.05ms     4.1 MB/sec    1.02      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    567.5±4.52µs     5.2 MB/sec    1.00    569.0±5.34µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.08ms     3.7 MB/sec    1.00      7.0±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.09ms     4.8 MB/sec    1.01      8.5±0.08ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1934.4±37.03µs     8.6 MB/sec    1.00  1915.0±22.53µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    211.8±2.71µs    13.9 MB/sec    1.01    213.4±3.16µs    13.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.05ms     6.4 MB/sec    1.00      4.0±0.06ms     6.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.6±0.56ms     2.6 MB/sec    1.04     16.2±0.31ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.19ms     3.8 MB/sec    1.00      4.4±0.10ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   522.3±27.48µs     5.6 MB/sec    1.10   577.1±17.44µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.33ms     3.9 MB/sec    1.11      7.3±0.24ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.26ms     5.0 MB/sec    1.10      8.9±0.17ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1759.6±55.82µs     9.5 MB/sec    1.12  1962.2±55.53µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   213.9±41.04µs    13.8 MB/sec    1.05    224.1±9.57µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.21ms     6.6 MB/sec    1.08      4.1±0.12ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/tryceratops/rules/try_consider_else.rs`:20 on 2023-03-20 20:50_

Done.

---

_@JonathanPlasse reviewed on 2023-03-20 20:50_

---

_Merged by @charliermarsh on 2023-03-20 20:55_

---

_Closed by @charliermarsh on 2023-03-20 20:55_

---

_Branch deleted on 2023-03-20 22:24_

---

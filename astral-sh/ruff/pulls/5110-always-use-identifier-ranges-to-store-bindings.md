```yaml
number: 5110
title: Always use identifier ranges to store bindings
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/binding-identifier
created_at: 2023-06-15T03:56:56Z
updated_at: 2023-06-15T18:43:33Z
url: https://github.com/astral-sh/ruff/pull/5110
synced_at: 2026-01-12T03:43:30Z
```

# Always use identifier ranges to store bindings

---

_Pull request opened by @charliermarsh on 2023-06-15 03:56_

## Summary

At present, when we store a binding, we include a `TextRange` alongside it. The `TextRange` _sometimes_ matches the exact range of the identifier to which the `Binding` is linked, but... not always.

For example, given:

```python
x = 1
```

The binding we create _will_ use the range of `x`, because the left-hand side is an `Expr::Name`, which has a valid range on it.

However, given:

```python
try:
  pass
except ValueError as e:
  pass
```

When we create a binding for `e`, we don't have a `TextRange`... The AST doesn't give us one. So we end up extracting it via lexing.

This PR extends that pattern to the rest of the binding kinds, to ensure that whenever we create a binding, we always use the range of the bound name. This leads to better diagnostics in cases like pattern matching, whereby the diagnostic for "unused variable `x`" here used to include `*x`, instead of just `x`:

```python
def f(provided: int) -> int:
    match provided:
        case [_, *x]:
            pass
```

This is _also_ required for symbol renames, since we track writes as bindings -- so we need to know the ranges of the bound symbols.

By storing these bindings precisely, we can also remove the `binding.trimmed_range` abstraction -- since bindings already use the "trimmed range".

To implement this behavior, I took some of our existing utilities (like the code we had for `except ValueError as e` above), migrated them from a full lexer to a zero-allocation lexer that _only_ identifies "identifiers", and moved the behavior into a trait, so we can now do `stmt.identifier(locator)` to get the range for the identifier.

Honestly, we might end up discarding much of this if we decide to put ranges on all identifiers (https://github.com/astral-sh/RustPython-Parser/pull/8). But even if we do, this will _still_ be a good change, because the lexer introduced here is useful beyond names (e.g., we use it find the `except` keyword in an exception handler, to find the `else` after a `for` loop, and so on). So, I'm fine committing this even if we end up changing our minds about the right approach.

Closes #5090.

## Benchmarks

No significant change, with one statistically significant improvement (-2.1654% on `linter/all-rules/large/dataset.py`):

```
linter/default-rules/numpy/globals.py
                        time:   [73.922 µs 73.955 µs 73.986 µs]
                        thrpt:  [39.882 MiB/s 39.898 MiB/s 39.916 MiB/s]
                 change:
                        time:   [-0.5579% -0.4732% -0.3980%] (p = 0.00 < 0.05)
                        thrpt:  [+0.3996% +0.4755% +0.5611%]
                        Change within noise threshold.
Found 6 outliers among 100 measurements (6.00%)
  4 (4.00%) low severe
  1 (1.00%) low mild
  1 (1.00%) high mild
linter/default-rules/pydantic/types.py
                        time:   [1.4909 ms 1.4917 ms 1.4926 ms]
                        thrpt:  [17.087 MiB/s 17.096 MiB/s 17.106 MiB/s]
                 change:
                        time:   [+0.2140% +0.2741% +0.3392%] (p = 0.00 < 0.05)
                        thrpt:  [-0.3380% -0.2734% -0.2136%]
                        Change within noise threshold.
Found 4 outliers among 100 measurements (4.00%)
  3 (3.00%) high mild
  1 (1.00%) high severe
linter/default-rules/numpy/ctypeslib.py
                        time:   [688.97 µs 691.34 µs 694.15 µs]
                        thrpt:  [23.988 MiB/s 24.085 MiB/s 24.168 MiB/s]
                 change:
                        time:   [-1.3282% -0.7298% -0.1466%] (p = 0.02 < 0.05)
                        thrpt:  [+0.1468% +0.7351% +1.3461%]
                        Change within noise threshold.
Found 15 outliers among 100 measurements (15.00%)
  1 (1.00%) low mild
  2 (2.00%) high mild
  12 (12.00%) high severe
linter/default-rules/large/dataset.py
                        time:   [3.3872 ms 3.4032 ms 3.4191 ms]
                        thrpt:  [11.899 MiB/s 11.954 MiB/s 12.011 MiB/s]
                 change:
                        time:   [-0.6427% -0.2635% +0.0906%] (p = 0.17 > 0.05)
                        thrpt:  [-0.0905% +0.2642% +0.6469%]
                        No change in performance detected.
Found 20 outliers among 100 measurements (20.00%)
  1 (1.00%) low severe
  2 (2.00%) low mild
  4 (4.00%) high mild
  13 (13.00%) high severe

linter/all-rules/numpy/globals.py
                        time:   [148.99 µs 149.21 µs 149.42 µs]
                        thrpt:  [19.748 MiB/s 19.776 MiB/s 19.805 MiB/s]
                 change:
                        time:   [-0.7340% -0.5068% -0.2778%] (p = 0.00 < 0.05)
                        thrpt:  [+0.2785% +0.5094% +0.7395%]
                        Change within noise threshold.
Found 2 outliers among 100 measurements (2.00%)
  1 (1.00%) low mild
  1 (1.00%) high severe
linter/all-rules/pydantic/types.py
                        time:   [3.0362 ms 3.0396 ms 3.0441 ms]
                        thrpt:  [8.3779 MiB/s 8.3903 MiB/s 8.3997 MiB/s]
                 change:
                        time:   [-0.0957% +0.0618% +0.2125%] (p = 0.45 > 0.05)
                        thrpt:  [-0.2121% -0.0618% +0.0958%]
                        No change in performance detected.
Found 11 outliers among 100 measurements (11.00%)
  1 (1.00%) low severe
  3 (3.00%) low mild
  5 (5.00%) high mild
  2 (2.00%) high severe
linter/all-rules/numpy/ctypeslib.py
                        time:   [1.6879 ms 1.6894 ms 1.6909 ms]
                        thrpt:  [9.8478 MiB/s 9.8562 MiB/s 9.8652 MiB/s]
                 change:
                        time:   [-0.2279% -0.0888% +0.0436%] (p = 0.18 > 0.05)
                        thrpt:  [-0.0435% +0.0889% +0.2284%]
                        No change in performance detected.
Found 5 outliers among 100 measurements (5.00%)
  4 (4.00%) low mild
  1 (1.00%) high severe
linter/all-rules/large/dataset.py
                        time:   [7.1520 ms 7.1586 ms 7.1654 ms]
                        thrpt:  [5.6777 MiB/s 5.6831 MiB/s 5.6883 MiB/s]
                 change:
                        time:   [-2.5626% -2.1654% -1.7780%] (p = 0.00 < 0.05)
                        thrpt:  [+1.8102% +2.2133% +2.6300%]
                        Performance has improved.
Found 2 outliers among 100 measurements (2.00%)
  1 (1.00%) low mild
  1 (1.00%) high mild
```


---

_Comment by @github-actions[bot] on 2023-06-15 04:31_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+12, -12, 0 error(s))

<details><summary>airflow (+11, -11)</summary>
<p>

```diff
- airflow/callbacks/pipe_callback_sink.py:20:40: TCH003 [*] Move standard library import `multiprocessing.connection.Connection` into a type-checking block
+ airflow/callbacks/pipe_callback_sink.py:20:54: TCH003 [*] Move standard library import `multiprocessing.connection.Connection` into a type-checking block
- airflow/dag_processing/manager.py:36:40: TCH003 [*] Move standard library import `multiprocessing.connection.Connection` into a type-checking block
+ airflow/dag_processing/manager.py:36:54: TCH003 [*] Move standard library import `multiprocessing.connection.Connection` into a type-checking block
- airflow/dag_processing/processor.py:29:40: TCH003 [*] Move standard library import `multiprocessing.connection.Connection` into a type-checking block
+ airflow/dag_processing/processor.py:29:54: TCH003 [*] Move standard library import `multiprocessing.connection.Connection` into a type-checking block
- airflow/executors/kubernetes_executor.py:36:46: TCH002 [*] Move third-party import `kubernetes.client.models` into a type-checking block
+ airflow/executors/kubernetes_executor.py:36:56: TCH002 [*] Move third-party import `kubernetes.client.models` into a type-checking block
- airflow/kubernetes/k8s_model.py:23:31: TCH002 [*] Move third-party import `kubernetes.client.models` into a type-checking block
+ airflow/kubernetes/k8s_model.py:23:41: TCH002 [*] Move third-party import `kubernetes.client.models` into a type-checking block
- airflow/providers/elasticsearch/hooks/elasticsearch.py:29:39: TCH001 [*] Move application import `airflow.models.connection.Connection` into a type-checking block
+ airflow/providers/elasticsearch/hooks/elasticsearch.py:29:53: TCH001 [*] Move application import `airflow.models.connection.Connection` into a type-checking block
+ airflow/providers/exasol/hooks/exasol.py:23:18: TCH002 [*] Move third-party import `pandas` into a type-checking block
- airflow/providers/exasol/hooks/exasol.py:23:8: TCH002 [*] Move third-party import `pandas` into a type-checking block
- airflow/providers/google/cloud/transfers/presto_to_gcs.py:23:28: TCH002 [*] Move third-party import `prestodb.dbapi.Cursor` into a type-checking block
+ airflow/providers/google/cloud/transfers/presto_to_gcs.py:23:38: TCH002 [*] Move third-party import `prestodb.dbapi.Cursor` into a type-checking block
- airflow/providers/google/cloud/transfers/trino_to_gcs.py:23:25: TCH002 [*] Move third-party import `trino.dbapi.Cursor` into a type-checking block
+ airflow/providers/google/cloud/transfers/trino_to_gcs.py:23:35: TCH002 [*] Move third-party import `trino.dbapi.Cursor` into a type-checking block
+ airflow/providers/influxdb/hooks/influxdb.py:27:18: TCH002 [*] Move third-party import `pandas` into a type-checking block
- airflow/providers/influxdb/hooks/influxdb.py:27:8: TCH002 [*] Move third-party import `pandas` into a type-checking block
- airflow/serialization/serialized_objects.py:74:39: TCH004 [*] Move import `kubernetes.client.models` out of type-checking block. Import is used for more than type hinting.
+ airflow/serialization/serialized_objects.py:74:49: TCH004 [*] Move import `kubernetes.client.models` out of type-checking block. Import is used for more than type hinting.
```

</p>
</details>
<details><summary>bokeh (+1, -1)</summary>
<p>

```diff
+ src/bokeh/plotting/_figure.py:25:17: TCH002 [*] Move third-party import `numpy` into a type-checking block
- src/bokeh/plotting/_figure.py:25:8: TCH002 [*] Move third-party import `numpy` into a type-checking block
```

</p>
</details>
Rules changed: 4

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| TCH002 | 14 | 7 | 7 |
| TCH003 | 6 | 3 | 3 |
| TCH001 | 2 | 1 | 1 |
| TCH004 | 2 | 1 | 1 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.3±0.04ms     6.4 MB/sec    1.00      6.3±0.03ms     6.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   1320.8±4.02µs    12.6 MB/sec    1.00   1318.0±5.29µs    12.6 MB/sec
formatter/numpy/globals.py                 1.00    127.5±0.22µs    23.1 MB/sec    1.00    126.9±0.54µs    23.2 MB/sec
formatter/pydantic/types.py                1.01      2.6±0.03ms     9.9 MB/sec    1.00      2.6±0.01ms    10.0 MB/sec
linter/all-rules/large/dataset.py          1.01     13.7±0.16ms     3.0 MB/sec    1.00     13.5±0.15ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.02ms     5.0 MB/sec    1.00      3.3±0.02ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    414.3±3.33µs     7.1 MB/sec    1.00    411.5±0.60µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.02      5.8±0.06ms     4.4 MB/sec    1.00      5.7±0.03ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.04ms     6.2 MB/sec    1.00      6.6±0.05ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1431.3±3.52µs    11.6 MB/sec    1.00   1412.8±2.88µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.02    160.6±0.26µs    18.4 MB/sec    1.00    158.0±0.81µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.02ms     8.5 MB/sec    1.00      3.0±0.02ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.06ms     5.2 MB/sec    1.10      8.5±0.02ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1560.8±6.07µs    10.7 MB/sec    1.08  1693.2±10.24µs     9.8 MB/sec
formatter/numpy/globals.py                 1.00    155.6±3.80µs    19.0 MB/sec    1.06    164.6±1.46µs    17.9 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.02ms     8.1 MB/sec    1.10      3.5±0.03ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.00     16.0±0.04ms     2.5 MB/sec    1.01     16.1±0.04ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.02ms     4.0 MB/sec    1.00      4.2±0.02ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    429.7±5.47µs     6.9 MB/sec    1.01    433.0±7.27µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.02ms     3.6 MB/sec    1.01      7.1±0.02ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.01ms     4.9 MB/sec    1.00      8.3±0.03ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1716.7±6.48µs     9.7 MB/sec    1.00   1719.1±8.34µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.2±1.16µs    15.9 MB/sec    1.01    186.5±0.86µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     6.8 MB/sec    1.00      3.8±0.01ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-06-15 04:39_

---

_Review requested from @konstin by @charliermarsh on 2023-06-15 04:39_

---

_@charliermarsh reviewed on 2023-06-15 04:39_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/identifier.rs`:17 on 2023-06-15 04:39_

This file got collapsed, but it's really the one that needs to be reviewed. Most of the rest is just API migration.

---

_Review comment by @konstin on `crates/ruff_python_ast/src/helpers.rs`:1377 on 2023-06-15 09:09_

```suggestion
                        // e.g. `list()`
                        if args.is_empty() && keywords.is_empty() {
```

---

_Review comment by @konstin on `crates/ruff_python_ast/src/helpers.rs`:1380 on 2023-06-15 09:11_

```suggestion
                        // Truthiness depends on the inner iterable, e.g. `list({})` (falsey) or `list([1, 2, 3])` (truthy)
                        } else if args.len() == 1 && keywords.is_empty() {
```

---

_Review comment by @konstin on `crates/ruff_python_ast/src/identifier.rs`:192 on 2023-06-15 09:23_

```suggestion
                // From https://docs.python.org/3/reference/compound_stmts.html#grammar-token-python-grammar-mapping_pattern
                // > At most one double star pattern may be in a mapping pattern.
                // > The double star pattern must be the last subpattern in the mapping pattern.
                if let Some(pattern) = patterns.last() {
```

You might want to change the exact comment, but i expect many readers won't be familiar with what exactly the  `PatternMatchMapping` node entails

---

_Review comment by @konstin on `crates/ruff_python_ast/src/identifier.rs`:244 on 2023-06-15 09:24_

What if it's just `expect ValueError` without a name?

---

_Review comment by @konstin on `crates/ruff_python_ast/src/identifier.rs`:291 on 2023-06-15 09:27_

This seems to contradict https://docs.python.org/3/reference/lexical_analysis.html#identifiers, or is this a different kind of identifier?

---

_Review comment by @konstin on `crates/ruff_python_ast/src/identifier.rs`:382 on 2023-06-15 09:30_

Is this in the context of escaped newlines?

---

_Review comment by @konstin on `crates/ruff_python_ast/src/identifier.rs`:426 on 2023-06-15 09:35_

Why does this return EOF while bump returns an option

---

_@konstin approved on 2023-06-15 09:39_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/identifier.rs`:291 on 2023-06-15 16:40_

Fixed!

---

_@charliermarsh reviewed on 2023-06-15 16:40_

---

_@charliermarsh reviewed on 2023-06-15 16:40_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/identifier.rs`:382 on 2023-06-15 16:40_

No, these are intended to match continuation characters.

---

_@charliermarsh reviewed on 2023-06-15 16:48_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/identifier.rs`:426 on 2023-06-15 16:48_

This is consistent with how Micha designed it in the `trivia.rs` (from which this is derived), so I'm just gonna leave it for now.

---

_@konstin reviewed on 2023-06-15 17:15_

---

_Review comment by @konstin on `crates/ruff_python_ast/src/identifier.rs`:382 on 2023-06-15 17:15_

yep that's what i meant but didn't have the right term

---

_Merged by @charliermarsh on 2023-06-15 18:43_

---

_Closed by @charliermarsh on 2023-06-15 18:43_

---

_Branch deleted on 2023-06-15 18:43_

---

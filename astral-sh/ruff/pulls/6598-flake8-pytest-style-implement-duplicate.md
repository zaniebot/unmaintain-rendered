```yaml
number: 6598
title: "[`flake8-pytest-style`] Implement duplicate parameterized fixture detection (`PT014`)"
type: pull_request
state: merged
author: harupy
labels:
  - rule
assignees: []
merged: true
base: main
head: PT014
created_at: 2023-08-15T13:58:32Z
updated_at: 2023-08-16T03:57:03Z
url: https://github.com/astral-sh/ruff/pull/6598
synced_at: 2026-01-12T15:55:22Z
```

# [`flake8-pytest-style`] Implement duplicate parameterized fixture detection (`PT014`)

---

_@harupy_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

https://github.com/astral-sh/ruff/discussions/6597

## Test Plan

<!-- How was it tested? -->

Unit tests

---

_Review comment by @harupy on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:562 on 2023-08-15 13:59_

flake8-pytest-style's implementation:

https://github.com/m-burst/flake8-pytest-style/blob/1b0feb85b4e189242a42725e8a2045f0880598d9/flake8_pytest_style/visitors/parametrize.py#L103

```python
    def _check_parametrize_duplicates(
        self, node: ast.AST, values: Optional[ast.AST]
    ) -> None:
        """Checks for PT014."""
        if not isinstance(values, (ast.List, ast.Tuple, ast.Set)):
            return

        for (i, element1), (j, element2) in itertools.combinations(
            enumerate(values.elts, start=1), 2
        ):
            if check_equivalent_nodes(element1, element2):
                self.error_from_node(
                    DuplicateParametrizeTestCases, node, indexes=(i, j)
                )
```

---

_@harupy reviewed on 2023-08-15 13:59_

---

_@harupy reviewed on 2023-08-15 14:00_

---

_Review comment by @harupy on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:562 on 2023-08-15 14:00_

Should we replicate the copy original behavior?

---

_Review comment by @harupy on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:562 on 2023-08-15 14:10_

The original behavior seems nice because we can easily tell where duplicates are.

---

_@harupy reviewed on 2023-08-15 14:10_

---

_@harupy reviewed on 2023-08-15 14:14_

---

_Review comment by @harupy on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:562 on 2023-08-15 14:14_

The original implementation raises the error against the call node.

---

_Comment by @github-actions[bot] on 2023-08-15 14:31_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+23, -0, 0 error(s))

<details><summary>airflow (+12, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/1a828f393b220abbcd3a751bf98a6ec003776c1e/tests/always/test_secrets_local_filesystem.py#L103'>tests/always/test_secrets_local_filesystem.py:103:13:</a> PT014 Duplicate of test case at index 2 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/1a828f393b220abbcd3a751bf98a6ec003776c1e/tests/models/test_dagrun.py#L2537'>tests/models/test_dagrun.py:2537:9:</a> PT014 Duplicate of test case at index 5 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/1a828f393b220abbcd3a751bf98a6ec003776c1e/tests/operators/test_bash.py#L181'>tests/operators/test_bash.py:181:13:</a> PT014 Duplicate of test case at index 2 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/1a828f393b220abbcd3a751bf98a6ec003776c1e/tests/providers/cncf/kubernetes/operators/test_pod.py#L1214'>tests/providers/cncf/kubernetes/operators/test_pod.py:1214:13:</a> PT014 Duplicate of test case at index 2 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/1a828f393b220abbcd3a751bf98a6ec003776c1e/tests/providers/databricks/operators/test_databricks_sql.py#L233'>tests/providers/databricks/operators/test_databricks_sql.py:233:9:</a> PT014 Duplicate of test case at index 5 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/1a828f393b220abbcd3a751bf98a6ec003776c1e/tests/providers/docker/operators/test_docker.py#L533'>tests/providers/docker/operators/test_docker.py:533:13:</a> PT014 Duplicate of test case at index 2 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/1a828f393b220abbcd3a751bf98a6ec003776c1e/tests/providers/google/cloud/hooks/test_cloud_storage_transfer_service.py#L488'>tests/providers/google/cloud/hooks/test_cloud_storage_transfer_service.py:488:13:</a> PT014 Duplicate of test case at index 0 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/1a828f393b220abbcd3a751bf98a6ec003776c1e/tests/providers/google/cloud/hooks/test_cloud_storage_transfer_service.py#L489'>tests/providers/google/cloud/hooks/test_cloud_storage_transfer_service.py:489:13:</a> PT014 Duplicate of test case at index 1 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/1a828f393b220abbcd3a751bf98a6ec003776c1e/tests/providers/google/cloud/hooks/test_cloud_storage_transfer_service.py#L493'>tests/providers/google/cloud/hooks/test_cloud_storage_transfer_service.py:493:13:</a> PT014 Duplicate of test case at index 2 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/1a828f393b220abbcd3a751bf98a6ec003776c1e/tests/providers/presto/hooks/test_presto.py#L197'>tests/providers/presto/hooks/test_presto.py:197:13:</a> PT014 Duplicate of test case at index 2 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/1a828f393b220abbcd3a751bf98a6ec003776c1e/tests/providers/trino/hooks/test_trino.py#L216'>tests/providers/trino/hooks/test_trino.py:216:13:</a> PT014 Duplicate of test case at index 2 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/1a828f393b220abbcd3a751bf98a6ec003776c1e/tests/www/views/test_views.py#L534'>tests/www/views/test_views.py:534:9:</a> PT014 Duplicate of test case at index 4 in `@pytest_mark.parametrize`
</pre>

</p>
</details>
<details><summary>securedrop (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/f051a65ae47d080e339f108afbb518df70c4d7a9/molecule/testinfra/common/test_fpf_apt_repo.py#L64'>molecule/testinfra/common/test_fpf_apt_repo.py:64:9:</a> PT014 Duplicate of test case at index 1 in `@pytest_mark.parametrize`
</pre>

</p>
</details>
<details><summary>scikit-build (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build/blob/676e110315a971abb856edbd6df0c74293e5ba2d/tests/test_setup.py#L545'>tests/test_setup.py:545:9:</a> PT014 Duplicate of test case at index 3 in `@pytest_mark.parametrize`
</pre>

</p>
</details>
<details><summary>sphinx (+9, -0)</summary>
<p>

<pre>
+ 
+    |
+    |     ^^^^^^^^^^^^^^^^ PT014
+ 35 |     ("a * b", "a * b"),                         # Mult
+ 36 |     ("sys", "sys"),                             # Name, NameConstant
+ 37 |     ("1234", "1234"),                           # Num
+ 38 |     ("not a", "not a"),                         # Not
+ 39 |     ("a or b", "a or b"),                       # Or
+ <a href='https://github.com/sphinx-doc/sphinx/blob/eab54533a56119c5badd5aac647c595a9adae720/tests/test_pycode_ast.py#L37'>tests/test_pycode_ast.py:37:5:</a> PT014 Duplicate of test case at index 10 in `@pytest_mark.parametrize`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PT014 | 15 | 15 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.8±0.03ms    10.8 MB/sec    1.04      3.9±0.02ms    10.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   731.9±11.11µs    22.8 MB/sec    1.03   754.4±11.32µs    22.1 MB/sec
formatter/numpy/globals.py                 1.00     75.1±0.29µs    39.3 MB/sec    1.06     79.9±8.66µs    37.0 MB/sec
formatter/pydantic/types.py                1.00  1477.4±11.35µs    17.3 MB/sec    1.04  1530.4±12.56µs    16.7 MB/sec
linter/all-rules/large/dataset.py          1.01     10.7±0.09ms     3.8 MB/sec    1.00     10.7±0.05ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.01ms     5.7 MB/sec    1.00      2.9±0.01ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    327.7±0.78µs     9.0 MB/sec    1.00    326.4±1.65µs     9.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.04ms     4.6 MB/sec    1.00      5.5±0.03ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.01      5.7±0.05ms     7.2 MB/sec    1.00      5.6±0.04ms     7.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1192.1±3.23µs    14.0 MB/sec    1.00   1184.2±2.31µs    14.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    124.2±0.67µs    23.7 MB/sec    1.00    124.3±0.53µs    23.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.1 MB/sec    1.00      2.5±0.01ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.3±0.07ms     9.4 MB/sec    1.00      4.3±0.06ms     9.4 MB/sec
formatter/numpy/ctypeslib.py               1.01   845.4±11.96µs    19.7 MB/sec    1.00   837.0±12.85µs    19.9 MB/sec
formatter/numpy/globals.py                 1.01     85.9±1.64µs    34.4 MB/sec    1.00     85.2±3.03µs    34.6 MB/sec
formatter/pydantic/types.py                1.00  1688.0±23.15µs    15.1 MB/sec    1.02  1714.8±28.04µs    14.9 MB/sec
linter/all-rules/large/dataset.py          1.01     12.7±0.13ms     3.2 MB/sec    1.00     12.6±0.11ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.04ms     4.8 MB/sec    1.00      3.5±0.04ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.01   438.1±11.54µs     6.7 MB/sec    1.00    434.8±4.60µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.08ms     3.9 MB/sec    1.00      6.6±0.13ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.06ms     5.9 MB/sec    1.00      6.9±0.06ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1482.2±17.84µs    11.2 MB/sec    1.00  1473.3±18.77µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    175.2±4.52µs    16.8 MB/sec    1.01    176.4±5.08µs    16.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.2 MB/sec    1.00      3.1±0.04ms     8.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@harupy reviewed on 2023-08-15 14:36_

---

_Review comment by @harupy on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:562 on 2023-08-15 14:36_

Implemented the original behavior, but fixed the error range.

---

_@harupy reviewed on 2023-08-15 14:43_

---

_Review comment by @harupy on `crates/ruff/resources/test/fixtures/flake8_pytest_style/PT014.py`:1 on 2023-08-15 14:43_

flake8_pytest_style result:

```
> flake8 crates/ruff/resources/test/fixtures/flake8_pytest_style/PT014.py
crates/ruff/resources/test/fixtures/flake8_pytest_style/PT014.py:4:2: PT014 found duplicate test cases (1, 2) in @pytest.mark.parametrize
crates/ruff/resources/test/fixtures/flake8_pytest_style/PT014.py:14:2: PT014 found duplicate test cases (1, 2) in @pytest.mark.parametrize
crates/ruff/resources/test/fixtures/flake8_pytest_style/PT014.py:14:2: PT014 found duplicate test cases (3, 4) in @pytest.mark.parametrize
crates/ruff/resources/test/fixtures/flake8_pytest_style/PT014.py:19:2: PT014 found duplicate test cases (1, 2) in @pytest.mark.parametrize
```

---

_Marked ready for review by @harupy on 2023-08-15 14:43_

---

_Renamed from "Implement PT014" to "Implement `PT014`" by @harupy on 2023-08-15 15:34_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:220 on 2023-08-15 16:16_

should we say "at indices ..."? I wasn't sure what the message meant at first.

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:554 on 2023-08-15 16:17_

Should we use a `SmallVec` since it's unlikely for there to be many duplicates?

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:555 on 2023-08-15 16:18_

Is it feasible to factor this so the type of `values` is more meaningful/narrow?

---

_@zanieb approved on 2023-08-15 16:19_

Great work :) some minor comments

---

_@charliermarsh reviewed on 2023-08-15 17:06_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:554 on 2023-08-15 17:06_

I tend to prefer `Vec` unless we can demonstrate that `SmallVec` has better performance. In this case, we probably don't expect _any_ duplicates in general, in which case this will never allocate anyway, so seems okay in my opinion.

---

_@harupy reviewed on 2023-08-15 17:12_

---

_Review comment by @harupy on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:220 on 2023-08-15 17:12_

Can we raise PT014 against items that should be removed?

```
# Example

[1, 1, 2]
    ^ PT014 ...
```

This allows us to remove indices.

---

_@charliermarsh reviewed on 2023-08-15 17:14_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:559 on 2023-08-15 17:14_

I think I'd suggest using an `FxHashSet` or `FxHashMap` here... As-is, this is quadratic, since we're checking every value against every other value. It's sometimes better to use a vector if you know the number of items is really small, but this also means we're re-computing the hash many times over.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:220 on 2023-08-15 22:29_

I think it makes sense to use the range of the duplicated item, so that we underline the duplicated item specifically

---

_@charliermarsh reviewed on 2023-08-15 22:29_

---

_@harupy reviewed on 2023-08-15 23:13_

---

_Review comment by @harupy on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:220 on 2023-08-15 23:13_

```
# Example

[100, 100, 100, 200]
      ^^^  ^^^
```

If we have multiple duplicated items, we underline them?

---

_@zanieb reviewed on 2023-08-15 23:20_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:220 on 2023-08-15 23:20_

I don't think we support highlighting multiple ranges. In that case, they'd each need to be a new violation which seems okay.

---

_@charliermarsh reviewed on 2023-08-15 23:24_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:220 on 2023-08-15 23:24_

I think in the duplicate value rule, we just highlight the _second_ value (i.e., the one that is a duplicate). That seems reasonable to me. (We could also mention the index of which it's a duplicate in the message.)

---

_@harupy reviewed on 2023-08-15 23:26_

---

_Review comment by @harupy on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:220 on 2023-08-15 23:26_

```
# Example

[100, 100, 100, 200]
      ^^^ PT014: duplicate of item at {0}
```

so it should look like this?

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:220 on 2023-08-15 23:32_

Yeah, and the same for the next 100 -- one violation for each duplicate (but no violation for the first `100`), would be my suggestion.

---

_@charliermarsh reviewed on 2023-08-15 23:32_

---

_Review requested from @charliermarsh by @harupy on 2023-08-16 02:02_

---

_Review requested from @zanieb by @harupy on 2023-08-16 02:02_

---

_Label `rule` added by @charliermarsh on 2023-08-16 03:27_

---

_Renamed from "Implement `PT014`" to "[`flake8-pytest-style`] Implement duplicate parameterized fixture detection (`PT014`)" by @charliermarsh on 2023-08-16 03:27_

---

_Merged by @charliermarsh on 2023-08-16 03:35_

---

_Closed by @charliermarsh on 2023-08-16 03:35_

---

_Branch deleted on 2023-08-16 03:40_

---

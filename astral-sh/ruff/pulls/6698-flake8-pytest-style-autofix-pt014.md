```yaml
number: 6698
title: "[`flake8-pytest-style`] Autofix `PT014`"
type: pull_request
state: merged
author: harupy
labels:
  - fixes
assignees: []
merged: true
base: main
head: autofix-PT014
created_at: 2023-08-20T03:47:25Z
updated_at: 2023-08-21T03:59:17Z
url: https://github.com/astral-sh/ruff/pull/6698
synced_at: 2026-01-12T02:52:04Z
```

# [`flake8-pytest-style`] Autofix `PT014`

---

_Pull request opened by @harupy on 2023-08-20 03:47_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-08-20 03:59_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+14, -13, 0 error(s))

<details><summary>airflow (+11, -11)</summary>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/always/test_secrets_local_filesystem.py#L103'>tests/always/test_secrets_local_filesystem.py:103:13:</a> PT014 Duplicate of test case at index 2 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/always/test_secrets_local_filesystem.py#L103'>tests/always/test_secrets_local_filesystem.py:103:13:</a> PT014 [*] Duplicate of test case at index 2 in `@pytest_mark.parametrize`
- <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/operators/test_bash.py#L181'>tests/operators/test_bash.py:181:13:</a> PT014 Duplicate of test case at index 2 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/operators/test_bash.py#L181'>tests/operators/test_bash.py:181:13:</a> PT014 [*] Duplicate of test case at index 2 in `@pytest_mark.parametrize`
- <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/providers/cncf/kubernetes/operators/test_pod.py#L1214'>tests/providers/cncf/kubernetes/operators/test_pod.py:1214:13:</a> PT014 Duplicate of test case at index 2 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/providers/cncf/kubernetes/operators/test_pod.py#L1214'>tests/providers/cncf/kubernetes/operators/test_pod.py:1214:13:</a> PT014 [*] Duplicate of test case at index 2 in `@pytest_mark.parametrize`
- <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/providers/databricks/operators/test_databricks_sql.py#L232'>tests/providers/databricks/operators/test_databricks_sql.py:232:9:</a> PT014 Duplicate of test case at index 5 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/providers/databricks/operators/test_databricks_sql.py#L232'>tests/providers/databricks/operators/test_databricks_sql.py:232:9:</a> PT014 [*] Duplicate of test case at index 5 in `@pytest_mark.parametrize`
- <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/providers/docker/operators/test_docker.py#L533'>tests/providers/docker/operators/test_docker.py:533:13:</a> PT014 Duplicate of test case at index 2 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/providers/docker/operators/test_docker.py#L533'>tests/providers/docker/operators/test_docker.py:533:13:</a> PT014 [*] Duplicate of test case at index 2 in `@pytest_mark.parametrize`
- <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/providers/google/cloud/hooks/test_cloud_storage_transfer_service.py#L488'>tests/providers/google/cloud/hooks/test_cloud_storage_transfer_service.py:488:13:</a> PT014 Duplicate of test case at index 0 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/providers/google/cloud/hooks/test_cloud_storage_transfer_service.py#L488'>tests/providers/google/cloud/hooks/test_cloud_storage_transfer_service.py:488:13:</a> PT014 [*] Duplicate of test case at index 0 in `@pytest_mark.parametrize`
- <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/providers/google/cloud/hooks/test_cloud_storage_transfer_service.py#L489'>tests/providers/google/cloud/hooks/test_cloud_storage_transfer_service.py:489:13:</a> PT014 Duplicate of test case at index 1 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/providers/google/cloud/hooks/test_cloud_storage_transfer_service.py#L489'>tests/providers/google/cloud/hooks/test_cloud_storage_transfer_service.py:489:13:</a> PT014 [*] Duplicate of test case at index 1 in `@pytest_mark.parametrize`
- <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/providers/google/cloud/hooks/test_cloud_storage_transfer_service.py#L493'>tests/providers/google/cloud/hooks/test_cloud_storage_transfer_service.py:493:13:</a> PT014 Duplicate of test case at index 2 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/providers/google/cloud/hooks/test_cloud_storage_transfer_service.py#L493'>tests/providers/google/cloud/hooks/test_cloud_storage_transfer_service.py:493:13:</a> PT014 [*] Duplicate of test case at index 2 in `@pytest_mark.parametrize`
- <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/providers/presto/hooks/test_presto.py#L197'>tests/providers/presto/hooks/test_presto.py:197:13:</a> PT014 Duplicate of test case at index 2 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/providers/presto/hooks/test_presto.py#L197'>tests/providers/presto/hooks/test_presto.py:197:13:</a> PT014 [*] Duplicate of test case at index 2 in `@pytest_mark.parametrize`
- <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/providers/trino/hooks/test_trino.py#L216'>tests/providers/trino/hooks/test_trino.py:216:13:</a> PT014 Duplicate of test case at index 2 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/providers/trino/hooks/test_trino.py#L216'>tests/providers/trino/hooks/test_trino.py:216:13:</a> PT014 [*] Duplicate of test case at index 2 in `@pytest_mark.parametrize`
- <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/www/views/test_views.py#L534'>tests/www/views/test_views.py:534:9:</a> PT014 Duplicate of test case at index 4 in `@pytest_mark.parametrize`
+ <a href='https://github.com/apache/airflow/blob/46fa5a2743c0c864f5282abd6055c5418585955b/tests/www/views/test_views.py#L534'>tests/www/views/test_views.py:534:9:</a> PT014 [*] Duplicate of test case at index 4 in `@pytest_mark.parametrize`
</pre>

</p>
</details>
<details><summary>securedrop (+1, -1)</summary>
<p>

<pre>
- <a href='https://github.com/freedomofpress/securedrop/blob/7eb784e3355de3400336adadce4915273e004671/molecule/testinfra/common/test_fpf_apt_repo.py#L64'>molecule/testinfra/common/test_fpf_apt_repo.py:64:9:</a> PT014 Duplicate of test case at index 1 in `@pytest_mark.parametrize`
+ <a href='https://github.com/freedomofpress/securedrop/blob/7eb784e3355de3400336adadce4915273e004671/molecule/testinfra/common/test_fpf_apt_repo.py#L64'>molecule/testinfra/common/test_fpf_apt_repo.py:64:9:</a> PT014 [*] Duplicate of test case at index 1 in `@pytest_mark.parametrize`
</pre>

</p>
</details>
<details><summary>scikit-build (+1, -1)</summary>
<p>

<pre>
- <a href='https://github.com/scikit-build/scikit-build/blob/676e110315a971abb856edbd6df0c74293e5ba2d/tests/test_setup.py#L545'>tests/test_setup.py:545:9:</a> PT014 Duplicate of test case at index 3 in `@pytest_mark.parametrize`
+ <a href='https://github.com/scikit-build/scikit-build/blob/676e110315a971abb856edbd6df0c74293e5ba2d/tests/test_setup.py#L545'>tests/test_setup.py:545:9:</a> PT014 [*] Duplicate of test case at index 3 in `@pytest_mark.parametrize`
</pre>

</p>
</details>
<details><summary>sphinx (+1, -0)</summary>
<p>

<pre>
+    = help: Remove duplicate test case
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PT014 | 26 | 13 | 13 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.2±0.03ms    12.6 MB/sec    1.01      3.3±0.02ms    12.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   668.4±15.48µs    24.9 MB/sec    1.01   676.1±16.63µs    24.6 MB/sec
formatter/numpy/globals.py                 1.00     69.2±0.37µs    42.6 MB/sec    1.01     70.2±0.32µs    42.0 MB/sec
formatter/pydantic/types.py                1.00  1316.9±28.50µs    19.4 MB/sec    1.01  1325.1±29.29µs    19.2 MB/sec
linter/all-rules/large/dataset.py          1.00     10.5±0.07ms     3.9 MB/sec    1.00     10.4±0.06ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.01ms     5.8 MB/sec    1.00      2.9±0.02ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    319.5±1.68µs     9.2 MB/sec    1.00    319.6±1.24µs     9.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.04ms     4.7 MB/sec    1.00      5.4±0.03ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.01      5.6±0.02ms     7.3 MB/sec    1.00      5.6±0.01ms     7.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1185.5±2.64µs    14.0 MB/sec    1.00   1182.7±3.18µs    14.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    122.9±0.21µs    24.0 MB/sec    1.01    123.6±1.15µs    23.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.1 MB/sec    1.01      2.5±0.03ms    10.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.7±0.05ms    10.9 MB/sec    1.01      3.8±0.06ms    10.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   763.9±17.69µs    21.8 MB/sec    1.01   769.0±13.43µs    21.7 MB/sec
formatter/numpy/globals.py                 1.00     78.4±1.14µs    37.6 MB/sec    1.02     79.9±1.79µs    36.9 MB/sec
formatter/pydantic/types.py                1.00  1531.7±22.02µs    16.7 MB/sec    1.02  1557.2±40.76µs    16.4 MB/sec
linter/all-rules/large/dataset.py          1.00     12.6±0.12ms     3.2 MB/sec    1.00     12.6±0.14ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.09ms     4.8 MB/sec    1.00      3.5±0.03ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    437.1±8.46µs     6.8 MB/sec    1.00    436.1±5.44µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.07ms     3.9 MB/sec    1.00      6.6±0.08ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.07ms     5.8 MB/sec    1.00      7.1±0.07ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1486.3±16.48µs    11.2 MB/sec    1.01  1498.3±15.39µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.8±3.42µs    17.1 MB/sec    1.01    174.5±2.74µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.04ms     8.1 MB/sec    1.01      3.2±0.04ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Autofix `PT014`" to "[`[flake8-pytest-style]`] Autofix `PT014`" by @harupy on 2023-08-20 04:36_

---

_Renamed from "[`[flake8-pytest-style]`] Autofix `PT014`" to "[`flake8-pytest-style`] Autofix `PT014`" by @harupy on 2023-08-20 07:39_

---

_@charliermarsh reviewed on 2023-08-20 14:19_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:573 on 2023-08-20 14:19_

I think this should either be optional _or_ we should insert it into `seen` outside of the loop and skip the first element.

---

_@charliermarsh reviewed on 2023-08-20 14:19_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:567 on 2023-08-20 14:19_

In general I'd probably prefer something like `let [mut prev, ..] = elts else { return; };`.

---

_@charliermarsh reviewed on 2023-08-20 14:20_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:567 on 2023-08-20 14:20_

It better couples the condition to the access, whereas here, we have the condition up here, then the potentially-unsafe `elts[0]` below.

---

_@charliermarsh reviewed on 2023-08-20 15:03_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pytest_style/rules/parametrize.rs`:585 on 2023-08-20 15:03_

I think this won't quite do the right thing if one of the expressions is parenthesized (the range won't include the parentheses). We need to do something similar to `remove_argument`.

---

_Review requested from @charliermarsh by @charliermarsh on 2023-08-20 17:29_

---

_Label `autofix` added by @charliermarsh on 2023-08-21 03:35_

---

_Merged by @charliermarsh on 2023-08-21 03:45_

---

_Closed by @charliermarsh on 2023-08-21 03:45_

---

_Branch deleted on 2023-08-21 03:45_

---

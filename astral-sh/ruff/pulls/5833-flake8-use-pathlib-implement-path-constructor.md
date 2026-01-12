```yaml
number: 5833
title: "[`flake8-use-pathlib`] Implement `path-constructor-default-argument` (`PTH201`)"
type: pull_request
state: merged
author: sbrugman
labels:
  - rule
assignees: []
merged: true
base: main
head: rule-path-constructor-default-argument
created_at: 2023-07-17T14:12:53Z
updated_at: 2023-07-20T08:42:22Z
url: https://github.com/astral-sh/ruff/pull/5833
synced_at: 2026-01-12T03:30:21Z
```

# [`flake8-use-pathlib`] Implement `path-constructor-default-argument` (`PTH201`)

---

_Pull request opened by @sbrugman on 2023-07-17 14:12_

Reviving https://github.com/astral-sh/ruff/pull/2348 step by step

Pt 2. PTH201: Path Constructor Default Argument

- rule originates from `refurb`: https://github.com/charliermarsh/ruff/issues/1348
- Using PTH201 rather than FURBXXX to keep all pathlib logic together


---

_Comment by @github-actions[bot] on 2023-07-17 14:25_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+4, -0, 0 error(s))

<details><summary>airflow (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/7092cfdbbfcfd3c03909229daa741a5bcd7ccc64/airflow/models/dag.py#L1297'>airflow/models/dag.py:1297:41:</a> PTH201 [*] Do not pass the current directory explicitly to `Path`
</pre>

</p>
</details>
<details><summary>scikit-build-core (+3, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/6e18ece11af457c322ebb1733e304aec473925d3/src/scikit_build_core/build/sdist.py#L103'>src/scikit_build_core/build/sdist.py:103:22:</a> PTH201 [*] Do not pass the current directory explicitly to `Path`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/6e18ece11af457c322ebb1733e304aec473925d3/src/scikit_build_core/build/wheel.py#L46'>src/scikit_build_core/build/wheel.py:46:41:</a> PTH201 [*] Do not pass the current directory explicitly to `Path`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/6e18ece11af457c322ebb1733e304aec473925d3/tests/test_file_processor.py#L48'>tests/test_file_processor.py:48:44:</a> PTH201 [*] Do not pass the current directory explicitly to `Path`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PTH201 | 4 | 4 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.6±0.31ms     3.9 MB/sec    1.01     10.7±0.30ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.06ms     8.0 MB/sec    1.04      2.2±0.06ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    244.4±4.62µs    12.1 MB/sec    1.02    248.7±3.56µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.12ms     5.6 MB/sec    1.03      4.7±0.09ms     5.5 MB/sec
linter/all-rules/large/dataset.py          1.02     14.8±0.50ms     2.7 MB/sec    1.00     14.5±0.83ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.16ms     4.5 MB/sec    1.02      3.8±0.08ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.04   507.8±10.42µs     5.8 MB/sec    1.00   487.4±15.76µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.22ms     3.8 MB/sec    1.00      6.7±0.24ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.02      7.6±0.21ms     5.4 MB/sec    1.00      7.4±0.22ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1614.5±49.26µs    10.3 MB/sec    1.00  1582.1±56.42µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    177.7±6.69µs    16.6 MB/sec    1.00    178.2±3.88µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.14ms     7.5 MB/sec    1.00      3.4±0.06ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.2±0.25ms     3.6 MB/sec    1.00     11.3±0.33ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.05ms     7.6 MB/sec    1.00      2.2±0.06ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00   251.9±12.01µs    11.7 MB/sec    1.01   255.0±13.15µs    11.6 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.13ms     5.3 MB/sec    1.01      4.8±0.15ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.8±0.31ms     2.6 MB/sec    1.02     16.1±0.69ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.08ms     4.1 MB/sec    1.00      4.1±0.07ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    495.3±8.69µs     6.0 MB/sec    1.00   496.7±15.21µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.2±0.19ms     3.5 MB/sec    1.00      7.1±0.12ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.18ms     5.0 MB/sec    1.00      8.1±0.13ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1699.4±36.97µs     9.8 MB/sec    1.01  1714.1±30.30µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.1±3.93µs    15.4 MB/sec    1.00    191.6±5.41µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.08ms     7.0 MB/sec    1.00      3.6±0.06ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @sbrugman on 2023-07-17 14:28_

Ecosystem checks are true positives

---

_Renamed from "[`flake8-use-pathlib`] path constructor default argument" to "[`flake8-use-pathlib`] Implement `path-constructor-default-argument` (`PTH201`)" by @sbrugman on 2023-07-17 14:56_

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_use_pathlib/rules/path_constructor_current_directory.rs`:9 on 2023-07-18 13:26_

```suggestion
/// Checks for pathlib `Path` objects that are initialized with the current directory. 
```

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_use_pathlib/rules/path_constructor_current_directory.rs`:12 on 2023-07-18 13:27_

nit: we wrap documentation at 80 characters
```suggestion
/// The `Path()` constructor defaults to the current directory. There is no
/// need to pass the current directory (`"."`) explicitly.
```

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_use_pathlib/rules/path_constructor_current_directory.rs`:40 on 2023-07-18 13:28_

```suggestion
        "Remove the current directory constructor argument".to_string()
```

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_use_pathlib/rules/path_constructor_current_directory.rs`:56 on 2023-07-18 13:31_

nit: use let-else to avoid indentation like
```suggestion
    let Expr::Call(ExprCall { args, keywords, .. }) = expr else {
        return;
    }
```

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_use_pathlib/rules/path_constructor_current_directory.rs`:60 on 2023-07-18 13:33_

We can avoid the unwrap using pattern matching
```suggestion
        if !keywords.is_empty() {
            return;
        }
        let [arg] = args.as_slice() else {
            return;
        }
```
You might even be able to merge it with the the next if-let

---

_@konstin approved on 2023-07-18 13:36_

---

_Label `rule` added by @charliermarsh on 2023-07-20 01:41_

---

_Merged by @charliermarsh on 2023-07-20 01:50_

---

_Closed by @charliermarsh on 2023-07-20 01:50_

---

_Branch deleted on 2023-07-20 08:42_

---

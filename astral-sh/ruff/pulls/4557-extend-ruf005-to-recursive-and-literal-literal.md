```yaml
number: 4557
title: "Extend `RUF005` to recursive and literal-literal concatenations"
type: pull_request
state: merged
author: hoel-bagard
labels: []
assignees: []
merged: true
base: main
head: fix-RUF005
created_at: 2023-05-21T03:44:25Z
updated_at: 2023-05-24T01:42:10Z
url: https://github.com/astral-sh/ruff/pull/4557
synced_at: 2026-01-12T03:50:03Z
```

# Extend `RUF005` to recursive and literal-literal concatenations

---

_Pull request opened by @hoel-bagard on 2023-05-21 03:44_

This attempts to fix https://github.com/charliermarsh/ruff/issues/3496.

**It introduces the following changes:**
- The building of the new suggested expression is moved to a recursive function to handle cases with multiple concatenations on a single line.
- The rule now also checks for concatenations of lists/tuples (for example `a = [1] + [2]`).  (I'm not sure if this behavior is desired, it's not directly related to the issue).
- New tests to check the new behavior.

---

_Comment by @github-actions[bot] on 2023-05-21 03:56_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+10, -5, 0 error(s))

<details><summary>airflow (+9, -5)</summary>
<p>

```diff
+ airflow/providers/apache/beam/hooks/beam.py:262:30: RUF005 [*] Consider `[py_interpreter, *py_options, py_file]` instead of concatenation
- airflow/providers/apache/beam/hooks/beam.py:262:30: RUF005 [*] Consider `[py_interpreter, *py_options]` instead of concatenation
+ airflow/providers/apache/hive/hooks/hive.py:170:16: RUF005 [*] Consider `[hive_bin, *cmd_extra, *hive_params_list]` instead of concatenation
- airflow/providers/apache/hive/hooks/hive.py:170:16: RUF005 [*] Consider `[hive_bin, *cmd_extra]` instead of concatenation
+ airflow/providers/apache/hive/operators/hive_stats.py:172:13: RUF005 [*] Consider `(self.ds, self.dttm, self.table, part_json, r[0][0], r[0][1], r[1])` instead of concatenation
+ airflow/utils/python_virtualenv.py:43:11: RUF005 [*] Consider `[f"{tmp_dir}/bin/pip", "install", *pip_install_options, "-r"]` instead of concatenation
- airflow/utils/python_virtualenv.py:43:11: RUF005 [*] Consider `[f"{tmp_dir}/bin/pip", "install", *pip_install_options]` instead of concatenation
+ tests/providers/amazon/aws/hooks/test_eks.py:1170:17: RUF005 Consider `[POD_EXECUTION_ROLE_ARN, (ClusterAttributes.CLUSTER_NAME, cluster_name), (FargateProfileAttributes.FARGATE_PROFILE_NAME, fargate_profile_name), (FargateProfileAttributes.SELECTORS, selectors)]` instead of concatenation
- tests/providers/amazon/aws/hooks/test_eks.py:754:17: RUF005 Consider `[*NodegroupInputs.REQUIRED, (ClusterAttributes.CLUSTER_NAME, generated_test_data.existing_cluster_name), (NodegroupAttributes.NODEGROUP_NAME, nodegroup_name)]` instead of concatenation
+ tests/providers/apache/hive/operators/test_hive_stats.py:155:13: RUF005 [*] Consider `(hive_stats_collection_operator.ds, hive_stats_collection_operator.dttm, hive_stats_collection_operator.table, mock_json_dumps.return_value, r[0][0], r[0][1], r[1])` instead of concatenation
+ tests/providers/apache/hive/operators/test_hive_stats.py:203:13: RUF005 [*] Consider `(hive_stats_collection_operator.ds, hive_stats_collection_operator.dttm, hive_stats_collection_operator.table, mock_json_dumps.return_value, r[0][0], r[0][1], r[1])` instead of concatenation
+ tests/providers/apache/hive/operators/test_hive_stats.py:251:13: RUF005 [*] Consider `(hive_stats_collection_operator.ds, hive_stats_collection_operator.dttm, hive_stats_collection_operator.table, mock_json_dumps.return_value, r[0][0], r[0][1], r[1])` instead of concatenation
+ tests/utils/test_python_virtualenv.py:64:13: RUF005 [*] Consider `["/VENV/bin/pip", "install", *pip_install_options, "apache-beam[gcp]"]` instead of concatenation
- tests/utils/test_python_virtualenv.py:64:13: RUF005 [*] Consider `["/VENV/bin/pip", "install", *pip_install_options]` instead of concatenation
```

</p>
</details>
<details><summary>scikit-build (+1, -0)</summary>
<p>

```diff
+ tests/test_issue342_cmake_osx_args_in_setup.py:178:44: RUF005 [*] Consider `["build", *cli_setup_args, "--", *cli_cmake_args]` instead of concatenation
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| RUF005 | 15 | 10 | 5 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.22ms     2.4 MB/sec    1.00     16.7±0.15ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.02ms     4.2 MB/sec    1.00      4.0±0.04ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    494.0±2.97µs     6.0 MB/sec    1.01   499.0±18.17µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.05ms     3.7 MB/sec    1.00      6.8±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.04ms     5.1 MB/sec    1.00      8.0±0.09ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1722.6±19.07µs     9.7 MB/sec    1.00  1714.7±18.93µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.01   207.8±11.11µs    14.2 MB/sec    1.00    205.3±9.97µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.12ms     6.9 MB/sec    1.00      3.7±0.09ms     7.0 MB/sec
parser/large/dataset.py                    1.00      6.2±0.03ms     6.6 MB/sec    1.01      6.2±0.03ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.00   1216.0±6.28µs    13.7 MB/sec    1.00   1219.4±8.27µs    13.7 MB/sec
parser/numpy/globals.py                    1.00    124.9±0.69µs    23.6 MB/sec    1.00    124.5±0.99µs    23.7 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.01ms     9.6 MB/sec    1.00      2.7±0.02ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.3±0.09ms     2.5 MB/sec    1.00     16.3±0.05ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.02ms     4.0 MB/sec    1.00      4.2±0.02ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    423.9±4.29µs     7.0 MB/sec    1.00    425.3±5.02µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.05ms     3.6 MB/sec    1.00      7.0±0.02ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.04ms     5.0 MB/sec    1.00      8.2±0.02ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1683.0±7.76µs     9.9 MB/sec    1.00   1686.9±7.08µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.0±1.08µs    16.3 MB/sec    1.01    183.0±6.98µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.02ms     6.9 MB/sec    1.00      3.7±0.01ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.7±0.01ms     6.1 MB/sec    1.01      6.8±0.03ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00   1268.7±7.42µs    13.1 MB/sec    1.01   1276.7±5.69µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    129.9±0.98µs    22.7 MB/sec    1.01    131.2±0.62µs    22.5 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.01ms     9.0 MB/sec    1.00      2.8±0.01ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `crates/ruff/src/rules/ruff/rules/collection_literal_concatenation.rs`:57 on 2023-05-22 09:00_

Please add a comment what this function does

---

_Review comment by @konstin on `crates/ruff/src/rules/ruff/rules/collection_literal_concatenation.rs`:134 on 2023-05-22 09:26_

In what case would `splat_at_left` be true here?

---

_Review comment by @konstin on `crates/ruff/src/rules/ruff/rules/collection_literal_concatenation.rs`:128 on 2023-05-22 09:28_

How would this deal with `(1,) + [2]` or `[1] + (2,)`?

---

_@konstin reviewed on 2023-05-22 09:35_

---

_@hoel-bagard reviewed on 2023-05-22 12:59_

---

_Review comment by @hoel-bagard on `crates/ruff/src/rules/ruff/rules/collection_literal_concatenation.rs`:128 on 2023-05-22 12:59_

Running ruff on a file with:
```python
a = (1,) + [2]
b = (1, 2, 3) + (4,)
c = (7, 8, 9) + b
```

The current version outputs a warning only for the last line, whereas the PR in its current state outputs a warning for all three lines. While a warning on the first line is clearly undesired (that's a typing error), should the one on the second line be kept ?

---

_@hoel-bagard reviewed on 2023-05-22 13:10_

---

_Review comment by @hoel-bagard on `crates/ruff/src/rules/ruff/rules/collection_literal_concatenation.rs`:134 on 2023-05-22 13:10_

It seems like it's always true indeed, I've removed the `if`, thank you! 

---

_Review requested from @MichaReiser by @konstin on 2023-05-22 13:26_

---

_Comment by @charliermarsh on 2023-05-22 13:35_

Do you mind adding some information to the PR summary around what needed to change, and why?

---

_Comment by @hoel-bagard on 2023-05-22 14:02_

To be honest I'm not sure I went about it the right way, so if you think that there is a better way to solve the issue I don't mind redoing it another way or trying to tackle a different issue.

On another note, I have a question about the formatting of the test fixture. The following test always gets formatted by black during the pre-commit verify:
```python
foo + [  # This will be preserved, but doesn't prevent the fix
]
```

Should I remove this test (and maybe include a replacement one), or if it better to use `--no-verify` when making a commit modifying the test fixture file ?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/collection_literal_concatenation.rs`:51 on 2023-05-23 06:28_

```suggestion
#[derive(Debug, Copy, Clone, PartialEq, Eq)]
```

Adding `Eq` here should be fine. 

---

_@MichaReiser reviewed on 2023-05-23 06:29_

---

_Review comment by @hoel-bagard on `crates/ruff/src/rules/ruff/rules/collection_literal_concatenation.rs`:51 on 2023-05-23 07:28_

@MichaReiser Thank you for the comment.

Adding `Eq` does not cause any issue indeed. I'm a bit new to Rust, is it good practice to add `Eq` along with `PartialEq` wherever possible ?


Edit: nvm, I found out that it's even a [clippy lint](https://rust-lang.github.io/rust-clippy/master/index.html#derive_partial_eq_without_eq) (with an explanation).

---

_@hoel-bagard reviewed on 2023-05-23 07:28_

---

_Comment by @konstin on 2023-05-23 15:04_

Could you check with `checker.semantic_model().expr_parent()` if we already generated a diagnostic and fix for the parent node and then omit the diagnostic for the current node? https://github.com/charliermarsh/ruff/blob/74effb40b965ea99f92d5096a49a17bbfaf34add/crates/ruff/src/checkers/ast/mod.rs#L1521-L1528 is an example for a similar problem

---

_Comment by @hoel-bagard on 2023-05-23 15:24_

Thank you for the suggestion, I've done that and it got rid of the unwanted warnings.

---

_@konstin approved on 2023-05-23 20:17_

This needs to be updated with the changes from main and a `cargo test` `cargo insta review`, otherwise i think it's ready

---

_Comment by @charliermarsh on 2023-05-23 22:58_

I'll take a look at this tonight and (hopefully) merge.

---

_Renamed from "Fix ruf005" to "Extend `RUF005` to recursive and literal-literal concatenations" by @charliermarsh on 2023-05-24 01:17_

---

_Merged by @charliermarsh on 2023-05-24 01:26_

---

_Closed by @charliermarsh on 2023-05-24 01:26_

---

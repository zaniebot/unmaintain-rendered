```yaml
number: 4743
title: Add autofix to move runtime-imports out of type-checking blocks
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/move-runtime-import
created_at: 2023-05-31T04:08:02Z
updated_at: 2023-05-31T18:21:55Z
url: https://github.com/astral-sh/ruff/pull/4743
synced_at: 2026-01-12T15:55:16Z
```

# Add autofix to move runtime-imports out of type-checking blocks

---

_@charliermarsh_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Similar to #4742, this PR adds autofix for the inverse rule: if an import is in an `if TYPE_CHECKING:` block, but is required at runtime, remove it from the block, and add it to the top level.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-05-31 04:28_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+31, -31, 0 error(s))

<details><summary>airflow (+31, -31)</summary>
<p>

```diff
- airflow/models/__init__.py:118:37: TCH004 Move import `airflow.models.base.ID_LEN` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:118:37: TCH004 [*] Move import `airflow.models.base.ID_LEN` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:118:45: TCH004 Move import `airflow.models.base.Base` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:118:45: TCH004 [*] Move import `airflow.models.base.Base` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:119:45: TCH004 Move import `airflow.models.baseoperator.BaseOperator` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:119:45: TCH004 [*] Move import `airflow.models.baseoperator.BaseOperator` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:119:59: TCH004 Move import `airflow.models.baseoperator.BaseOperatorLink` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:119:59: TCH004 [*] Move import `airflow.models.baseoperator.BaseOperatorLink` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:120:43: TCH004 Move import `airflow.models.connection.Connection` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:120:43: TCH004 [*] Move import `airflow.models.connection.Connection` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:121:36: TCH004 Move import `airflow.models.dag.DAG` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:121:36: TCH004 [*] Move import `airflow.models.dag.DAG` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:121:41: TCH004 Move import `airflow.models.dag.DagModel` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:121:41: TCH004 [*] Move import `airflow.models.dag.DagModel` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:121:51: TCH004 Move import `airflow.models.dag.DagTag` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:121:51: TCH004 [*] Move import `airflow.models.dag.DagTag` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:122:39: TCH004 Move import `airflow.models.dagbag.DagBag` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:122:39: TCH004 [*] Move import `airflow.models.dagbag.DagBag` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:123:42: TCH004 Move import `airflow.models.dagpickle.DagPickle` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:123:42: TCH004 [*] Move import `airflow.models.dagpickle.DagPickle` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:124:39: TCH004 Move import `airflow.models.dagrun.DagRun` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:124:39: TCH004 [*] Move import `airflow.models.dagrun.DagRun` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:125:52: TCH004 Move import `airflow.models.db_callback_request.DbCallbackRequest` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:125:52: TCH004 [*] Move import `airflow.models.db_callback_request.DbCallbackRequest` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:126:39: TCH004 Move import `airflow.models.errors.ImportError` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:126:39: TCH004 [*] Move import `airflow.models.errors.ImportError` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:127:36: TCH004 Move import `airflow.models.log.Log` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:127:36: TCH004 [*] Move import `airflow.models.log.Log` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:128:47: TCH004 Move import `airflow.models.mappedoperator.MappedOperator` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:128:47: TCH004 [*] Move import `airflow.models.mappedoperator.MappedOperator` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:129:41: TCH004 Move import `airflow.models.operator.Operator` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:129:41: TCH004 [*] Move import `airflow.models.operator.Operator` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:130:38: TCH004 Move import `airflow.models.param.Param` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:130:38: TCH004 [*] Move import `airflow.models.param.Param` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:131:37: TCH004 Move import `airflow.models.pool.Pool` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:131:37: TCH004 [*] Move import `airflow.models.pool.Pool` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:132:49: TCH004 Move import `airflow.models.renderedtifields.RenderedTaskInstanceFields` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:132:49: TCH004 [*] Move import `airflow.models.renderedtifields.RenderedTaskInstanceFields` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:133:42: TCH004 Move import `airflow.models.skipmixin.SkipMixin` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:133:42: TCH004 [*] Move import `airflow.models.skipmixin.SkipMixin` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:134:40: TCH004 Move import `airflow.models.slamiss.SlaMiss` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:134:40: TCH004 [*] Move import `airflow.models.slamiss.SlaMiss` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:135:41: TCH004 Move import `airflow.models.taskfail.TaskFail` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:135:41: TCH004 [*] Move import `airflow.models.taskfail.TaskFail` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:136:45: TCH004 Move import `airflow.models.taskinstance.TaskInstance` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:136:45: TCH004 [*] Move import `airflow.models.taskinstance.TaskInstance` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:136:59: TCH004 Move import `airflow.models.taskinstance.clear_task_instances` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:136:59: TCH004 [*] Move import `airflow.models.taskinstance.clear_task_instances` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:137:47: TCH004 Move import `airflow.models.taskreschedule.TaskReschedule` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:137:47: TCH004 [*] Move import `airflow.models.taskreschedule.TaskReschedule` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:138:40: TCH004 Move import `airflow.models.trigger.Trigger` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:138:40: TCH004 [*] Move import `airflow.models.trigger.Trigger` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:139:41: TCH004 Move import `airflow.models.variable.Variable` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:139:41: TCH004 [*] Move import `airflow.models.variable.Variable` out of type-checking block. Import is used for more than type hinting.
- airflow/models/__init__.py:140:37: TCH004 Move import `airflow.models.xcom.XCom` out of type-checking block. Import is used for more than type hinting.
+ airflow/models/__init__.py:140:37: TCH004 [*] Move import `airflow.models.xcom.XCom` out of type-checking block. Import is used for more than type hinting.
- airflow/serialization/serialized_objects.py:74:39: TCH004 Move import `kubernetes.client.models` out of type-checking block. Import is used for more than type hinting.
+ airflow/serialization/serialized_objects.py:74:39: TCH004 [*] Move import `kubernetes.client.models` out of type-checking block. Import is used for more than type hinting.
- airflow/serialization/serialized_objects.py:76:54: TCH004 Move import `airflow.kubernetes.pod_generator.PodGenerator` out of type-checking block. Import is used for more than type hinting.
+ airflow/serialization/serialized_objects.py:76:54: TCH004 [*] Move import `airflow.kubernetes.pod_generator.PodGenerator` out of type-checking block. Import is used for more than type hinting.
- airflow/utils/mixins.py:28:12: TCH004 Move import `multiprocessing.context` out of type-checking block. Import is used for more than type hinting.
+ airflow/utils/mixins.py:28:12: TCH004 [*] Move import `multiprocessing.context` out of type-checking block. Import is used for more than type hinting.
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| TCH004 | 62 | 31 | 31 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.3±0.24ms     2.9 MB/sec    1.01     14.4±0.29ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.05ms     4.9 MB/sec    1.00      3.4±0.07ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    425.6±1.04µs     6.9 MB/sec    1.00    423.1±1.46µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.14ms     4.3 MB/sec    1.00      6.0±0.13ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      6.9±0.08ms     5.9 MB/sec    1.00      6.8±0.07ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1498.8±6.93µs    11.1 MB/sec    1.00   1492.5±5.05µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.1±0.33µs    17.3 MB/sec    1.00    169.9±0.26µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.02ms     8.2 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.2±0.02ms     7.8 MB/sec    1.00      5.2±0.01ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1013.5±4.69µs    16.4 MB/sec    1.00   1011.5±0.73µs    16.5 MB/sec
parser/numpy/globals.py                    1.00    104.6±0.23µs    28.2 MB/sec    1.00    104.5±0.26µs    28.2 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.4 MB/sec    1.00      2.2±0.00ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.3±0.23ms     2.3 MB/sec    1.03     17.8±0.34ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.05ms     3.8 MB/sec    1.00      4.4±0.15ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   509.4±11.94µs     5.8 MB/sec    1.01    512.3±6.60µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.18ms     3.5 MB/sec    1.00      7.4±0.12ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.9±0.09ms     4.6 MB/sec    1.00      8.9±0.12ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1867.7±24.97µs     8.9 MB/sec    1.01  1880.4±21.32µs     8.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    210.5±3.28µs    14.0 MB/sec    1.00    210.5±4.76µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.05ms     6.5 MB/sec    1.00      4.0±0.05ms     6.4 MB/sec
parser/large/dataset.py                    1.16      7.5±0.09ms     5.4 MB/sec    1.00      6.5±0.06ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.13  1373.6±20.50µs    12.1 MB/sec    1.00  1214.7±17.95µs    13.7 MB/sec
parser/numpy/globals.py                    1.09    135.8±2.65µs    21.7 MB/sec    1.00    124.1±1.90µs    23.8 MB/sec
parser/pydantic/types.py                   1.13      3.1±0.04ms     8.2 MB/sec    1.00      2.7±0.04ms     9.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/importer/mod.rs`:83 on 2023-05-31 07:28_

Nit: I would expect a `to_runtime_import` method to return a `RuntimeImport`. How about `to_runtime_import_edit()`

---

_Review comment by @MichaReiser on `crates/ruff/src/importer/mod.rs`:99 on 2023-05-31 07:30_

Super Nit: 

To emphasize that the branching is to create different `insertions`. 

```suggestion
       let insertion = if let Some(stmt) = self.preceding_import(at) {
            // Insert after the last top-level import.
            Insertion::end_of_statement(stmt, self.locator, self.stylist)
        } else {
            // Insert at the start of the file.
            Insertion::start_of_file(self.python_ast, self.locator, self.stylist)
        };
        
        Ok(insertion.into_edit(&content))
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs`:103 on 2023-05-31 07:30_

Can you add a safety comment why calling `unwrap` here is always safe?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs`:104 on 2023-05-31 07:31_

Nit: I don't know if that works but I find this easier to read because it tells me to what it converts it into

```suggestion
                let deleted: Vec<_> = checker.deletions.iter().map(Stmt::from).collect();
```

I'm not sure if `Stmt::from` is the right type because I have no idea what `Into::into` should be converting into :D

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs`:127 on 2023-05-31 07:33_

I think I would prefer to always store the `Edit`s in sorted order if that's what we need rather than requiring fix authors to remember calling `sorted`. 

---

_@MichaReiser approved on 2023-05-31 07:34_

---

_Merged by @charliermarsh on 2023-05-31 18:09_

---

_Closed by @charliermarsh on 2023-05-31 18:09_

---

_Branch deleted on 2023-05-31 18:09_

---

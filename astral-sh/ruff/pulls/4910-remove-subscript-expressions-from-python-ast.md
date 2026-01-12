```yaml
number: 4910
title: Remove subscript expressions from python_ast contains_effect helper
type: pull_request
state: closed
author: Sacrimento
labels: []
assignees: []
base: main
head: fix_contains_effect_subscript
created_at: 2023-06-06T22:12:49Z
updated_at: 2023-06-08T07:37:46Z
url: https://github.com/astral-sh/ruff/pull/4910
synced_at: 2026-01-12T15:55:17Z
```

# Remove subscript expressions from python_ast contains_effect helper

---

_@Sacrimento_

## Summary

Hello!
This PR fixes a few rules where a fix was not possible because of the presence of square brackets in an expression. Previously, the `Subscript` expression was incorrectly considered to have runtime effects. However, I was unable to reproduce any scenarios where square brackets actually had any impact.

Here's an example for rule F841:
```py
foo = {"hello": "world"}

def hello():
    bar = foo["hello"] # Unused variable, ruff won't fix because of [...] 

hello()
```

With this PR, the rules using ruff_python_ast.helpers.contains_effect will be able to fix errors containing subscript expressions.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-06-06 22:24_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+27, -0, 0 error(s))

<details><summary>airflow (+9, -0)</summary>
<p>

```diff
+ airflow/providers/amazon/aws/hooks/cloud_formation.py:55:13: TRY300 Consider moving this statement to an `else` block
+ airflow/providers/amazon/aws/hooks/emr.py:451:13: TRY300 Consider moving this statement to an `else` block
+ airflow/providers/amazon/aws/hooks/glue_catalog.py:162:13: TRY300 Consider moving this statement to an `else` block
+ airflow/providers/amazon/aws/hooks/quicksight.py:115:13: TRY300 Consider moving this statement to an `else` block
+ airflow/providers/amazon/aws/hooks/redshift_cluster.py:99:13: TRY300 Consider moving this statement to an `else` block
+ airflow/providers/amazon/aws/operators/sagemaker.py:1110:13: TRY300 Consider moving this statement to an `else` block
+ airflow/providers/amazon/aws/secrets/systems_manager.py:186:13: TRY300 Consider moving this statement to an `else` block
+ airflow/providers/amazon/aws/sensors/dynamodb.py:107:13: TRY300 Consider moving this statement to an `else` block
+ tests/models/test_xcom_arg.py:126:13: B018 Found useless expression. Either assign it to a variable or remove it.
```

</p>
</details>
<details><summary>bokeh (+7, -0)</summary>
<p>

```diff
+ tests/unit/bokeh/colors/test_util__colors.py:126:13: B018 Found useless expression. Either assign it to a variable or remove it.
+ tests/unit/bokeh/colors/test_util__colors.py:133:13: B018 Found useless expression. Either assign it to a variable or remove it.
+ tests/unit/bokeh/colors/test_util__colors.py:135:13: B018 Found useless expression. Either assign it to a variable or remove it.
+ tests/unit/bokeh/colors/test_util__colors.py:139:13: B018 Found useless expression. Either assign it to a variable or remove it.
+ tests/unit/bokeh/colors/test_util__colors.py:141:13: B018 Found useless expression. Either assign it to a variable or remove it.
+ tests/unit/bokeh/colors/test_util__colors.py:143:13: B018 Found useless expression. Either assign it to a variable or remove it.
+ tests/unit/bokeh/document/test_models.py:85:13: B018 Found useless expression. Either assign it to a variable or remove it.
```

</p>
</details>
<details><summary>disnake (+1, -0)</summary>
<p>

```diff
+ disnake/embeds.py:732:13: B018 Found useless expression. Either assign it to a variable or remove it.
```

</p>
</details>
<details><summary>zulip (+10, -0)</summary>
<p>

```diff
+ zerver/data_import/slack.py:1109:13: SIM401 [*] Use `file_name = fileinfo.get("title", fileinfo["name"])` instead of an `if` block
+ zerver/openapi/markdown_extension.py:231:5: SIM401 Use `param_type = param["schema"].get("type", param["schema"]["oneOf"][0]["type"])` instead of an `if` block
+ zerver/tests/test_custom_profile_data.py:980:17: B018 Found useless expression. Either assign it to a variable or remove it.
+ zerver/tests/test_decorators.py:1078:13: B018 Found useless expression. Either assign it to a variable or remove it.
+ zerver/tests/test_decorators.py:1080:13: B018 Found useless expression. Either assign it to a variable or remove it.
+ zerver/tests/test_decorators.py:1082:13: B018 Found useless expression. Either assign it to a variable or remove it.
+ zerver/tests/test_decorators.py:1086:13: B018 Found useless expression. Either assign it to a variable or remove it.
+ zerver/tests/test_decorators.py:1102:13: B018 Found useless expression. Either assign it to a variable or remove it.
+ zerver/tests/test_decorators.py:1104:13: B018 Found useless expression. Either assign it to a variable or remove it.
+ zerver/worker/queue_processors.py:848:9: SIM401 Use `user_ids = event.get("user_ids", [event["user_profile_id"]])` instead of an `if` block
```

</p>
</details>
Rules changed: 3

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| B018 | 16 | 16 | 0 |
| TRY300 | 8 | 8 | 0 |
| SIM401 | 3 | 3 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.8±0.03ms     7.1 MB/sec  
formatter/numpy/ctypeslib.py               1.00   1198.2±4.11µs    13.9 MB/sec  
formatter/numpy/globals.py                 1.00    140.3±3.78µs    21.0 MB/sec  
formatter/pydantic/types.py                1.00      2.6±0.00ms    10.0 MB/sec  
linter/all-rules/large/dataset.py          1.00     14.1±0.06ms     2.9 MB/sec    1.02     14.3±0.11ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.02      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    412.7±2.43µs     7.1 MB/sec    1.04    429.3±1.73µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.02      6.0±0.06ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.03ms     6.1 MB/sec    1.03      6.9±0.05ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1444.0±6.73µs    11.5 MB/sec    1.05   1510.9±2.03µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    157.3±0.76µs    18.8 MB/sec    1.24   195.8±68.93µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.00ms     8.4 MB/sec    1.04      3.1±0.06ms     8.1 MB/sec
parser/large/dataset.py                                                           1.00      5.2±0.01ms     7.8 MB/sec
parser/numpy/ctypeslib.py                                                         1.00   1016.8±0.77µs    16.4 MB/sec
parser/numpy/globals.py                                                           1.00    105.1±0.25µs    28.1 MB/sec
parser/pydantic/types.py                                                          1.00      2.2±0.00ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.1±0.05ms     5.7 MB/sec  
formatter/numpy/ctypeslib.py               1.00  1415.7±55.82µs    11.8 MB/sec  
formatter/numpy/globals.py                 1.00    164.2±5.96µs    18.0 MB/sec  
formatter/pydantic/types.py                1.00      3.1±0.04ms     8.3 MB/sec  
linter/all-rules/large/dataset.py          1.01     16.9±0.14ms     2.4 MB/sec    1.00     16.7±0.07ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.08ms     3.8 MB/sec    1.00      4.3±0.03ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    430.0±7.77µs     6.9 MB/sec    1.04   447.6±13.37µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.16ms     3.5 MB/sec    1.00      7.2±0.06ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.04ms     4.9 MB/sec    1.00      8.4±0.03ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1759.9±27.34µs     9.5 MB/sec    1.02  1794.8±16.11µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.7±2.92µs    15.6 MB/sec    1.04    198.1±5.88µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.06ms     6.6 MB/sec    1.01      3.9±0.07ms     6.6 MB/sec
parser/large/dataset.py                                                           1.00      6.6±0.02ms     6.2 MB/sec
parser/numpy/ctypeslib.py                                                         1.00  1246.0±10.50µs    13.4 MB/sec
parser/numpy/globals.py                                                           1.00    130.4±0.86µs    22.6 MB/sec
parser/pydantic/types.py                                                          1.00      2.8±0.02ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Closed by @Sacrimento on 2023-06-08 07:37_

---

```yaml
number: 9978
title: "Fix(PERF102): earlier binding causes false positive when analyzing possible incorrect dict iterator"
type: pull_request
state: closed
author: mikeleppane
labels: []
assignees: []
draft: true
base: main
head: fix(PERF102)/false_positive_for_earlier_binding
created_at: 2024-02-13T20:53:50Z
updated_at: 2024-02-16T11:24:34Z
url: https://github.com/astral-sh/ruff/pull/9978
synced_at: 2026-01-12T15:55:30Z
```

# Fix(PERF102): earlier binding causes false positive when analyzing possible incorrect dict iterator

---

_@mikeleppane_

## Summary

This review contains a fix for a false positive scenario in `incorrect dict iterator` rule [PERF102](https://docs.astral.sh/ruff/rules/incorrect-dict-iterator/) 
The problem is that if the current scope contains a binding before `For Statement` (same binding as loop variables) then the rule considers that as a used variable. This causes a false positive scenario. It matters whether references are bound to a loop variable in for statement and after it.   


See: https://github.com/astral-sh/ruff/issues/8786

## Test Plan

```bash
cargo test
```


---

_Renamed from "Fix(PERF102): earlier binding caused false positive when analyzing possible incorrect dict iterator" to "Fix(PERF102): earlier binding causes false positive when analyzing possible incorrect dict iterator" by @mikeleppane on 2024-02-13 21:06_

---

_Comment by @github-actions[bot] on 2024-02-13 21:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+10 -0 violations, +0 -0 fixes in 4 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/3d35fe602ce4c033e47dbe3a4d67f673a14f48d9/airflow/providers/google/cloud/hooks/bigquery.py#L3190'>airflow/providers/google/cloud/hooks/bigquery.py:3190:17:</a> PERF102 When using only the keys of a dict use the `keys()` method
+ <a href='https://github.com/apache/airflow/blob/3d35fe602ce4c033e47dbe3a4d67f673a14f48d9/airflow/providers/google/cloud/transfers/gcs_to_bigquery.py#L716'>airflow/providers/google/cloud/transfers/gcs_to_bigquery.py:716:21:</a> PERF102 When using only the keys of a dict use the `keys()` method
+ <a href='https://github.com/apache/airflow/blob/3d35fe602ce4c033e47dbe3a4d67f673a14f48d9/airflow/www/views.py#L2120'>airflow/www/views.py:2120:25:</a> PERF102 When using only the keys of a dict use the `keys()` method
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L732'>src/bokeh/core/has_props.py:732:21:</a> PERF102 When using only the keys of a dict use the `keys()` method
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/util.py#L270'>src/bokeh/embed/util.py:270:36:</a> PERF102 When using only the keys of a dict use the `keys()` method
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/6ee9854254b72a52c03b50b123d265682aecc448/rotkehlchen/tests/api/test_settings.py#L234'>rotkehlchen/tests/api/test_settings.py:234:27:</a> PERF102 When using only the keys of a dict use the `keys()` method
+ <a href='https://github.com/rotki/rotki/blob/6ee9854254b72a52c03b50b123d265682aecc448/rotkehlchen/tests/api/test_settings.py#L244'>rotkehlchen/tests/api/test_settings.py:244:27:</a> PERF102 When using only the keys of a dict use the `keys()` method
+ <a href='https://github.com/rotki/rotki/blob/6ee9854254b72a52c03b50b123d265682aecc448/rotkehlchen/tests/utils/blockchain.py#L457'>rotkehlchen/tests/utils/blockchain.py:457:51:</a> PERF102 When using only the keys of a dict use the `keys()` method
+ <a href='https://github.com/rotki/rotki/blob/6ee9854254b72a52c03b50b123d265682aecc448/rotkehlchen/tests/utils/blockchain.py#L491'>rotkehlchen/tests/utils/blockchain.py:491:47:</a> PERF102 When using only the keys of a dict use the `keys()` method
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/e246db6709b9edf73d62204c8e6fd6469f3ee15f/zerver/migrations/0221_subscription_notifications_data_migration.py#L47'>zerver/migrations/0221_subscription_notifications_data_migration.py:47:48:</a> PERF102 When using only the keys of a dict use the `keys()` method
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PERF102 | 10 | 10 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+10 -0 violations, +0 -0 fixes in 4 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/3d35fe602ce4c033e47dbe3a4d67f673a14f48d9/airflow/providers/google/cloud/hooks/bigquery.py#L3190'>airflow/providers/google/cloud/hooks/bigquery.py:3190:17:</a> PERF102 When using only the keys of a dict use the `keys()` method
+ <a href='https://github.com/apache/airflow/blob/3d35fe602ce4c033e47dbe3a4d67f673a14f48d9/airflow/providers/google/cloud/transfers/gcs_to_bigquery.py#L716'>airflow/providers/google/cloud/transfers/gcs_to_bigquery.py:716:21:</a> PERF102 When using only the keys of a dict use the `keys()` method
+ <a href='https://github.com/apache/airflow/blob/3d35fe602ce4c033e47dbe3a4d67f673a14f48d9/airflow/www/views.py#L2120'>airflow/www/views.py:2120:25:</a> PERF102 When using only the keys of a dict use the `keys()` method
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L732'>src/bokeh/core/has_props.py:732:21:</a> PERF102 When using only the keys of a dict use the `keys()` method
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/util.py#L270'>src/bokeh/embed/util.py:270:36:</a> PERF102 When using only the keys of a dict use the `keys()` method
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/6ee9854254b72a52c03b50b123d265682aecc448/rotkehlchen/tests/api/test_settings.py#L234'>rotkehlchen/tests/api/test_settings.py:234:27:</a> PERF102 When using only the keys of a dict use the `keys()` method
+ <a href='https://github.com/rotki/rotki/blob/6ee9854254b72a52c03b50b123d265682aecc448/rotkehlchen/tests/api/test_settings.py#L244'>rotkehlchen/tests/api/test_settings.py:244:27:</a> PERF102 When using only the keys of a dict use the `keys()` method
+ <a href='https://github.com/rotki/rotki/blob/6ee9854254b72a52c03b50b123d265682aecc448/rotkehlchen/tests/utils/blockchain.py#L457'>rotkehlchen/tests/utils/blockchain.py:457:51:</a> PERF102 When using only the keys of a dict use the `keys()` method
+ <a href='https://github.com/rotki/rotki/blob/6ee9854254b72a52c03b50b123d265682aecc448/rotkehlchen/tests/utils/blockchain.py#L491'>rotkehlchen/tests/utils/blockchain.py:491:47:</a> PERF102 When using only the keys of a dict use the `keys()` method
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/e246db6709b9edf73d62204c8e6fd6469f3ee15f/zerver/migrations/0221_subscription_notifications_data_migration.py#L47'>zerver/migrations/0221_subscription_notifications_data_migration.py:47:48:</a> PERF102 When using only the keys of a dict use the `keys()` method
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PERF102 | 10 | 10 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @mikeleppane on 2024-02-13 21:19_

Hmm...some false positives from ecosystem tests. I need to investigate...

Update: I now know what's the issue. I need to refactor the code to properly tackle the issue.

---

_Converted to draft by @mikeleppane on 2024-02-14 08:11_

---

_Closed by @mikeleppane on 2024-02-15 07:52_

---

_Branch deleted on 2024-02-15 07:52_

---

_Comment by @mikeleppane on 2024-02-15 13:17_

@dhruvmanila Question: 
If you have the following code:
```python  
def foo():
    for key, value in some_dict.items():
        ...

    for key, value in some_dict.items():
        ...
```
What's the best way to get all the child statements of foo func when analyzing the first `ForStmt`? Or, put it differently, get all statements from the parent/global scope. So, in this case, those would be the two `ForStmt`s. 



---

_Comment by @dhruvmanila on 2024-02-16 07:19_

I believe you can use the semantic model to traverse up the tree until you reach a function definition and then you can get the body of the function which would contain all of the statements.

So, the `current_statements` would return an iterator over all the statements in the current hierarchy, starting from the first `for` loop to the function definition:

https://github.com/astral-sh/ruff/blob/fe79798c12b4771cee0b0c59964ad7bd751c3779/crates/ruff_python_semantic/src/model.rs#L894-L901

@mikeleppane Does this help?

---

_Comment by @mikeleppane on 2024-02-16 11:22_

> I believe you can use the semantic model to traverse up the tree until you reach a function definition and then you can get the body of the function which would contain all of the statements.
> 
> So, the `current_statements` would return an iterator over all the statements in the current hierarchy, starting from the first `for` loop to the function definition:
> 
> https://github.com/astral-sh/ruff/blob/fe79798c12b4771cee0b0c59964ad7bd751c3779/crates/ruff_python_semantic/src/model.rs#L894-L901
> 
> @mikeleppane Does this help?

Yeah, I could use that. Thanks! Originally, I thought that there might be some short-cut to get those required stmts, but this will do it. 

---

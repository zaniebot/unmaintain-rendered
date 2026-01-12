```yaml
number: 15160
title: "[ruff] Implement falsy-dict-get-fallback (RUF056)"
type: pull_request
state: merged
author: guptaarnav
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: falsy-dict-get-fallback
created_at: 2024-12-27T00:40:49Z
updated_at: 2024-12-31T10:40:52Z
url: https://github.com/astral-sh/ruff/pull/15160
synced_at: 2026-01-12T15:55:50Z
```

# [ruff] Implement falsy-dict-get-fallback (RUF056)

---

_@guptaarnav_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Closes #4934 by implementing a new RUF rule that checks for dict.get calls that include a falsy fallback used in the context of a boolean, and returns a safe fix that removes that falsy fallback (except for situations where there are comments between arguments, in which case an unsafe fix is returned).  
Note, using a falsy fallback for dict.get is okay when the expression is not used as a boolean, e.g. when the the actual value of the fallback is used instead of being converted to a boolean.

## Test Plan

A RUF056.py snapshot test file was created that includes valid and invalid code based on this lint rule.  
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-12-27 00:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+40 -0 violations, +0 -0 fixes in 7 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/domain.py#L1546'>rasa/shared/core/domain.py:1546:43:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/utils/tensorflow/rasa_layers.py#L397'>rasa/utils/tensorflow/rasa_layers.py:397:47:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/utils/tensorflow/rasa_layers.py#L398'>rasa/utils/tensorflow/rasa_layers.py:398:50:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/utils/tensorflow/rasa_layers.py#L758'>rasa/utils/tensorflow/rasa_layers.py:758:77:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/52ed7d72e9669a7bdfaf11caeea6daa29911bc8f/airflow/serialization/serialized_objects.py#L1393'>airflow/serialization/serialized_objects.py:1393:67:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/apache/airflow/blob/52ed7d72e9669a7bdfaf11caeea6daa29911bc8f/airflow/serialization/serialized_objects.py#L1401'>airflow/serialization/serialized_objects.py:1401:85:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/apache/airflow/blob/52ed7d72e9669a7bdfaf11caeea6daa29911bc8f/airflow/serialization/serialized_objects.py#L1431'>airflow/serialization/serialized_objects.py:1431:41:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/apache/airflow/blob/52ed7d72e9669a7bdfaf11caeea6daa29911bc8f/providers/src/airflow/providers/apache/hdfs/hooks/webhdfs.py#L123'>providers/src/airflow/providers/apache/hdfs/hooks/webhdfs.py:123:90:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/apache/airflow/blob/52ed7d72e9669a7bdfaf11caeea6daa29911bc8f/scripts/in_container/in_container_utils.py#L48'>scripts/in_container/in_container_utils.py:48:74:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/91d16482301f8dd944e0e137b7c185da50aeb991/superset/db_engine_specs/databricks.py#L286'>superset/db_engine_specs/databricks.py:286:69:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+24 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/client/grpc_handler.py#L1298'>pymilvus/client/grpc_handler.py:1298:33:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/client/grpc_handler.py#L1528'>pymilvus/client/grpc_handler.py:1528:33:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/client/grpc_handler.py#L2099'>pymilvus/client/grpc_handler.py:2099:33:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/client/grpc_handler.py#L307'>pymilvus/client/grpc_handler.py:307:33:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/client/grpc_handler.py#L602'>pymilvus/client/grpc_handler.py:602:37:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/client/grpc_handler.py#L613'>pymilvus/client/grpc_handler.py:613:37:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/client/grpc_handler.py#L638'>pymilvus/client/grpc_handler.py:638:37:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/client/grpc_handler.py#L649'>pymilvus/client/grpc_handler.py:649:37:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/client/grpc_handler.py#L709'>pymilvus/client/grpc_handler.py:709:37:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/client/grpc_handler.py#L760'>pymilvus/client/grpc_handler.py:760:37:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/client/grpc_handler.py#L775'>pymilvus/client/grpc_handler.py:775:37:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/client/grpc_handler.py#L783'>pymilvus/client/grpc_handler.py:783:37:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/client/grpc_handler.py#L794'>pymilvus/client/grpc_handler.py:794:37:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/client/prepare.py#L413'>pymilvus/client/prepare.py:413:42:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/client/prepare.py#L414'>pymilvus/client/prepare.py:414:35:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/orm/collection.py#L581'>pymilvus/orm/collection.py:581:33:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/orm/collection.py#L654'>pymilvus/orm/collection.py:654:60:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/orm/collection.py#L796'>pymilvus/orm/collection.py:796:63:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/orm/collection.py#L814'>pymilvus/orm/collection.py:814:59:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/orm/collection.py#L940'>pymilvus/orm/collection.py:940:63:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/orm/collection.py#L956'>pymilvus/orm/collection.py:956:59:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/orm/connections.py#L413'>pymilvus/orm/connections.py:413:41:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/orm/index.py#L77'>pymilvus/orm/index.py:77:41:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/milvus-io/pymilvus/blob/e3988936b188bdc846f92f013bc320904074e47f/pymilvus/orm/partition.py#L50'>pymilvus/orm/partition.py:50:41:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/1166bc001e70c74d0cf2b2a35183da96421ae3a7/zerver/actions/streams.py#L297'>zerver/actions/streams.py:297:66:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/zulip/zulip/blob/1166bc001e70c74d0cf2b2a35183da96421ae3a7/zerver/lib/import_realm.py#L903'>zerver/lib/import_realm.py:903:54:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/zulip/zulip/blob/1166bc001e70c74d0cf2b2a35183da96421ae3a7/zerver/lib/outgoing_webhook.py#L87'>zerver/lib/outgoing_webhook.py:87:55:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
+ <a href='https://github.com/zulip/zulip/blob/1166bc001e70c74d0cf2b2a35183da96421ae3a7/zerver/views/documentation.py#L268'>zerver/views/documentation.py:268:53:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/9c02a3f6ed59215a06fb146ccb573182e9481e06/src/_pytest/_py/path.py#L953'>src/_pytest/_py/path.py:953:30:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/116bf78a8941195922ef02b28ae2b489d65f1ea5/astropy/utils/masked/tests/test_function_helpers.py#L1050'>astropy/utils/masked/tests/test_function_helpers.py:1050:53:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF056 | 40 | 40 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Converted to draft by @guptaarnav on 2024-12-27 00:55_

---

_Marked ready for review by @guptaarnav on 2024-12-27 02:20_

---

_Comment by @guptaarnav on 2024-12-27 02:28_

> ## `ruff-ecosystem` results
> ### Linter (stable)
> ✅ ecosystem check detected no linter changes.
> 
> ### Linter (preview)
> ℹ️ ecosystem check **detected linter changes**. (+40 -0 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)
> 
> [RasaHQ/rasa](https://github.com/RasaHQ/rasa) (+5 -0 violations, +0 -0 fixes)
> [apache/airflow](https://github.com/apache/airflow) (+3 -0 violations, +0 -0 fixes)
> [milvus-io/pymilvus](https://github.com/milvus-io/pymilvus) (+26 -0 violations, +0 -0 fixes)
> [zulip/zulip](https://github.com/zulip/zulip) (+4 -0 violations, +0 -0 fixes)
> [pytest-dev/pytest](https://github.com/pytest-dev/pytest) (+1 -0 violations, +0 -0 fixes)
> [astropy/astropy](https://github.com/astropy/astropy) (+1 -0 violations, +0 -0 fixes)
> Changes by rule (1 rules affected)
> 
> code	total	+ violation	- violation	+ fix	- fix
> RUF056	40	40	0	0	0

Went through all 40 ecosystem linter changes, all are as expected. 

---

_Label `rule` added by @MichaReiser on 2024-12-30 14:01_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1384 on 2024-12-30 14:07_

I think we should hook up the rule in the `CallExpr` visiter only and instead use `checker.semantic().in_boolean_test()` to determine if the expression is in a boolean test position. That also ensures that it just works for `while`, match arms, assert, etc. See `len_test` for a similar rule https://github.com/astral-sh/ruff/blob/56ae73a925a6545a714c303e7aaeed4ea5b385bc/crates/ruff_linter/src/rules/pylint/rules/len_test.rs#L72

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/falsy_dict_get_fallback.rs`:13 on 2024-12-30 14:09_


```suggestion
/// Checks for `dict.get(key, falsy_value)` calls in boolean test positions.
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/falsy_dict_get_fallback.rs`:83 on 2024-12-30 14:12_

We should use `arguments.find_argument("default", 1)` instead to support `dict.get(default=False, key=10)` (and we should add a test for it)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/falsy_dict_get_fallback.rs`:133 on 2024-12-30 14:13_

Can we reuse `Truthiness` instead of rolling our own helper here? 

https://github.com/astral-sh/ruff/blob/de62e39eba6553545fa466f709c9a75e88bda9f0/crates/ruff_python_ast/src/helpers.rs#L1127

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/falsy_dict_get_fallback.rs`:91 on 2024-12-30 14:14_

Can we use the `remove_argument` helper here https://github.com/astral-sh/ruff/blob/b63c2e126b928fab4180eda5d9132552beeee7fe/crates/ruff_linter/src/fix/edits.rs#L207

---

_@MichaReiser requested changes on 2024-12-30 14:14_

Nice thank you. I think we can reuse some existing helpers to remove the necessary code for this rule

---

_Review requested from @MichaReiser by @guptaarnav on 2024-12-30 23:12_

---

_Label `preview` added by @dhruvmanila on 2024-12-31 05:20_

---

_@MichaReiser approved on 2024-12-31 10:31_

---

_Merged by @MichaReiser on 2024-12-31 10:40_

---

_Closed by @MichaReiser on 2024-12-31 10:40_

---

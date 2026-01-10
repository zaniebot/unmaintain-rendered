```yaml
number: 8450
title: "`PIE804`: Prevent keyword arguments duplication"
type: pull_request
state: merged
author: T-256
labels:
  - bug
assignees: []
merged: true
base: main
head: pie804-unsafe
created_at: 2023-11-03T00:48:11Z
updated_at: 2023-12-12T23:25:54Z
url: https://github.com/astral-sh/ruff/pull/8450
synced_at: 2026-01-10T23:31:11Z
```

# `PIE804`: Prevent keyword arguments duplication

---

_Pull request opened by @T-256 on 2023-11-03 00:48_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

According to the https://github.com/astral-sh/ruff/issues/8402#issuecomment-1788785766

## Test Plan

<!-- How was it tested? -->
Added new unsafe test

---

_@zanieb reviewed on 2023-11-03 00:56_

---

_Review comment by @zanieb on `crates/ruff_linter/resources/test/fixtures/flake8_pie/PIE804.py`:24 on 2023-11-03 00:56_

Did you mean
```suggestion
unsafe(**{'a': b}, **{'a': c})  # PIE804
```

---

_@zanieb requested changes on 2023-11-03 00:58_

Hm... I'm not sure just changing this fix to unsafe is the best approach. The unsafe fix will will still always result in a syntax error when applied.

This fix just turns a runtime error into a syntax error though:

```python
def foo(*args, **kwargs):
    pass

foo(**{'a': 1}, **{'a': 2})
```
```
❯ python example.py
Traceback (most recent call last):
  File "/Users/mz/eng/src/astral-sh/puffin/example.py", line 4, in <module>
    foo(**{'a': 1}, **{'a': 2})
TypeError: __main__.foo() got multiple values for keyword argument 'a'
```

Since the keys are static, can't we just add a guard here to determine if a key already exists?

---

_Comment by @github-actions[bot] on 2023-11-03 01:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+24 -24 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+24 -24 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/operators/test_eks.py#L728'>tests/providers/amazon/aws/operators/test_eks.py:728:30:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/operators/test_eks.py#L728'>tests/providers/amazon/aws/operators/test_eks.py:728:45:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L102'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:102:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L103'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:103:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L132'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:132:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L133'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:133:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L181'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:181:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L182'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:182:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L219'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:219:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L220'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:220:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L268'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:268:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L269'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:269:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L61'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:61:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L62'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:62:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/cncf/kubernetes/utils/test_pod_manager.py#L826'>tests/providers/cncf/kubernetes/utils/test_pod_manager.py:826:30:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/cncf/kubernetes/utils/test_pod_manager.py#L826'>tests/providers/cncf/kubernetes/utils/test_pod_manager.py:826:46:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/cloud/hooks/test_life_sciences.py#L125'>tests/providers/google/cloud/hooks/test_life_sciences.py:125:24:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/cloud/hooks/test_life_sciences.py#L126'>tests/providers/google/cloud/hooks/test_life_sciences.py:126:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/cloud/hooks/test_life_sciences.py#L154'>tests/providers/google/cloud/hooks/test_life_sciences.py:154:24:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/cloud/hooks/test_life_sciences.py#L154'>tests/providers/google/cloud/hooks/test_life_sciences.py:154:34:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/cloud/hooks/test_life_sciences.py#L231'>tests/providers/google/cloud/hooks/test_life_sciences.py:231:24:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/cloud/hooks/test_life_sciences.py#L231'>tests/providers/google/cloud/hooks/test_life_sciences.py:231:34:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/cloud/hooks/test_life_sciences.py#L259'>tests/providers/google/cloud/hooks/test_life_sciences.py:259:24:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/cloud/hooks/test_life_sciences.py#L259'>tests/providers/google/cloud/hooks/test_life_sciences.py:259:34:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/cloud/operators/test_kubernetes_engine.py#L406'>tests/providers/google/cloud/operators/test_kubernetes_engine.py:406:30:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/cloud/operators/test_kubernetes_engine.py#L406'>tests/providers/google/cloud/operators/test_kubernetes_engine.py:406:45:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/firebase/hooks/test_firestore.py#L100'>tests/providers/google/firebase/hooks/test_firestore.py:100:24:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/firebase/hooks/test_firestore.py#L101'>tests/providers/google/firebase/hooks/test_firestore.py:101:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/firebase/hooks/test_firestore.py#L122'>tests/providers/google/firebase/hooks/test_firestore.py:122:24:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/firebase/hooks/test_firestore.py#L122'>tests/providers/google/firebase/hooks/test_firestore.py:122:34:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/firebase/hooks/test_firestore.py#L189'>tests/providers/google/firebase/hooks/test_firestore.py:189:24:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/firebase/hooks/test_firestore.py#L190'>tests/providers/google/firebase/hooks/test_firestore.py:190:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/firebase/hooks/test_firestore.py#L216'>tests/providers/google/firebase/hooks/test_firestore.py:216:24:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/firebase/hooks/test_firestore.py#L216'>tests/providers/google/firebase/hooks/test_firestore.py:216:34:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/telegram/hooks/test_telegram.py#L109'>tests/providers/telegram/hooks/test_telegram.py:109:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/telegram/hooks/test_telegram.py#L110'>tests/providers/telegram/hooks/test_telegram.py:110:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/telegram/hooks/test_telegram.py#L128'>tests/providers/telegram/hooks/test_telegram.py:128:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/telegram/hooks/test_telegram.py#L129'>tests/providers/telegram/hooks/test_telegram.py:129:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/telegram/hooks/test_telegram.py#L147'>tests/providers/telegram/hooks/test_telegram.py:147:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/telegram/hooks/test_telegram.py#L148'>tests/providers/telegram/hooks/test_telegram.py:148:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/telegram/hooks/test_telegram.py#L174'>tests/providers/telegram/hooks/test_telegram.py:174:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/telegram/hooks/test_telegram.py#L175'>tests/providers/telegram/hooks/test_telegram.py:175:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/telegram/hooks/test_telegram.py#L194'>tests/providers/telegram/hooks/test_telegram.py:194:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/telegram/hooks/test_telegram.py#L195'>tests/providers/telegram/hooks/test_telegram.py:195:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/weaviate/operators/test_weaviate.py#L49'>tests/providers/weaviate/operators/test_weaviate.py:49:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/weaviate/operators/test_weaviate.py#L50'>tests/providers/weaviate/operators/test_weaviate.py:50:71:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/utils/test_compression.py#L76'>tests/utils/test_compression.py:76:13:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/utils/test_compression.py#L77'>tests/utils/test_compression.py:77:17:</a> PIE804 [*] Unnecessary `dict` kwargs
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PIE804 | 48 | 24 | 24 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+24 -24 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+24 -24 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/operators/test_eks.py#L728'>tests/providers/amazon/aws/operators/test_eks.py:728:30:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/operators/test_eks.py#L728'>tests/providers/amazon/aws/operators/test_eks.py:728:45:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L102'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:102:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L103'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:103:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L132'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:132:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L133'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:133:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L181'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:181:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L182'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:182:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L219'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:219:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L220'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:220:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L268'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:268:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L269'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:269:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L61'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:61:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L62'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:62:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/cncf/kubernetes/utils/test_pod_manager.py#L826'>tests/providers/cncf/kubernetes/utils/test_pod_manager.py:826:30:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/cncf/kubernetes/utils/test_pod_manager.py#L826'>tests/providers/cncf/kubernetes/utils/test_pod_manager.py:826:46:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/cloud/hooks/test_life_sciences.py#L125'>tests/providers/google/cloud/hooks/test_life_sciences.py:125:24:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/cloud/hooks/test_life_sciences.py#L126'>tests/providers/google/cloud/hooks/test_life_sciences.py:126:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/cloud/hooks/test_life_sciences.py#L154'>tests/providers/google/cloud/hooks/test_life_sciences.py:154:24:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/cloud/hooks/test_life_sciences.py#L154'>tests/providers/google/cloud/hooks/test_life_sciences.py:154:34:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/cloud/hooks/test_life_sciences.py#L231'>tests/providers/google/cloud/hooks/test_life_sciences.py:231:24:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/cloud/hooks/test_life_sciences.py#L231'>tests/providers/google/cloud/hooks/test_life_sciences.py:231:34:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/cloud/hooks/test_life_sciences.py#L259'>tests/providers/google/cloud/hooks/test_life_sciences.py:259:24:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/cloud/hooks/test_life_sciences.py#L259'>tests/providers/google/cloud/hooks/test_life_sciences.py:259:34:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/cloud/operators/test_kubernetes_engine.py#L406'>tests/providers/google/cloud/operators/test_kubernetes_engine.py:406:30:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/cloud/operators/test_kubernetes_engine.py#L406'>tests/providers/google/cloud/operators/test_kubernetes_engine.py:406:45:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/firebase/hooks/test_firestore.py#L100'>tests/providers/google/firebase/hooks/test_firestore.py:100:24:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/firebase/hooks/test_firestore.py#L101'>tests/providers/google/firebase/hooks/test_firestore.py:101:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/firebase/hooks/test_firestore.py#L122'>tests/providers/google/firebase/hooks/test_firestore.py:122:24:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/firebase/hooks/test_firestore.py#L122'>tests/providers/google/firebase/hooks/test_firestore.py:122:34:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/firebase/hooks/test_firestore.py#L189'>tests/providers/google/firebase/hooks/test_firestore.py:189:24:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/firebase/hooks/test_firestore.py#L190'>tests/providers/google/firebase/hooks/test_firestore.py:190:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/firebase/hooks/test_firestore.py#L216'>tests/providers/google/firebase/hooks/test_firestore.py:216:24:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/google/firebase/hooks/test_firestore.py#L216'>tests/providers/google/firebase/hooks/test_firestore.py:216:34:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/telegram/hooks/test_telegram.py#L109'>tests/providers/telegram/hooks/test_telegram.py:109:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/telegram/hooks/test_telegram.py#L110'>tests/providers/telegram/hooks/test_telegram.py:110:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/telegram/hooks/test_telegram.py#L128'>tests/providers/telegram/hooks/test_telegram.py:128:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/telegram/hooks/test_telegram.py#L129'>tests/providers/telegram/hooks/test_telegram.py:129:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/telegram/hooks/test_telegram.py#L147'>tests/providers/telegram/hooks/test_telegram.py:147:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/telegram/hooks/test_telegram.py#L148'>tests/providers/telegram/hooks/test_telegram.py:148:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/telegram/hooks/test_telegram.py#L174'>tests/providers/telegram/hooks/test_telegram.py:174:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/telegram/hooks/test_telegram.py#L175'>tests/providers/telegram/hooks/test_telegram.py:175:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/telegram/hooks/test_telegram.py#L194'>tests/providers/telegram/hooks/test_telegram.py:194:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/telegram/hooks/test_telegram.py#L195'>tests/providers/telegram/hooks/test_telegram.py:195:13:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/weaviate/operators/test_weaviate.py#L49'>tests/providers/weaviate/operators/test_weaviate.py:49:9:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/providers/weaviate/operators/test_weaviate.py#L50'>tests/providers/weaviate/operators/test_weaviate.py:50:71:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/utils/test_compression.py#L76'>tests/utils/test_compression.py:76:13:</a> PIE804 [*] Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/6f57ee12564759892af1152606af5373d2b709c0/tests/utils/test_compression.py#L77'>tests/utils/test_compression.py:77:17:</a> PIE804 [*] Unnecessary `dict` kwargs
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PIE804 | 48 | 24 | 24 | 0 | 0 |

</p>
</details>




---

_Converted to draft by @T-256 on 2023-11-03 02:17_

---

_Renamed from "`PIE804`: Convert to unsafe autofixes" to "`PIE804`: Prevent keyword arguments duplication" by @T-256 on 2023-11-03 04:14_

---

_Marked ready for review by @T-256 on 2023-11-03 04:19_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_dict_kwargs.rs`:56 on 2023-11-03 14:59_

Should this be a `Vec`? Why not a `HashSet`?

---

_@zanieb reviewed on 2023-11-03 15:00_

---

_Comment by @zanieb on 2023-11-03 15:06_

While this doesn't introduce a syntax error, there's still an error that I don't think any lint rule addresses yet. Maybe we should also consider a rule that catches duplicate keyword arguments?

---

_Review comment by @zanieb on `crates/ruff_linter/resources/test/fixtures/flake8_pie/PIE804.py`:24 on 2023-11-03 15:08_

We should add a comment explaining this test case (and its not unsafe anymore)

---

_@zanieb reviewed on 2023-11-03 15:08_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_dict_kwargs.rs`:93 on 2023-11-03 15:09_

`HashSet.insert` will return a bool https://doc.rust-lang.org/std/collections/struct.HashSet.html#method.insert

---

_@zanieb reviewed on 2023-11-03 15:09_

---

_Comment by @T-256 on 2023-11-03 15:18_

Sorry for all violations, I'm not expert in Rust, they are fixing.

---

_Comment by @T-256 on 2023-11-03 16:08_

> Maybe we should also consider a rule that catches duplicate keyword arguments?

Perhaps I could work on it, which namespace should put this rule?

---

_@dhruvmanila reviewed on 2023-11-06 03:19_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/flake8_pie/PIE804.py`:25 on 2023-11-06 03:19_

How would this work with positional arguments? For example,
```python
def abc(a):
	pass

abc(1, **{'a': 2})
```

Correct me if I'm wrong but I guess it would fix it to `abc(1, a=2)` which isn't a syntax error although it's a runtime error.

I don't think this is in scope for this rule so maybe we could just highlight it in the docs. I'll leave this up to @zanieb as the main reviewer :)

---

_@T-256 reviewed on 2023-11-06 11:39_

---

_Review comment by @T-256 on `crates/ruff_linter/resources/test/fixtures/flake8_pie/PIE804.py`:25 on 2023-11-06 11:39_

Yes, needs info in docs, with considering bellow code as correct:
```py
def abc(a, /, **kw): ...

abc(1, **{'a': 2})
```

---

_@zanieb reviewed on 2023-11-06 16:12_

---

_Review comment by @zanieb on `crates/ruff_linter/resources/test/fixtures/flake8_pie/PIE804.py`:25 on 2023-11-06 16:12_

Yeah since we're just transforming invalid code at runtime to more invalid code that seems fine. We could probably include this in a new rule too 😬 


---

_Comment by @zanieb on 2023-11-06 16:13_

> > Maybe we should also consider a rule that catches duplicate keyword arguments?
> 
> Perhaps I could work on it, which namespace should put this rule?

The first step is probably a new issue for discussion. We could probably encompass the positional case in the same issue.

---

_Comment by @T-256 on 2023-11-06 16:55_

Fix safety added to docs.

@zanieb Can you open issue for the new rule? Since I don't know what's correct behavior for new rule on:
```
abc(a=1, **{'a': 2})
```
If function definition uses `/` for kwargs then above function calling is correct. It's not easy get access to definition for checking `/`.

---

_Assigned to @zanieb by @zanieb on 2023-11-06 22:49_

---

_Review requested from @zanieb by @T-256 on 2023-11-10 12:34_

---

_Comment by @charliermarsh on 2023-11-16 01:00_

@zanieb - Relatedly, there's now a PR for duplicate keyword arguments: https://github.com/astral-sh/ruff/pull/8706

---

_@zanieb reviewed on 2023-11-22 18:13_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_pie/snapshots/ruff_linter__rules__flake8_pie__tests__PIE804_PIE804.py.snap`:136 on 2023-11-22 18:13_

Sorry about the delay in the review here!

Should we even offer to fix the first one if the whole thing is invalid?

---

_@T-256 reviewed on 2023-11-23 07:50_

---

_Review comment by @T-256 on `crates/ruff_linter/src/rules/flake8_pie/snapshots/ruff_linter__rules__flake8_pie__tests__PIE804_PIE804.py.snap`:136 on 2023-11-23 07:50_

Fix does have exactly the same behavior and doesn't change the code.
Also, only syntax errors could be unsafe fix, so it doesn't even have need to be unsafe (discussed above)

If we want to change it to unsafe Or sometimes fixable violation, we should also consider this:
```py
abc(**a, **{'b': 0})
```
where `a` may have `b` key inside.

---

_@T-256 reviewed on 2023-11-23 07:54_

---

_Review comment by @T-256 on `crates/ruff_linter/src/rules/flake8_pie/snapshots/ruff_linter__rules__flake8_pie__tests__PIE804_PIE804.py.snap`:136 on 2023-11-23 07:54_

I think it need another rule when there is multiple unknown keyword unpacking on calling callables.
examples:
```
abc(**a, **b)
abc(**a, **{'b': 0})
abc(a=2, **b)
```
all of these potentially can be runtime error.

---

_@zanieb reviewed on 2023-11-28 20:10_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_pie/snapshots/ruff_linter__rules__flake8_pie__tests__PIE804_PIE804.py.snap`:136 on 2023-11-28 20:10_

> Fix does have exactly the same behavior and doesn't change the code.

Yes but the "same behavior" here is a runtime error. I'm worried it will be confusing to users to fix one violation of this then not the second. We cannot fix the second violation because we don't want to introduce a syntax error. It might be clearer to the user not suggesting _any_ fixes when we can tell the code has a runtime error.

---

_@charliermarsh approved on 2023-12-12 23:12_

I modified to match Zanie's feedback, but otherwise LGTM.

---

_Label `bug` added by @charliermarsh on 2023-12-12 23:13_

---

_Merged by @charliermarsh on 2023-12-12 23:19_

---

_Closed by @charliermarsh on 2023-12-12 23:19_

---

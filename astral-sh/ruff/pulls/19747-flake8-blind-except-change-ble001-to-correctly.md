```yaml
number: 19747
title: "[`flake8-blind-except`] Change `BLE001` to correctly parse exception tuples"
type: pull_request
state: merged
author: deliro
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: ble-tuple-exceptions
created_at: 2025-08-04T18:14:39Z
updated_at: 2025-08-05T05:58:14Z
url: https://github.com/astral-sh/ruff/pull/19747
synced_at: 2026-01-10T17:52:17Z
```

# [`flake8-blind-except`] Change `BLE001` to correctly parse exception tuples

---

_Pull request opened by @deliro on 2025-08-04 18:14_

## Summary

This PR enhances the `BLE001` rule to correctly detect blind exception handling in tuple exceptions. Previously, the rule only checked single exception types, but Python allows catching multiple exceptions using tuples like `except (Exception, ValueError):`. 

## Test Plan

It fails the following (whereas the main branch does not):

```bash
cargo run -p ruff -- check somefile.py --no-cache --select=BLE001
```

```python
# somefile.py

try:
    1/0
except (ValueError, Exception) as e:
    print(e)
```

```
somefile.py:3:21: BLE001 Do not catch blind exception: `Exception`
  |
1 | try:
2 |     1/0
3 | except (ValueError, Exception) as e:
  |                     ^^^^^^^^^ BLE001
4 |     print(e)
  |

Found 1 error.
```

---

_Label `rule` added by @dylwil3 on 2025-08-04 20:30_

---

_Renamed from "[flake8-blind-except] Change BLE001 to correctly parse exception tuples" to "[f`lake8-blind-except`] Change `BLE001` to correctly parse exception tuples" by @dylwil3 on 2025-08-04 20:30_

---

_Label `bug` added by @dylwil3 on 2025-08-04 20:33_

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/flake8_blind_except/BLE.py`:1 on 2025-08-04 20:34_

Can you add a test fixture with a tuple and an `as` clause? And verify that the rule is still skipped when the exception is re-raised?

---

_@dylwil3 requested changes on 2025-08-04 20:39_

Neat, thank you! I'm gonna call this a bug instead of an extension of the rule - I don't think it needs to be gated behind preview. Let's just add a couple more tests - the implementation looks good to me though!

---

_Comment by @github-actions[bot] on 2025-08-04 20:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/c2059b9394d021cf955cf8bae13da8c39965bd92/airflow-core/src/airflow/models/dagbundle.py#L74'>airflow-core/src/airflow/models/dagbundle.py:74:31:</a> BLE001 Do not catch blind exception: `Exception`
+ <a href='https://github.com/apache/airflow/blob/c2059b9394d021cf955cf8bae13da8c39965bd92/providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py#L283'>providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py:283:13:</a> BLE001 Do not catch blind exception: `Exception`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/3ae0c4c68b10b493396eb51b8a8800d5a6e66c4f/zerver/webhooks/slack/view.py#L225'>zerver/webhooks/slack/view.py:225:29:</a> BLE001 Do not catch blind exception: `Exception`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| BLE001 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/c2059b9394d021cf955cf8bae13da8c39965bd92/airflow-core/src/airflow/models/dagbundle.py#L74'>airflow-core/src/airflow/models/dagbundle.py:74:31:</a> BLE001 Do not catch blind exception: `Exception`
+ <a href='https://github.com/apache/airflow/blob/c2059b9394d021cf955cf8bae13da8c39965bd92/providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py#L283'>providers/celery/src/airflow/providers/celery/executors/celery_executor_utils.py:283:13:</a> BLE001 Do not catch blind exception: `Exception`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/3ae0c4c68b10b493396eb51b8a8800d5a6e66c4f/zerver/webhooks/slack/view.py#L225'>zerver/webhooks/slack/view.py:225:29:</a> BLE001 Do not catch blind exception: `Exception`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| BLE001 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>




---

_@deliro reviewed on 2025-08-04 20:45_

---

_Review comment by @deliro on `crates/ruff_linter/resources/test/fixtures/flake8_blind_except/BLE.py`:1 on 2025-08-04 20:45_

Sure! [Added](https://github.com/astral-sh/ruff/compare/5620d2f5b5c7dac12c8c6cff228db24445dcb66b..e433fe784a7861a396a726b9fab3ec7f2e1877fd)

---

_Review requested from @dylwil3 by @deliro on 2025-08-04 20:47_

---

_Renamed from "[f`lake8-blind-except`] Change `BLE001` to correctly parse exception tuples" to "[`flake8-blind-except`] Change `BLE001` to correctly parse exception tuples" by @deliro on 2025-08-04 20:51_

---

_@dylwil3 approved on 2025-08-04 21:10_

Nice, thank you!

---

_Merged by @dylwil3 on 2025-08-04 21:12_

---

_Closed by @dylwil3 on 2025-08-04 21:12_

---

_Branch deleted on 2025-08-05 05:58_

---

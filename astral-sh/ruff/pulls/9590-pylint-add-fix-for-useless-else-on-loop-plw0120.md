```yaml
number: 9590
title: "[`pylint`] Add fix for `useless-else-on-loop` (`PLW0120`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: add-autofix-to-PLW0120
created_at: 2024-01-20T07:11:59Z
updated_at: 2024-01-21T16:48:33Z
url: https://github.com/astral-sh/ruff/pull/9590
synced_at: 2026-01-12T15:55:29Z
```

# [`pylint`] Add fix for `useless-else-on-loop` (`PLW0120`)

---

_@diceroll123_

## Summary

adds fix for `useless-else-on-loop` / `PLW0120`.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2024-01-20 07:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+13 -13 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+11 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/configuration.py#L1640'>airflow/configuration.py:1640:9:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/configuration.py#L1640'>airflow/configuration.py:1640:9:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/amazon/aws/hooks/batch_client.py#L371'>airflow/providers/amazon/aws/hooks/batch_client.py:371:9:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/amazon/aws/hooks/batch_client.py#L371'>airflow/providers/amazon/aws/hooks/batch_client.py:371:9:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/amazon/aws/hooks/batch_client.py#L409'>airflow/providers/amazon/aws/hooks/batch_client.py:409:9:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/amazon/aws/hooks/batch_client.py#L409'>airflow/providers/amazon/aws/hooks/batch_client.py:409:9:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/amazon/aws/hooks/datasync.py#L318'>airflow/providers/amazon/aws/hooks/datasync.py:318:9:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/amazon/aws/hooks/datasync.py#L318'>airflow/providers/amazon/aws/hooks/datasync.py:318:9:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/google/cloud/hooks/bigquery.py#L2237'>airflow/providers/google/cloud/hooks/bigquery.py:2237:17:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/google/cloud/hooks/bigquery.py#L2237'>airflow/providers/google/cloud/hooks/bigquery.py:2237:17:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/google/cloud/hooks/bigquery.py#L3075'>airflow/providers/google/cloud/hooks/bigquery.py:3075:17:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/google/cloud/hooks/bigquery.py#L3075'>airflow/providers/google/cloud/hooks/bigquery.py:3075:17:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/google/cloud/hooks/gcs.py#L357'>airflow/providers/google/cloud/hooks/gcs.py:357:9:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/google/cloud/hooks/gcs.py#L357'>airflow/providers/google/cloud/hooks/gcs.py:357:9:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/http/hooks/http.py#L411'>airflow/providers/http/hooks/http.py:411:13:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/http/hooks/http.py#L411'>airflow/providers/http/hooks/http.py:411:13:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/jenkins/operators/jenkins_job_trigger.py#L181'>airflow/providers/jenkins/operators/jenkins_job_trigger.py:181:9:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/jenkins/operators/jenkins_job_trigger.py#L181'>airflow/providers/jenkins/operators/jenkins_job_trigger.py:181:9:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/scripts/in_container/run_generate_constraints.py#L317'>scripts/in_container/run_generate_constraints.py:317:13:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/scripts/in_container/run_generate_constraints.py#L317'>scripts/in_container/run_generate_constraints.py:317:13:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/tests/providers/elasticsearch/log/elasticmock/__init__.py#L85'>tests/providers/elasticsearch/log/elasticmock/__init__.py:85:5:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/tests/providers/elasticsearch/log/elasticmock/__init__.py#L85'>tests/providers/elasticsearch/log/elasticmock/__init__.py:85:5:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/7f07209d1cf5e5a21bc60ac49206f23704185cec/Packs/Carbon_Black_Enterprise_Response/Scripts/CBLiveGetFile/CBLiveGetFile.py#L142'>Packs/Carbon_Black_Enterprise_Response/Scripts/CBLiveGetFile/CBLiveGetFile.py:142:1:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/demisto/content/blob/7f07209d1cf5e5a21bc60ac49206f23704185cec/Packs/Carbon_Black_Enterprise_Response/Scripts/CBLiveGetFile/CBLiveGetFile.py#L142'>Packs/Carbon_Black_Enterprise_Response/Scripts/CBLiveGetFile/CBLiveGetFile.py:142:1:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/demisto/content/blob/7f07209d1cf5e5a21bc60ac49206f23704185cec/Packs/ThreatQ/Integrations/ThreatQ_v2/ThreatQ_v2.py#L589'>Packs/ThreatQ/Integrations/ThreatQ_v2/ThreatQ_v2.py:589:5:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/demisto/content/blob/7f07209d1cf5e5a21bc60ac49206f23704185cec/Packs/ThreatQ/Integrations/ThreatQ_v2/ThreatQ_v2.py#L589'>Packs/ThreatQ/Integrations/ThreatQ_v2/ThreatQ_v2.py:589:5:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0120 | 26 | 13 | 13 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+13 -13 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+11 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/configuration.py#L1640'>airflow/configuration.py:1640:9:</a> PLW0120 [*] `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/configuration.py#L1640'>airflow/configuration.py:1640:9:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/amazon/aws/hooks/batch_client.py#L371'>airflow/providers/amazon/aws/hooks/batch_client.py:371:9:</a> PLW0120 [*] `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/amazon/aws/hooks/batch_client.py#L371'>airflow/providers/amazon/aws/hooks/batch_client.py:371:9:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/amazon/aws/hooks/batch_client.py#L409'>airflow/providers/amazon/aws/hooks/batch_client.py:409:9:</a> PLW0120 [*] `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/amazon/aws/hooks/batch_client.py#L409'>airflow/providers/amazon/aws/hooks/batch_client.py:409:9:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/amazon/aws/hooks/datasync.py#L318'>airflow/providers/amazon/aws/hooks/datasync.py:318:9:</a> PLW0120 [*] `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/amazon/aws/hooks/datasync.py#L318'>airflow/providers/amazon/aws/hooks/datasync.py:318:9:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/google/cloud/hooks/bigquery.py#L2237'>airflow/providers/google/cloud/hooks/bigquery.py:2237:17:</a> PLW0120 [*] `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/google/cloud/hooks/bigquery.py#L2237'>airflow/providers/google/cloud/hooks/bigquery.py:2237:17:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/google/cloud/hooks/bigquery.py#L3075'>airflow/providers/google/cloud/hooks/bigquery.py:3075:17:</a> PLW0120 [*] `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/google/cloud/hooks/bigquery.py#L3075'>airflow/providers/google/cloud/hooks/bigquery.py:3075:17:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/google/cloud/hooks/gcs.py#L357'>airflow/providers/google/cloud/hooks/gcs.py:357:9:</a> PLW0120 [*] `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/google/cloud/hooks/gcs.py#L357'>airflow/providers/google/cloud/hooks/gcs.py:357:9:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/http/hooks/http.py#L411'>airflow/providers/http/hooks/http.py:411:13:</a> PLW0120 [*] `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/http/hooks/http.py#L411'>airflow/providers/http/hooks/http.py:411:13:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/jenkins/operators/jenkins_job_trigger.py#L181'>airflow/providers/jenkins/operators/jenkins_job_trigger.py:181:9:</a> PLW0120 [*] `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/airflow/providers/jenkins/operators/jenkins_job_trigger.py#L181'>airflow/providers/jenkins/operators/jenkins_job_trigger.py:181:9:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/scripts/in_container/run_generate_constraints.py#L317'>scripts/in_container/run_generate_constraints.py:317:13:</a> PLW0120 [*] `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/scripts/in_container/run_generate_constraints.py#L317'>scripts/in_container/run_generate_constraints.py:317:13:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/tests/providers/elasticsearch/log/elasticmock/__init__.py#L85'>tests/providers/elasticsearch/log/elasticmock/__init__.py:85:5:</a> PLW0120 [*] `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/apache/airflow/blob/77f9f02865a6fe0ceb0b408bab3bbaef5c32438a/tests/providers/elasticsearch/log/elasticmock/__init__.py#L85'>tests/providers/elasticsearch/log/elasticmock/__init__.py:85:5:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/7f07209d1cf5e5a21bc60ac49206f23704185cec/Packs/Carbon_Black_Enterprise_Response/Scripts/CBLiveGetFile/CBLiveGetFile.py#L142'>Packs/Carbon_Black_Enterprise_Response/Scripts/CBLiveGetFile/CBLiveGetFile.py:142:1:</a> PLW0120 [*] `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/demisto/content/blob/7f07209d1cf5e5a21bc60ac49206f23704185cec/Packs/Carbon_Black_Enterprise_Response/Scripts/CBLiveGetFile/CBLiveGetFile.py#L142'>Packs/Carbon_Black_Enterprise_Response/Scripts/CBLiveGetFile/CBLiveGetFile.py:142:1:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
+ <a href='https://github.com/demisto/content/blob/7f07209d1cf5e5a21bc60ac49206f23704185cec/Packs/ThreatQ/Integrations/ThreatQ_v2/ThreatQ_v2.py#L589'>Packs/ThreatQ/Integrations/ThreatQ_v2/ThreatQ_v2.py:589:5:</a> PLW0120 [*] `else` clause on loop without a `break` statement; remove the `else` and dedent its contents
- <a href='https://github.com/demisto/content/blob/7f07209d1cf5e5a21bc60ac49206f23704185cec/Packs/ThreatQ/Integrations/ThreatQ_v2/ThreatQ_v2.py#L589'>Packs/ThreatQ/Integrations/ThreatQ_v2/ThreatQ_v2.py:589:5:</a> PLW0120 `else` clause on loop without a `break` statement; remove the `else` and de-indent all the code inside it
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0120 | 26 | 13 | 13 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @diceroll123 on 2024-01-20 08:15_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-01-21 03:57_

---

_Label `autofix` added by @charliermarsh on 2024-01-21 03:57_

---

_Renamed from "[`pylint`] - add fix for `useless-else-on-loop` / `PLW0120`" to "[`pylint`] Add fix for `useless-else-on-loop` (`PLW0120`)" by @charliermarsh on 2024-01-21 03:57_

---

_Comment by @charliermarsh on 2024-01-21 03:57_

Thanks @diceroll123, these are great! I know I'm behind on these reviews but I will get to them over time.

---

_Comment by @diceroll123 on 2024-01-21 04:00_

Thanks, and no rush! 😄 It's the weekend, take it easy.

---

_@charliermarsh approved on 2024-01-21 15:53_

Looks great!

---

_@charliermarsh reviewed on 2024-01-21 15:54_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/useless_else_on_loop.rs`:136 on 2024-01-21 15:54_

I moved the handling out to its own function so that it can return `Result` and make all the error cases explicit (rather than using `unwrap`, which panics on failure).

---

_@charliermarsh reviewed on 2024-01-21 15:54_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/useless_else_on_loop.rs`:179 on 2024-01-21 15:54_

I simplified this a bit, but we now drop comments on the same line as the `else:`. I think this is ok.

---

_Label `preview` added by @charliermarsh on 2024-01-21 15:55_

---

_@charliermarsh reviewed on 2024-01-21 15:58_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/useless_else_on_loop.rs`:179 on 2024-01-21 15:58_

@diceroll123 - Did you feel strongly about preserving these? I can see why it might be useful, but the comments _do_ get moved, right? Like:

```python
else:  # some comment
  x = 1
  y = 2
```

Was getting moved to:

```python
x = 1
y = 2  # some comment
```

Which struct me as potentially-wrong.

For example, in this case:

```python
def test_retain_comment():
    """Retain the comment within the `else` block"""
    for j in range(10):
        pass
    else:  # [useless-else-on-loop]
        print("fat chance")
        for j in range(10):
            break

```

The comment was moved to after the _break_.

---

_Merged by @charliermarsh on 2024-01-21 16:17_

---

_Closed by @charliermarsh on 2024-01-21 16:17_

---

_@charliermarsh reviewed on 2024-01-21 16:18_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/useless_else_on_loop.rs`:179 on 2024-01-21 16:18_

(Merging to keep things moving but happy to revisit.)

---

_@diceroll123 reviewed on 2024-01-21 16:48_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/pylint/rules/useless_else_on_loop.rs`:179 on 2024-01-21 16:48_

No strong feelings either way! It's much simpler without preserving comments on the else line. 

---

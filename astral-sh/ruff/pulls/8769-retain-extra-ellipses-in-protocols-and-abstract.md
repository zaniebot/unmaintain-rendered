```yaml
number: 8769
title: Retain extra ellipses in protocols and abstract methods
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pie
created_at: 2023-11-19T14:50:10Z
updated_at: 2023-11-19T15:05:31Z
url: https://github.com/astral-sh/ruff/pull/8769
synced_at: 2026-01-12T15:55:26Z
```

# Retain extra ellipses in protocols and abstract methods

---

_@charliermarsh_

## Summary

It turns out that some type checkers rely on the presence of ellipses in `Protocol` interfaces and abstract methods, in order to differentiate between default implementations and stubs. This PR modifies the preview behavior of `PIE790` to avoid flagging "unnecessary" ellipses in such cases.

Closes https://github.com/astral-sh/ruff/issues/8756.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2023-11-19 14:50_

---

_Comment by @github-actions[bot] on 2023-11-19 15:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -24 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -18 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/efecebde3274fcfa833edbcd0fea98473e7a4873/airflow/hooks/base.py#L168'>airflow/hooks/base.py:168:9:</a> PIE790 [*] Unnecessary `...` literal
- <a href='https://github.com/apache/airflow/blob/efecebde3274fcfa833edbcd0fea98473e7a4873/airflow/hooks/base.py#L188'>airflow/hooks/base.py:188:9:</a> PIE790 [*] Unnecessary `...` literal
- <a href='https://github.com/apache/airflow/blob/efecebde3274fcfa833edbcd0fea98473e7a4873/airflow/metrics/protocols.py#L40'>airflow/metrics/protocols.py:40:9:</a> PIE790 [*] Unnecessary `...` literal
- <a href='https://github.com/apache/airflow/blob/efecebde3274fcfa833edbcd0fea98473e7a4873/airflow/metrics/protocols.py#L44'>airflow/metrics/protocols.py:44:9:</a> PIE790 [*] Unnecessary `...` literal
- <a href='https://github.com/apache/airflow/blob/efecebde3274fcfa833edbcd0fea98473e7a4873/airflow/notifications/basenotifier.py#L81'>airflow/notifications/basenotifier.py:81:9:</a> PIE790 [*] Unnecessary `...` literal
- <a href='https://github.com/apache/airflow/blob/efecebde3274fcfa833edbcd0fea98473e7a4873/airflow/providers/amazon/aws/hooks/batch_client.py#L125'>airflow/providers/amazon/aws/hooks/batch_client.py:125:9:</a> PIE790 [*] Unnecessary `...` literal
- <a href='https://github.com/apache/airflow/blob/efecebde3274fcfa833edbcd0fea98473e7a4873/airflow/providers/amazon/aws/hooks/batch_client.py#L137'>airflow/providers/amazon/aws/hooks/batch_client.py:137:9:</a> PIE790 [*] Unnecessary `...` literal
- <a href='https://github.com/apache/airflow/blob/efecebde3274fcfa833edbcd0fea98473e7a4873/airflow/providers/amazon/aws/hooks/batch_client.py#L68'>airflow/providers/amazon/aws/hooks/batch_client.py:68:9:</a> PIE790 [*] Unnecessary `...` literal
- <a href='https://github.com/apache/airflow/blob/efecebde3274fcfa833edbcd0fea98473e7a4873/airflow/providers/amazon/aws/hooks/batch_client.py#L94'>airflow/providers/amazon/aws/hooks/batch_client.py:94:9:</a> PIE790 [*] Unnecessary `...` literal
- <a href='https://github.com/apache/airflow/blob/efecebde3274fcfa833edbcd0fea98473e7a4873/airflow/providers/amazon/aws/hooks/ecs.py#L159'>airflow/providers/amazon/aws/hooks/ecs.py:159:9:</a> PIE790 [*] Unnecessary `...` literal
- <a href='https://github.com/apache/airflow/blob/efecebde3274fcfa833edbcd0fea98473e7a4873/airflow/providers/amazon/aws/hooks/ecs.py#L166'>airflow/providers/amazon/aws/hooks/ecs.py:166:9:</a> PIE790 [*] Unnecessary `...` literal
- <a href='https://github.com/apache/airflow/blob/efecebde3274fcfa833edbcd0fea98473e7a4873/airflow/providers/amazon/aws/hooks/ecs.py#L173'>airflow/providers/amazon/aws/hooks/ecs.py:173:9:</a> PIE790 [*] Unnecessary `...` literal
- <a href='https://github.com/apache/airflow/blob/efecebde3274fcfa833edbcd0fea98473e7a4873/airflow/providers/amazon/aws/hooks/ecs.py#L180'>airflow/providers/amazon/aws/hooks/ecs.py:180:9:</a> PIE790 [*] Unnecessary `...` literal
- <a href='https://github.com/apache/airflow/blob/efecebde3274fcfa833edbcd0fea98473e7a4873/airflow/providers/amazon/aws/hooks/ecs.py#L187'>airflow/providers/amazon/aws/hooks/ecs.py:187:9:</a> PIE790 [*] Unnecessary `...` literal
- <a href='https://github.com/apache/airflow/blob/efecebde3274fcfa833edbcd0fea98473e7a4873/airflow/providers/amazon/aws/hooks/ecs.py#L194'>airflow/providers/amazon/aws/hooks/ecs.py:194:9:</a> PIE790 [*] Unnecessary `...` literal
- <a href='https://github.com/apache/airflow/blob/efecebde3274fcfa833edbcd0fea98473e7a4873/airflow/serialization/json_schema.py#L43'>airflow/serialization/json_schema.py:43:9:</a> PIE790 [*] Unnecessary `...` literal
- <a href='https://github.com/apache/airflow/blob/efecebde3274fcfa833edbcd0fea98473e7a4873/airflow/serialization/json_schema.py#L47'>airflow/serialization/json_schema.py:47:9:</a> PIE790 [*] Unnecessary `...` literal
- <a href='https://github.com/apache/airflow/blob/efecebde3274fcfa833edbcd0fea98473e7a4873/airflow/serialization/json_schema.py#L51'>airflow/serialization/json_schema.py:51:9:</a> PIE790 [*] Unnecessary `...` literal
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/96243416771109fbaf383167470ac2481fe938ef/ibis/common/patterns.py#L208'>ibis/common/patterns.py:208:9:</a> PIE790 [*] Unnecessary `...` literal
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+0 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/scikit-build/scikit-build-core/blob/de72cb49fcf57fd6d2025a60338f5d58272933d1/src/scikit_build_core/settings/sources.py#L154'>src/scikit_build_core/settings/sources.py:154:9:</a> PIE790 [*] Unnecessary `...` literal
- <a href='https://github.com/scikit-build/scikit-build-core/blob/de72cb49fcf57fd6d2025a60338f5d58272933d1/src/scikit_build_core/settings/sources.py#L161'>src/scikit_build_core/settings/sources.py:161:9:</a> PIE790 [*] Unnecessary `...` literal
- <a href='https://github.com/scikit-build/scikit-build-core/blob/de72cb49fcf57fd6d2025a60338f5d58272933d1/src/scikit_build_core/settings/sources.py#L169'>src/scikit_build_core/settings/sources.py:169:9:</a> PIE790 [*] Unnecessary `...` literal
- <a href='https://github.com/scikit-build/scikit-build-core/blob/de72cb49fcf57fd6d2025a60338f5d58272933d1/src/scikit_build_core/settings/sources.py#L177'>src/scikit_build_core/settings/sources.py:177:9:</a> PIE790 [*] Unnecessary `...` literal
- <a href='https://github.com/scikit-build/scikit-build-core/blob/de72cb49fcf57fd6d2025a60338f5d58272933d1/src/scikit_build_core/settings/sources.py#L184'>src/scikit_build_core/settings/sources.py:184:9:</a> PIE790 [*] Unnecessary `...` literal
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PIE790 | 24 | 0 | 24 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2023-11-19 15:05_

The ecosystem changes are correct (they were false positives given this new information).

---

_Merged by @charliermarsh on 2023-11-19 15:05_

---

_Closed by @charliermarsh on 2023-11-19 15:05_

---

_Branch deleted on 2023-11-19 15:05_

---

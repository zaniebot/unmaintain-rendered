```yaml
number: 8641
title: "Extend `unnecessary-pass` (`PIE790`) to include ellipses in preview"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: charlie/pie
created_at: 2023-11-13T02:28:16Z
updated_at: 2023-11-13T19:35:55Z
url: https://github.com/astral-sh/ruff/pull/8641
synced_at: 2026-01-12T15:55:26Z
```

# Extend `unnecessary-pass` (`PIE790`) to include ellipses in preview

---

_@charliermarsh_

## Summary

This PR extends `unnecessary-pass` (`PIE790`) to flag unnecessary ellipsis expressions in addition to `pass` statements. A `pass` is equivalent to a standalone `...`, so it feels correct to me that a single rule should cover both cases.

When we look to v0.2.0, we should also consider deprecating `PYI013`, which flags ellipses only for classes.

Closes https://github.com/astral-sh/ruff/issues/8602.


---

_Label `rule` added by @charliermarsh on 2023-11-13 02:28_

---

_Label `preview` added by @charliermarsh on 2023-11-13 02:28_

---

_Review requested from @zanieb by @charliermarsh on 2023-11-13 02:28_

---

_Comment by @github-actions[bot] on 2023-11-13 02:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+29 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+22 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/airflow/hooks/base.py#L168'>airflow/hooks/base.py:168:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/airflow/hooks/base.py#L188'>airflow/hooks/base.py:188:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/airflow/jobs/scheduler_job_runner.py#L1586'>airflow/jobs/scheduler_job_runner.py:1586:13:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/airflow/metrics/protocols.py#L40'>airflow/metrics/protocols.py:40:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/airflow/metrics/protocols.py#L44'>airflow/metrics/protocols.py:44:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/airflow/metrics/validators.py#L44'>airflow/metrics/validators.py:44:5:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/airflow/notifications/basenotifier.py#L81'>airflow/notifications/basenotifier.py:81:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/airflow/providers/amazon/aws/hooks/batch_client.py#L125'>airflow/providers/amazon/aws/hooks/batch_client.py:125:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/airflow/providers/amazon/aws/hooks/batch_client.py#L137'>airflow/providers/amazon/aws/hooks/batch_client.py:137:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/airflow/providers/amazon/aws/hooks/batch_client.py#L68'>airflow/providers/amazon/aws/hooks/batch_client.py:68:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/airflow/providers/amazon/aws/hooks/batch_client.py#L94'>airflow/providers/amazon/aws/hooks/batch_client.py:94:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/airflow/providers/amazon/aws/hooks/ecs.py#L159'>airflow/providers/amazon/aws/hooks/ecs.py:159:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/airflow/providers/amazon/aws/hooks/ecs.py#L166'>airflow/providers/amazon/aws/hooks/ecs.py:166:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/airflow/providers/amazon/aws/hooks/ecs.py#L173'>airflow/providers/amazon/aws/hooks/ecs.py:173:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/airflow/providers/amazon/aws/hooks/ecs.py#L180'>airflow/providers/amazon/aws/hooks/ecs.py:180:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/airflow/providers/amazon/aws/hooks/ecs.py#L187'>airflow/providers/amazon/aws/hooks/ecs.py:187:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/airflow/providers/amazon/aws/hooks/ecs.py#L194'>airflow/providers/amazon/aws/hooks/ecs.py:194:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/airflow/serialization/json_schema.py#L43'>airflow/serialization/json_schema.py:43:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/airflow/serialization/json_schema.py#L47'>airflow/serialization/json_schema.py:47:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/airflow/serialization/json_schema.py#L51'>airflow/serialization/json_schema.py:51:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/airflow/www/security.py#L43'>airflow/www/security.py:43:5:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/apache/airflow/blob/1a5a272312f31ff8481b647ea1f4616af7e5b4fe/docs/exts/substitution_extensions.py#L120'>docs/exts/substitution_extensions.py:120:9:</a> PIE790 [*] Unnecessary `...` literal
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/79b3c261d5ff72d9a367a01772b078523ea8f472/Packs/TrendMicroEmailSecurity/Integrations/TrendMicroEmailSecurityEventCollector/TrendMicroEmailSecurityEventCollector.py#L70'>Packs/TrendMicroEmailSecurity/Integrations/TrendMicroEmailSecurityEventCollector/TrendMicroEmailSecurityEventCollector.py:70:5:</a> PIE790 [*] Unnecessary `...` literal
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/7dbbeb2f631c367471dc849c13d7e66dde934840/ibis/common/patterns.py#L208'>ibis/common/patterns.py:208:9:</a> PIE790 [*] Unnecessary `...` literal
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/c67339b4493e1df4a5ff10d92a72cc362db18d0e/src/scikit_build_core/settings/sources.py#L154'>src/scikit_build_core/settings/sources.py:154:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/c67339b4493e1df4a5ff10d92a72cc362db18d0e/src/scikit_build_core/settings/sources.py#L161'>src/scikit_build_core/settings/sources.py:161:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/c67339b4493e1df4a5ff10d92a72cc362db18d0e/src/scikit_build_core/settings/sources.py#L169'>src/scikit_build_core/settings/sources.py:169:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/c67339b4493e1df4a5ff10d92a72cc362db18d0e/src/scikit_build_core/settings/sources.py#L177'>src/scikit_build_core/settings/sources.py:177:9:</a> PIE790 [*] Unnecessary `...` literal
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/c67339b4493e1df4a5ff10d92a72cc362db18d0e/src/scikit_build_core/settings/sources.py#L184'>src/scikit_build_core/settings/sources.py:184:9:</a> PIE790 [*] Unnecessary `...` literal
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PIE790 | 29 | 29 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_placeholder.rs`:16 on 2023-11-13 04:07_

Should we use the term "literal" here instead of "expression"? This applies to wherever this is being used.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_placeholder.rs`:34 on 2023-11-13 04:10_

Can we add an example with `...`, probably stating that this is only applicable in preview mode?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_pie/snapshots/ruff_linter__rules__flake8_pie__tests__preview__PIE790_PIE790.py.snap`:522 on 2023-11-13 04:13_

I'm not sure if this is a bug in the diff printer or the range is incorrect but the violation is on line 165 while the fix is removing the `...` literal from the _next_ line (166).

---

_@dhruvmanila approved on 2023-11-13 04:13_

---

_Merged by @charliermarsh on 2023-11-13 19:28_

---

_Closed by @charliermarsh on 2023-11-13 19:28_

---

_Branch deleted on 2023-11-13 19:28_

---

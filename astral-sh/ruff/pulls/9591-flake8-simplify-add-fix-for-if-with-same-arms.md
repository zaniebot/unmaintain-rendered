```yaml
number: 9591
title: "[`flake8-simplify`] Add fix for `if-with-same-arms` (`SIM114`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: add-autofix-for-SIM114
created_at: 2024-01-20T09:41:03Z
updated_at: 2024-01-22T04:45:45Z
url: https://github.com/astral-sh/ruff/pull/9591
synced_at: 2026-01-12T15:55:29Z
```

# [`flake8-simplify`] Add fix for `if-with-same-arms` (`SIM114`)

---

_@diceroll123_

## Summary

 add fix for `if-with-same-arms` / `SIM114`

Also preserves comments!

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2024-01-20 10:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +60 -0 fixes in 4 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +20 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/common/sql/operators/sql.py#L791'>airflow/providers/common/sql/operators/sql.py:791:9:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/providers/common/sql/operators/sql.py#L791'>airflow/providers/common/sql/operators/sql.py:791:9:</a> SIM114 [*] Combine `if` branches using logical `or` operator
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/utils/dates.py#L97'>airflow/utils/dates.py:97:5:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/utils/dates.py#L97'>airflow/utils/dates.py:97:5:</a> SIM114 [*] Combine `if` branches using logical `or` operator
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/utils/task_group.py#L376'>airflow/utils/task_group.py:376:17:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/airflow/utils/task_group.py#L376'>airflow/utils/task_group.py:376:17:</a> SIM114 [*] Combine `if` branches using logical `or` operator
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/dev/breeze/src/airflow_breeze/commands/ci_commands.py#L357'>dev/breeze/src/airflow_breeze/commands/ci_commands.py:357:5:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/dev/breeze/src/airflow_breeze/commands/ci_commands.py#L357'>dev/breeze/src/airflow_breeze/commands/ci_commands.py:357:5:</a> SIM114 [*] Combine `if` branches using logical `or` operator
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/dev/breeze/src/airflow_breeze/commands/ci_commands.py#L361'>dev/breeze/src/airflow_breeze/commands/ci_commands.py:361:5:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/dev/breeze/src/airflow_breeze/commands/ci_commands.py#L361'>dev/breeze/src/airflow_breeze/commands/ci_commands.py:361:5:</a> SIM114 [*] Combine `if` branches using logical `or` operator
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/dev/breeze/src/airflow_breeze/utils/confirm.py#L110'>dev/breeze/src/airflow_breeze/utils/confirm.py:110:5:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/dev/breeze/src/airflow_breeze/utils/confirm.py#L110'>dev/breeze/src/airflow_breeze/utils/confirm.py:110:5:</a> SIM114 [*] Combine `if` branches using logical `or` operator
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/docs/conf.py#L765'>docs/conf.py:765:1:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/docs/conf.py#L765'>docs/conf.py:765:1:</a> SIM114 [*] Combine `if` branches using logical `or` operator
- <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/docs/conf.py#L858'>docs/conf.py:858:5:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/apache/airflow/blob/d48985c8f2e57171674505830d5d13bec4dd5f8e/docs/conf.py#L858'>docs/conf.py:858:5:</a> SIM114 [*] Combine `if` branches using logical `or` operator
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/models/plots.py#L471'>src/bokeh/models/plots.py:471:17:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/models/plots.py#L471'>src/bokeh/models/plots.py:471:17:</a> SIM114 [*] Combine `if` branches using logical `or` operator
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/models/plots.py#L478'>src/bokeh/models/plots.py:478:17:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/models/plots.py#L478'>src/bokeh/models/plots.py:478:17:</a> SIM114 [*] Combine `if` branches using logical `or` operator
- <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/util/serialization.py#L190'>src/bokeh/util/serialization.py:190:5:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/util/serialization.py#L190'>src/bokeh/util/serialization.py:190:5:</a> SIM114 [*] Combine `if` branches using logical `or` operator
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/reflex-dev/reflex/blob/b305f895a8b471e679f0e26c5674ed22ed5d37a5/scripts/pyi_generator.py#L596'>scripts/pyi_generator.py:596:13:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/reflex-dev/reflex/blob/b305f895a8b471e679f0e26c5674ed22ed5d37a5/scripts/pyi_generator.py#L596'>scripts/pyi_generator.py:596:13:</a> SIM114 [*] Combine `if` branches using logical `or` operator
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +32 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/corporate/lib/stripe.py#L3723'>corporate/lib/stripe.py:3723:9:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/corporate/lib/stripe.py#L3723'>corporate/lib/stripe.py:3723:9:</a> SIM114 [*] Combine `if` branches using logical `or` operator
- <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/corporate/lib/stripe.py#L4148'>corporate/lib/stripe.py:4148:9:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/corporate/lib/stripe.py#L4148'>corporate/lib/stripe.py:4148:9:</a> SIM114 [*] Combine `if` branches using logical `or` operator
- <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/corporate/lib/stripe.py#L4158'>corporate/lib/stripe.py:4158:9:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/corporate/lib/stripe.py#L4158'>corporate/lib/stripe.py:4158:9:</a> SIM114 [*] Combine `if` branches using logical `or` operator
- <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/scripts/lib/supervisor.py#L74'>scripts/lib/supervisor.py:74:9:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/scripts/lib/supervisor.py#L74'>scripts/lib/supervisor.py:74:9:</a> SIM114 [*] Combine `if` branches using logical `or` operator
- <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/tools/droplets/cleanup.py#L39'>tools/droplets/cleanup.py:39:9:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/tools/droplets/cleanup.py#L39'>tools/droplets/cleanup.py:39:9:</a> SIM114 [*] Combine `if` branches using logical `or` operator
- <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/tools/lib/provision.py#L93'>tools/lib/provision.py:93:1:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/tools/lib/provision.py#L93'>tools/lib/provision.py:93:1:</a> SIM114 [*] Combine `if` branches using logical `or` operator
- <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/zerver/actions/realm_settings.py#L562'>zerver/actions/realm_settings.py:562:5:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/zerver/actions/realm_settings.py#L562'>zerver/actions/realm_settings.py:562:5:</a> SIM114 [*] Combine `if` branches using logical `or` operator
- <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/zerver/lib/import_realm.py#L495'>zerver/lib/import_realm.py:495:13:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/zerver/lib/import_realm.py#L495'>zerver/lib/import_realm.py:495:13:</a> SIM114 [*] Combine `if` branches using logical `or` operator
- <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/zerver/models/users.py#L625'>zerver/models/users.py:625:9:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/zerver/models/users.py#L625'>zerver/models/users.py:625:9:</a> SIM114 [*] Combine `if` branches using logical `or` operator
- <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/zerver/signals.py#L56'>zerver/signals.py:56:5:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/zerver/signals.py#L56'>zerver/signals.py:56:5:</a> SIM114 [*] Combine `if` branches using logical `or` operator
- <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/zerver/views/documentation.py#L94'>zerver/views/documentation.py:94:9:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/zerver/views/documentation.py#L94'>zerver/views/documentation.py:94:9:</a> SIM114 [*] Combine `if` branches using logical `or` operator
- <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/zerver/views/registration.py#L545'>zerver/views/registration.py:545:17:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/zerver/views/registration.py#L545'>zerver/views/registration.py:545:17:</a> SIM114 [*] Combine `if` branches using logical `or` operator
- <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/zerver/webhooks/clubhouse/view.py#L102'>zerver/webhooks/clubhouse/view.py:102:9:</a> SIM114 Combine `if` branches using logical `or` operator
+ <a href='https://github.com/zulip/zulip/blob/60225591dcc947c61b1d13202942d86886a42839/zerver/webhooks/clubhouse/view.py#L102'>zerver/webhooks/clubhouse/view.py:102:9:</a> SIM114 [*] Combine `if` branches using logical `or` operator
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM114 | 60 | 0 | 0 | 60 | 0 |

</p>
</details>




---

_Renamed from "[`flake8-simplify`] -  add fix for `if-with-same-arms` / `SIM114`" to "[`flake8-simplify`] -  add fix for `if-with-same-arms` (`SIM114`)" by @diceroll123 on 2024-01-21 04:34_

---

_@charliermarsh approved on 2024-01-22 04:29_

---

_Renamed from "[`flake8-simplify`] -  add fix for `if-with-same-arms` (`SIM114`)" to "[`flake8-simplify`] Add fix for `if-with-same-arms` (`SIM114`)" by @charliermarsh on 2024-01-22 04:29_

---

_Label `autofix` added by @charliermarsh on 2024-01-22 04:29_

---

_Label `preview` added by @charliermarsh on 2024-01-22 04:29_

---

_Merged by @charliermarsh on 2024-01-22 04:37_

---

_Closed by @charliermarsh on 2024-01-22 04:37_

---

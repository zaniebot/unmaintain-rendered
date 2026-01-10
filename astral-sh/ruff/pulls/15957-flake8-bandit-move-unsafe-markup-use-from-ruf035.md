```yaml
number: 15957
title: "[`flake8-bandit`] Move `unsafe-markup-use` from `RUF035` to `S704`"
type: pull_request
state: merged
author: Daverball
labels:
  - breaking
assignees: []
merged: true
base: micha/ruff-0.10
head: recategorize-RUF035-to-S704
created_at: 2025-02-05T08:15:50Z
updated_at: 2025-03-11T12:24:26Z
url: https://github.com/astral-sh/ruff/pull/15957
synced_at: 2026-01-10T19:49:01Z
```

# [`flake8-bandit`] Move `unsafe-markup-use` from `RUF035` to `S704`

---

_Pull request opened by @Daverball on 2025-02-05 08:15_

## Summary

`RUF035` has been backported into bandit as `S704` in this [PR](https://github.com/PyCQA/bandit/pull/1225)

This moves the rule and its corresponding setting to the `flake8-bandit` category

## Test Plan

`cargo nextest run`


---

_Added to milestone `v0.10` by @MichaReiser on 2025-02-05 08:16_

---

_Label `breaking` added by @MichaReiser on 2025-02-05 08:16_

---

_Comment by @Daverball on 2025-02-05 08:17_

@MichaReiser Can you think of a clever way to default the settings in their new section to the value in their deprecated section?

---

_Comment by @MichaReiser on 2025-02-05 08:17_

Thanks, this has to wait for the next minor release because it's breaking (selecting `RUF` will no longer select `RUF035` and selecting `S` now selects `S704`)

---

_Comment by @github-actions[bot] on 2025-02-05 08:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+30 -9 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/providers/fab/src/airflow/providers/fab/auth_manager/security_manager/override.py#L2307'>providers/fab/src/airflow/providers/fab/auth_manager/security_manager/override.py:2307:19:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/providers/fab/src/airflow/providers/fab/auth_manager/security_manager/override.py#L2307'>providers/fab/src/airflow/providers/fab/auth_manager/security_manager/override.py:2307:19:</a> S704 Unsafe use of `markupsafe.Markup` detected
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+7 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/connectors/sqla/models.py#L1308'>superset/connectors/sqla/models.py:1308:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/connectors/sqla/models.py#L1308'>superset/connectors/sqla/models.py:1308:16:</a> S704 Unsafe use of `markupsafe.Markup` detected
- <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/models/dashboard.py#L225'>superset/models/dashboard.py:225:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/models/dashboard.py#L225'>superset/models/dashboard.py:225:16:</a> S704 Unsafe use of `markupsafe.Markup` detected
- <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/models/helpers.py#L538'>superset/models/helpers.py:538:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/models/helpers.py#L538'>superset/models/helpers.py:538:16:</a> S704 Unsafe use of `markupsafe.Markup` detected
- <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/models/helpers.py#L567'>superset/models/helpers.py:567:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/models/helpers.py#L567'>superset/models/helpers.py:567:16:</a> S704 Unsafe use of `markupsafe.Markup` detected
- <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/models/slice.py#L338'>superset/models/slice.py:338:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/models/slice.py#L338'>superset/models/slice.py:338:16:</a> S704 Unsafe use of `markupsafe.Markup` detected
- <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/models/sql_lab.py#L445'>superset/models/sql_lab.py:445:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/models/sql_lab.py#L445'>superset/models/sql_lab.py:445:16:</a> S704 Unsafe use of `markupsafe.Markup` detected
- <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/utils/core.py#L485'>superset/utils/core.py:485:16:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/utils/core.py#L485'>superset/utils/core.py:485:16:</a> S704 Unsafe use of `markupsafe.Markup` detected
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+21 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/496c93abcef445e28acbda4e15b97a5b8366a848/securedrop/journalist_app/account.py#L100'>securedrop/journalist_app/account.py:100:20:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/496c93abcef445e28acbda4e15b97a5b8366a848/securedrop/journalist_app/account.py#L87'>securedrop/journalist_app/account.py:87:20:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/496c93abcef445e28acbda4e15b97a5b8366a848/securedrop/journalist_app/admin.py#L224'>securedrop/journalist_app/admin.py:224:32:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/496c93abcef445e28acbda4e15b97a5b8366a848/securedrop/journalist_app/admin.py#L279'>securedrop/journalist_app/admin.py:279:20:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/496c93abcef445e28acbda4e15b97a5b8366a848/securedrop/journalist_app/admin.py#L295'>securedrop/journalist_app/admin.py:295:20:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/496c93abcef445e28acbda4e15b97a5b8366a848/securedrop/journalist_app/col.py#L103'>securedrop/journalist_app/col.py:103:17:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/496c93abcef445e28acbda4e15b97a5b8366a848/securedrop/journalist_app/col.py#L75'>securedrop/journalist_app/col.py:75:13:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/496c93abcef445e28acbda4e15b97a5b8366a848/securedrop/journalist_app/main.py#L169'>securedrop/journalist_app/main.py:169:17:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/496c93abcef445e28acbda4e15b97a5b8366a848/securedrop/journalist_app/main.py#L192'>securedrop/journalist_app/main.py:192:21:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/496c93abcef445e28acbda4e15b97a5b8366a848/securedrop/journalist_app/main.py#L203'>securedrop/journalist_app/main.py:203:21:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/496c93abcef445e28acbda4e15b97a5b8366a848/securedrop/journalist_app/utils.py#L267'>securedrop/journalist_app/utils.py:267:9:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/496c93abcef445e28acbda4e15b97a5b8366a848/securedrop/journalist_app/utils.py#L337'>securedrop/journalist_app/utils.py:337:13:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/496c93abcef445e28acbda4e15b97a5b8366a848/securedrop/journalist_app/utils.py#L366'>securedrop/journalist_app/utils.py:366:13:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/496c93abcef445e28acbda4e15b97a5b8366a848/securedrop/journalist_app/utils.py#L380'>securedrop/journalist_app/utils.py:380:13:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/496c93abcef445e28acbda4e15b97a5b8366a848/securedrop/journalist_app/utils.py#L503'>securedrop/journalist_app/utils.py:503:17:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/496c93abcef445e28acbda4e15b97a5b8366a848/securedrop/journalist_app/utils.py#L519'>securedrop/journalist_app/utils.py:519:13:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/496c93abcef445e28acbda4e15b97a5b8366a848/securedrop/source_app/utils.py#L37'>securedrop/source_app/utils.py:37:16:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/496c93abcef445e28acbda4e15b97a5b8366a848/securedrop/source_app/utils.py#L44'>securedrop/source_app/utils.py:44:11:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/496c93abcef445e28acbda4e15b97a5b8366a848/securedrop/source_app/utils.py#L51'>securedrop/source_app/utils.py:51:22:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/496c93abcef445e28acbda4e15b97a5b8366a848/securedrop/source_app/utils.py#L65'>securedrop/source_app/utils.py:65:11:</a> S704 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/freedomofpress/securedrop/blob/496c93abcef445e28acbda4e15b97a5b8366a848/securedrop/template_filters.py#L24'>securedrop/template_filters.py:24:21:</a> S704 Unsafe use of `markupsafe.Markup` detected
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/834e69d17c254827d9d797abc22d227c3ffcdadf/zerver/views/documentation.py#L292'>zerver/views/documentation.py:292:35:</a> RUF035 Unsafe use of `markupsafe.Markup` detected
+ <a href='https://github.com/zulip/zulip/blob/834e69d17c254827d9d797abc22d227c3ffcdadf/zerver/views/documentation.py#L292'>zerver/views/documentation.py:292:35:</a> S704 Unsafe use of `markupsafe.Markup` detected
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S704 | 30 | 30 | 0 | 0 | 0 |
| RUF035 | 9 | 0 | 9 | 0 | 0 |

</p>
</details>




---

_Comment by @MichaReiser on 2025-02-05 08:33_

> @MichaReiser Can you think of a clever way to default the settings in their new section to the value in their deprecated section?

Clever, no :) But what I'd do is to lift the `Flake8Options::into_settings` into `Configuration::into_settings` because that's where you have access to both the `Ruff` and `Flake8` options. Or you change the `Flake8Options::into_settings` to take a second argument that's an `Option<&RuffOptions>`

---

_Comment by @MichaReiser on 2025-03-11 12:00_

Thanks @Daverball for rebasing

---

_Merged by @MichaReiser on 2025-03-11 12:19_

---

_Closed by @MichaReiser on 2025-03-11 12:19_

---

_Branch deleted on 2025-03-11 12:24_

---

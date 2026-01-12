```yaml
number: 16092
title: "[`flake8-builtins`] Remove `builtins-` prefix from option names"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - breaking
  - configuration
assignees: []
merged: true
base: micha/ruff-0.10
head: builtins-options
created_at: 2025-02-11T01:51:00Z
updated_at: 2025-03-11T15:54:27Z
url: https://github.com/astral-sh/ruff/pull/16092
synced_at: 2026-01-12T15:55:53Z
```

# [`flake8-builtins`] Remove `builtins-` prefix from option names

---

_@InSyncWithFoo_

## Summary

Resolves #15368.

The following options have been renamed:

* `builtins-allowed-modules` &rarr; `allowed-modules`
* `builtins-ignorelist` &rarr; `ignorelist`
* `builtins-strict-checking` &rarr; `strict-checking`

To preserve compatibility, the old names are kept as Serde aliases.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-02-11 01:58_

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

_Label `configuration` added by @MichaReiser on 2025-02-11 07:24_

---

_Comment by @MichaReiser on 2025-02-11 07:25_

Thanks. I think the `builtins-ignorelist` and `builtins-allowed-modules` in the summary are incorrect. 

What we did in the past is to emit a deprecation warning for renamed settings so that we can remove the old names at some point in the future. It probably requires duplicating the fields on the `Options` structs and adding some custom code in `Settings::from_options` (I think it's called). Would you mind looking into this?

---

_Label `breaking` added by @MichaReiser on 2025-02-11 07:26_

---

_Added to milestone `v0.10` by @MichaReiser on 2025-02-11 07:26_

---

_Comment by @dhruvmanila on 2025-02-11 08:45_

As an additional reference, I think the infrastructure already exists and [`lint.extend-ignore`](https://docs.astral.sh/ruff/settings/#lint_extend-fixable) is an example of a deprecated setting.

---

_@dhruvmanila reviewed on 2025-02-12 05:53_

Looks good

---

_Merged by @MichaReiser on 2025-03-11 12:29_

---

_Closed by @MichaReiser on 2025-03-11 12:29_

---

_Branch deleted on 2025-03-11 15:54_

---

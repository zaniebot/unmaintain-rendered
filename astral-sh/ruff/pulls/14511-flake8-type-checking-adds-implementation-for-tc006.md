```yaml
number: 14511
title: "[`flake8-type-checking`] Adds implementation for TC006"
type: pull_request
state: merged
author: Daverball
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: feat/tc006
created_at: 2024-11-21T11:13:58Z
updated_at: 2024-11-22T14:29:43Z
url: https://github.com/astral-sh/ruff/pull/14511
synced_at: 2026-01-12T15:55:48Z
```

# [`flake8-type-checking`] Adds implementation for TC006

---

_@Daverball_

Now that `TCH` has been renamed to `TC` and the redirect from `TCH006` to `TCH010` can no longer bite us, I've added an implementation for this very simple rule.

## Summary

`TC006` checks for non-string literal arguments to `typing.cast` which adds unnecessary runtime overhead, since the function doesn't do anything at runtime.

This PR has a very tiny overlap with my other PR for `TCH007`/`TCH008` in `flake8_type_checking/helpers.rs` for adding the `quote_type_expression` function, but the two changes don't really depend on one another, so they can be merged in any order.

## Test Plan

`cargo nextest run`


---

_Comment by @github-actions[bot] on 2024-11-21 11:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+610 -1 violations, +0 -0 fixes in 11 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/d95db29ef84aae61b208853af954ea1ec08e54e3/disnake/abc.py#L1249'>disnake/abc.py:1249:25:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/d95db29ef84aae61b208853af954ea1ec08e54e3/disnake/abc.py#L299'>disnake/abc.py:299:25:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/d95db29ef84aae61b208853af954ea1ec08e54e3/disnake/components.py#L755'>disnake/components.py:755:27:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/d95db29ef84aae61b208853af954ea1ec08e54e3/disnake/ext/commands/base_core.py#L196'>disnake/ext/commands/base_core.py:196:43:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/d95db29ef84aae61b208853af954ea1ec08e54e3/disnake/ext/commands/core.py#L1950'>disnake/ext/commands/core.py:1950:19:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/d95db29ef84aae61b208853af954ea1ec08e54e3/disnake/ext/commands/core.py#L1980'>disnake/ext/commands/core.py:1980:19:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/d95db29ef84aae61b208853af954ea1ec08e54e3/disnake/interactions/base.py#L197'>disnake/interactions/base.py:197:37:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/d95db29ef84aae61b208853af954ea1ec08e54e3/docs/extensions/fulltoc.py#L122'>docs/extensions/fulltoc.py:122:34:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/d95db29ef84aae61b208853af954ea1ec08e54e3/tests/test_abc.py#L165'>tests/test_abc.py:165:30:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/DisnakeDev/disnake/blob/d95db29ef84aae61b208853af954ea1ec08e54e3/tests/test_abc.py#L166'>tests/test_abc.py:166:30:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+176 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/cf2656179a87cdc0d643affb8cc08b36143d401a/airflow/api/auth/backend/deny_all.py#L44'>airflow/api/auth/backend/deny_all.py:44:17:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/cf2656179a87cdc0d643affb8cc08b36143d401a/airflow/api_connexion/endpoints/request_dict.py#L26'>airflow/api_connexion/endpoints/request_dict.py:26:17:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/cf2656179a87cdc0d643affb8cc08b36143d401a/airflow/api_connexion/parameters.py#L106'>airflow/api_connexion/parameters.py:106:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/cf2656179a87cdc0d643affb8cc08b36143d401a/airflow/api_connexion/security.py#L107'>airflow/api_connexion/security.py:107:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/cf2656179a87cdc0d643affb8cc08b36143d401a/airflow/api_connexion/security.py#L156'>airflow/api_connexion/security.py:156:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/cf2656179a87cdc0d643affb8cc08b36143d401a/airflow/api_connexion/security.py#L175'>airflow/api_connexion/security.py:175:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/cf2656179a87cdc0d643affb8cc08b36143d401a/airflow/api_connexion/security.py#L194'>airflow/api_connexion/security.py:194:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/cf2656179a87cdc0d643affb8cc08b36143d401a/airflow/api_connexion/security.py#L213'>airflow/api_connexion/security.py:213:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/cf2656179a87cdc0d643affb8cc08b36143d401a/airflow/api_connexion/security.py#L229'>airflow/api_connexion/security.py:229:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/cf2656179a87cdc0d643affb8cc08b36143d401a/airflow/api_connexion/security.py#L250'>airflow/api_connexion/security.py:250:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/cf2656179a87cdc0d643affb8cc08b36143d401a/airflow/api_connexion/security.py#L88'>airflow/api_connexion/security.py:88:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/cf2656179a87cdc0d643affb8cc08b36143d401a/airflow/api_fastapi/core_api/app.py#L86'>airflow/api_fastapi/core_api/app.py:86:29:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/cf2656179a87cdc0d643affb8cc08b36143d401a/airflow/cli/commands/task_command.py#L596'>airflow/cli/commands/task_command.py:596:25:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/airflow/blob/cf2656179a87cdc0d643affb8cc08b36143d401a/airflow/dag_processing/manager.py#L1172'>airflow/dag_processing/manager.py:1172:34:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
... 162 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+80 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/f8adaf66c198c4497b4134f106abdb5538051c43/superset/charts/api.py#L588'>superset/charts/api.py:588:22:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/superset/blob/f8adaf66c198c4497b4134f106abdb5538051c43/superset/charts/api.py#L710'>superset/charts/api.py:710:22:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/superset/blob/f8adaf66c198c4497b4134f106abdb5538051c43/superset/commands/dashboard/filter_state/create.py#L39'>superset/commands/dashboard/filter_state/create.py:39:22:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/superset/blob/f8adaf66c198c4497b4134f106abdb5538051c43/superset/commands/dashboard/filter_state/update.py#L36'>superset/commands/dashboard/filter_state/update.py:36:22:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/superset/blob/f8adaf66c198c4497b4134f106abdb5538051c43/superset/commands/database/tables.py#L136'>superset/commands/database/tables.py:136:28:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/superset/blob/f8adaf66c198c4497b4134f106abdb5538051c43/superset/commands/distributed_lock/get.py#L45'>superset/commands/distributed_lock/get.py:45:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/superset/blob/f8adaf66c198c4497b4134f106abdb5538051c43/superset/commands/explore/get.py#L116'>superset/commands/explore/get.py:116:26:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/superset/blob/f8adaf66c198c4497b4134f106abdb5538051c43/superset/commands/explore/get.py#L130'>superset/commands/explore/get.py:130:52:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/superset/blob/f8adaf66c198c4497b4134f106abdb5538051c43/superset/commands/sql_lab/export.py#L101'>superset/commands/sql_lab/export.py:101:44:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/apache/superset/blob/f8adaf66c198c4497b4134f106abdb5538051c43/superset/commands/sql_lab/results.py#L111'>superset/commands/sql_lab/results.py:111:44:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
... 70 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+60 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L428'>src/bokeh/core/serialization.py:428:26:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L537'>src/bokeh/core/serialization.py:537:50:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L539'>src/bokeh/core/serialization.py:539:53:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L541'>src/bokeh/core/serialization.py:541:53:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L543'>src/bokeh/core/serialization.py:543:52:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L545'>src/bokeh/core/serialization.py:545:50:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L547'>src/bokeh/core/serialization.py:547:50:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L549'>src/bokeh/core/serialization.py:549:52:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L551'>src/bokeh/core/serialization.py:551:52:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/serialization.py#L553'>src/bokeh/core/serialization.py:553:58:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
... 50 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/8925fab0b53694d52ebf59aeb77a162978fbe2db/src/latch/registry/table.py#L501'>src/latch/registry/table.py:501:35:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/latchbio/latch/blob/8925fab0b53694d52ebf59aeb77a162978fbe2db/src/latch/registry/table.py#L678'>src/latch/registry/table.py:678:34:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/latchbio/latch/blob/8925fab0b53694d52ebf59aeb77a162978fbe2db/src/latch/registry/utils.py#L38'>src/latch/registry/utils.py:38:26:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/latchbio/latch/blob/8925fab0b53694d52ebf59aeb77a162978fbe2db/src/latch/registry/utils.py#L54'>src/latch/registry/utils.py:54:28:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/latchbio/latch/blob/8925fab0b53694d52ebf59aeb77a162978fbe2db/src/latch/registry/utils.py#L64'>src/latch/registry/utils.py:64:22:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/latchbio/latch/blob/8925fab0b53694d52ebf59aeb77a162978fbe2db/src/latch_cli/services/register/utils.py#L157'>src/latch_cli/services/register/utils.py:157:32:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/latchbio/latch/blob/8925fab0b53694d52ebf59aeb77a162978fbe2db/src/latch_cli/services/register/utils.py#L159'>src/latch_cli/services/register/utils.py:159:24:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+177 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/e62fcb15a70dfb6f4c408cf801f83b216578335b/pandas/_config/localization.py#L159'>pandas/_config/localization.py:159:89:</a> E501 Line too long (90 > 88)
+ <a href='https://github.com/pandas-dev/pandas/blob/e62fcb15a70dfb6f4c408cf801f83b216578335b/pandas/core/algorithms.py#L27'>pandas/core/algorithms.py:27:5:</a> TC001 Move application import `pandas._typing.AnyArrayLike` into a type-checking block
+ <a href='https://github.com/pandas-dev/pandas/blob/e62fcb15a70dfb6f4c408cf801f83b216578335b/pandas/core/algorithms.py#L28'>pandas/core/algorithms.py:28:5:</a> TC001 Move application import `pandas._typing.ArrayLike` into a type-checking block
+ <a href='https://github.com/pandas-dev/pandas/blob/e62fcb15a70dfb6f4c408cf801f83b216578335b/pandas/core/algorithms.py#L29'>pandas/core/algorithms.py:29:5:</a> TC001 Move application import `pandas._typing.ArrayLikeT` into a type-checking block
+ <a href='https://github.com/pandas-dev/pandas/blob/e62fcb15a70dfb6f4c408cf801f83b216578335b/pandas/core/algorithms.py#L30'>pandas/core/algorithms.py:30:5:</a> TC001 Move application import `pandas._typing.AxisInt` into a type-checking block
+ <a href='https://github.com/pandas-dev/pandas/blob/e62fcb15a70dfb6f4c408cf801f83b216578335b/pandas/core/algorithms.py#L31'>pandas/core/algorithms.py:31:5:</a> TC001 Move application import `pandas._typing.DtypeObj` into a type-checking block
+ <a href='https://github.com/pandas-dev/pandas/blob/e62fcb15a70dfb6f4c408cf801f83b216578335b/pandas/core/algorithms.py#L32'>pandas/core/algorithms.py:32:5:</a> TC001 Move application import `pandas._typing.TakeIndexer` into a type-checking block
... 144 additional changes omitted for rule TC001
+ <a href='https://github.com/pandas-dev/pandas/blob/e62fcb15a70dfb6f4c408cf801f83b216578335b/pandas/core/apply.py#L5'>pandas/core/apply.py:5:29:</a> TC003 Move standard library import `collections.abc.Callable` into a type-checking block
+ <a href='https://github.com/pandas-dev/pandas/blob/e62fcb15a70dfb6f4c408cf801f83b216578335b/pandas/core/common.py#L15'>pandas/core/common.py:15:5:</a> TC003 Move standard library import `collections.abc.Callable` into a type-checking block
+ <a href='https://github.com/pandas-dev/pandas/blob/e62fcb15a70dfb6f4c408cf801f83b216578335b/pandas/core/common.py#L16'>pandas/core/common.py:16:5:</a> TC003 Move standard library import `collections.abc.Collection` into a type-checking block
+ <a href='https://github.com/pandas-dev/pandas/blob/e62fcb15a70dfb6f4c408cf801f83b216578335b/pandas/core/common.py#L17'>pandas/core/common.py:17:5:</a> TC003 Move standard library import `collections.abc.Generator` into a type-checking block
+ <a href='https://github.com/pandas-dev/pandas/blob/e62fcb15a70dfb6f4c408cf801f83b216578335b/pandas/core/common.py#L18'>pandas/core/common.py:18:5:</a> TC003 Move standard library import `collections.abc.Hashable` into a type-checking block
+ <a href='https://github.com/pandas-dev/pandas/blob/e62fcb15a70dfb6f4c408cf801f83b216578335b/pandas/core/common.py#L19'>pandas/core/common.py:19:5:</a> TC003 Move standard library import `collections.abc.Iterable` into a type-checking block
... 21 additional changes omitted for rule TC003
+ <a href='https://github.com/pandas-dev/pandas/blob/e62fcb15a70dfb6f4c408cf801f83b216578335b/pandas/core/indexes/api.py#L8'>pandas/core/indexes/api.py:8:17:</a> TC002 Move third-party import `numpy` into a type-checking block
... 163 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/inspection/lazy_wheel.py#L278'>src/poetry/inspection/lazy_wheel.py:278:41:</a> SIM115 Use a context manager for opening files
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/inspection/lazy_wheel.py#L278'>src/poetry/inspection/lazy_wheel.py:278:43:</a> SIM115 Use a context manager for opening files
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+51 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/abbe76dcea2b4a5fe0fb848741a01e554ee4b5bd/rotkehlchen/accounting/history_base_entries.py#L105'>rotkehlchen/accounting/history_base_entries.py:105:29:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/rotki/rotki/blob/abbe76dcea2b4a5fe0fb848741a01e554ee4b5bd/rotkehlchen/accounting/history_base_entries.py#L109'>rotkehlchen/accounting/history_base_entries.py:109:34:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/rotki/rotki/blob/abbe76dcea2b4a5fe0fb848741a01e554ee4b5bd/rotkehlchen/api/rest.py#L649'>rotkehlchen/api/rest.py:649:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/rotki/rotki/blob/abbe76dcea2b4a5fe0fb848741a01e554ee4b5bd/rotkehlchen/api/v1/schemas.py#L2861'>rotkehlchen/api/v1/schemas.py:2861:18:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/rotki/rotki/blob/abbe76dcea2b4a5fe0fb848741a01e554ee4b5bd/rotkehlchen/api/v1/schemas.py#L2892'>rotkehlchen/api/v1/schemas.py:2892:27:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/rotki/rotki/blob/abbe76dcea2b4a5fe0fb848741a01e554ee4b5bd/rotkehlchen/chain/aggregator.py#L1488'>rotkehlchen/chain/aggregator.py:1488:41:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/rotki/rotki/blob/abbe76dcea2b4a5fe0fb848741a01e554ee4b5bd/rotkehlchen/chain/arbitrum_one/node_inquirer.py#L63'>rotkehlchen/chain/arbitrum_one/node_inquirer.py:63:31:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/rotki/rotki/blob/abbe76dcea2b4a5fe0fb848741a01e554ee4b5bd/rotkehlchen/chain/base/node_inquirer.py#L63'>rotkehlchen/chain/base/node_inquirer.py:63:31:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/rotki/rotki/blob/abbe76dcea2b4a5fe0fb848741a01e554ee4b5bd/rotkehlchen/chain/bitcoin/hdkey.py#L283'>rotkehlchen/chain/bitcoin/hdkey.py:283:36:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/rotki/rotki/blob/abbe76dcea2b4a5fe0fb848741a01e554ee4b5bd/rotkehlchen/chain/bitcoin/hdkey.py#L284'>rotkehlchen/chain/bitcoin/hdkey.py:284:27:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
... 41 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/3943920fa267dc83f9295279bea1c774c0916f13/src/scikit_build_core/hatch/plugin.py#L50'>src/scikit_build_core/hatch/plugin.py:50:29:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/3943920fa267dc83f9295279bea1c774c0916f13/src/scikit_build_core/settings/sources.py#L642'>src/scikit_build_core/settings/sources.py:642:29:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/3943920fa267dc83f9295279bea1c774c0916f13/src/scikit_build_core/settings/sources.py#L643'>src/scikit_build_core/settings/sources.py:643:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/3943920fa267dc83f9295279bea1c774c0916f13/src/scikit_build_core/settings/sources.py#L644'>src/scikit_build_core/settings/sources.py:644:21:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/3943920fa267dc83f9295279bea1c774c0916f13/tests/test_builder.py#L108'>tests/test_builder.py:108:26:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/3943920fa267dc83f9295279bea1c774c0916f13/tests/test_builder.py#L128'>tests/test_builder.py:128:26:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/3943920fa267dc83f9295279bea1c774c0916f13/tests/test_builder.py#L141'>tests/test_builder.py:141:28:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/3943920fa267dc83f9295279bea1c774c0916f13/tests/test_pyproject_pep660.py#L14'>tests/test_pyproject_pep660.py:14:24:</a> TC006 [*] Add quotes to type expression in `typing.cast()`
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (6 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TC006 | 431 | 431 | 0 | 0 | 0 |
| TC001 | 149 | 149 | 0 | 0 | 0 |
| TC003 | 27 | 27 | 0 | 0 | 0 |
| SIM115 | 2 | 1 | 1 | 0 | 0 |
| E501 | 1 | 1 | 0 | 0 | 0 |
| TC002 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @Daverball on 2024-11-21 11:24_

The extra hits for the other rules make sense to me, since fixing `TC006` can result in subsequent `TC001-003` errors (or `E501` for a very long annotation).

In the case of `SIM115` the fix for `TC006` causes the `SIM115` violation to shift by two characters. But the actual violation doesn't change.

---

_Label `rule` added by @MichaReiser on 2024-11-22 12:48_

---

_Label `preview` added by @MichaReiser on 2024-11-22 12:48_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/mod.rs`:79 on 2024-11-22 12:51_

I know this is a pattern that we have in other linter-groups too, but I don't think it's actually necessary to test a preview-rule. It's only necessary if the rule depends on any other preview-only behavior

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_cast_value.rs`:67 on 2024-11-22 12:54_

The clone calls here seem unnecessary
```suggestion
    let edit = quote_type_expression(type_expr, checker.semantic(), checker.stylist()).ok();
    if let Some(edit) = edit {
        if checker
            .comment_ranges()
            .has_comments(type_expr, checker.source())
        {
            diagnostic.set_fix(Fix::unsafe_edit(edit));
        } else {
            diagnostic.set_fix(Fix::safe_edit(edit));
        }
    }
```

---

_@MichaReiser approved on 2024-11-22 12:54_

---

_Comment by @MichaReiser on 2024-11-22 12:55_

Thanks for contributing this rule. The code changes look good to me. 

I want to wait for updated ecosystem checks. I don't see why this PR should result in any new diagnostics other than TC006.

---

_@MichaReiser reviewed on 2024-11-22 12:57_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/flake8_type_checking/TC006.py`:52 on 2024-11-22 12:57_

Could we add a test where parts of the type are quoted?
```suggestion
    t.cast(Literal["3.0", '3'], 3.0)  # TC006
```

---

_Comment by @MichaReiser on 2024-11-22 12:59_

I'm a bit confused by the ecosystem checks. I understand that we run ruff exactly once and don't apply any fixes. That makes it unclear to me why there would be any changed diagnostics other than for TC006.

---

_Comment by @Daverball on 2024-11-22 13:30_

> I'm a bit confused by the ecosystem checks. I understand that we run ruff exactly once and don't apply any fixes. That makes it unclear to me why there would be any changed diagnostics other than for TC006.

The only way this output makes sense to me is if it does in fact repeatedly apply fixes until it runs out of fixes and uses the output from whatever the last iteration was for the ecosystem results. It's also a little sus that there's no +/- on the count of fixes, since these are definitely all auto-fixable. Maybe this was changed at some point without anyone noticing? Because the fixes count was always zero on my other PRs as well, even though they definitely included some new fixes.

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_cast_value.rs`:67 on 2024-11-22 13:37_

You're right I copied this from the other rule which shares a single edit between multiple diagnostics, so the copy was necessary there, but in this case there's only ever one use of `edit`, so we don't need to copy.

---

_@Daverball reviewed on 2024-11-22 13:37_

---

_Comment by @MichaReiser on 2024-11-22 13:44_

Something's definitely broken. Running the two ruff binaries without the ecosystem wrapper script gives me zero changes for pandas.

---

_Comment by @Daverball on 2024-11-22 13:49_

> Something's definitely broken. Running the two ruff binaries without the ecosystem wrapper script gives me zero changes for pandas.

Indeed, I double checked, there's not even any type aliases defined in the first file in pandas. Maybe it's the result of broken caching on the CI side? I've noticed the ecosystem job has gotten significantly faster recently.

---

_Comment by @MichaReiser on 2024-11-22 14:00_

I can reproduce the ecosystem differences locally.... I'm sure it's something obvious and very stupid :D

---

_Comment by @Daverball on 2024-11-22 14:04_

> I can reproduce the ecosystem differences locally.... I'm sure it's something obvious and very stupid :D

Actually I confused myself, this is about `typing.cast` and not type aliases. So it actually still makes sense. If you apply the fix for TC006, you will get the TC001 errors afterwards, because the file uses `from __future__ import annotations`, so the only runtime use of those `_typing` imports happened inside `cast` statements, so if you quote them, there are none left and `TC001-003` triggers.

So I still think the ecosystem script applies fixes repeatedly and uses either the state where it can no longer fix things or the state just before it fixed everything for the results.

That also explains the poetry hit, since fixing TC006 causes the SIM115 violation to be moved two characters over, by the newly introduced quotes.

---

_Comment by @MichaReiser on 2024-11-22 14:22_

> Actually I confused myself, this is about typing.cast and not type aliases. So it actually still makes sense. If you apply the fix for TC006, you will get the TC001 errors afterwards, because the file uses from __future__ import annotations, so the only runtime use of those _typing imports happened inside cast statements, so if you quote them, there are none left and TC001-003 triggers.

Possibly. But the ecosystem check run `ruff check`. You can see the command at the top: 

```
ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview
```

And that ultimately calls into `lint_only` which lints the file once, without applying fixes. 


Ohhhhh.... pandas sets `fix = true`

---

_@MichaReiser approved on 2024-11-22 14:22_

---

_Merged by @MichaReiser on 2024-11-22 14:23_

---

_Closed by @MichaReiser on 2024-11-22 14:23_

---

_Comment by @Daverball on 2024-11-22 14:26_

> Ohhhhh.... pandas sets `fix = true`

I see, ecosystem should probably force that to `false` then. Although I would see some value in checking for fix circularity issues in ecosystem, but that would require a larger restructure of how `ruff` is invoked and the results are interpreted.

---

_Branch deleted on 2024-11-22 14:29_

---

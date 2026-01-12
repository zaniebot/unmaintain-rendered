```yaml
number: 21585
title: "[ruff] single-assignment-missing-final (RUF066)"
type: pull_request
state: closed
author: hamirmahal
labels: []
assignees: []
base: main
head: hamir/feat/flag-variables-that-are-not-reassigned-and-missing-final
created_at: 2025-11-22T23:53:23Z
updated_at: 2025-11-24T17:21:09Z
url: https://github.com/astral-sh/ruff/pull/21585
synced_at: 2026-01-12T15:57:28Z
```

# [ruff] single-assignment-missing-final (RUF066)

---

_@hamirmahal_

Closes #21584
<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This change adds a new rule that warns when variables that are only assigned once are missing a `Final` annotation.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

I added several test files like

```
crates/ruff_linter/resources/test/fixtures/immutability/annotated_final.py
```
<!-- How was it tested? -->


---

_Comment by @hamirmahal on 2025-11-22 23:53_

This initial implementation is conservative and attempts to only flag cases where a variable is definitely missing a `Final` annotation.

---

_Comment by @astral-sh-bot[bot] on 2025-11-23 00:05_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+421004 -151 violations, +0 -0 fixes in 28 projects; 27 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+5231 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/0d09e360b9a8b6249988984b7ea766a67e051742/disnake/__init__.py#L13'>disnake/__init__.py:13:1:</a> RUF066 Variable `__title__` is assigned once and is missing `Final`
+ <a href='https://github.com/DisnakeDev/disnake/blob/0d09e360b9a8b6249988984b7ea766a67e051742/disnake/__init__.py#L14'>disnake/__init__.py:14:1:</a> RUF066 Variable `__author__` is assigned once and is missing `Final`
+ <a href='https://github.com/DisnakeDev/disnake/blob/0d09e360b9a8b6249988984b7ea766a67e051742/disnake/__init__.py#L15'>disnake/__init__.py:15:1:</a> RUF066 Variable `__license__` is assigned once and is missing `Final`
+ <a href='https://github.com/DisnakeDev/disnake/blob/0d09e360b9a8b6249988984b7ea766a67e051742/disnake/__init__.py#L16'>disnake/__init__.py:16:1:</a> RUF066 Variable `__copyright__` is assigned once and is missing `Final`
+ <a href='https://github.com/DisnakeDev/disnake/blob/0d09e360b9a8b6249988984b7ea766a67e051742/disnake/__init__.py#L18'>disnake/__init__.py:18:1:</a> RUF066 Variable `__path__` is assigned once and is missing `Final`
+ <a href='https://github.com/DisnakeDev/disnake/blob/0d09e360b9a8b6249988984b7ea766a67e051742/disnake/__main__.py#L109'>disnake/__main__.py:109:1:</a> RUF066 Variable `_cog_template` is assigned once and is missing `Final`
+ <a href='https://github.com/DisnakeDev/disnake/blob/0d09e360b9a8b6249988984b7ea766a67e051742/disnake/__main__.py#L123'>disnake/__main__.py:123:1:</a> RUF066 Variable `_cog_extras` is assigned once and is missing `Final`
+ <a href='https://github.com/DisnakeDev/disnake/blob/0d09e360b9a8b6249988984b7ea766a67e051742/disnake/__main__.py#L15'>disnake/__main__.py:15:5:</a> RUF066 Variable `entries` is assigned once and is missing `Final`
+ <a href='https://github.com/DisnakeDev/disnake/blob/0d09e360b9a8b6249988984b7ea766a67e051742/disnake/__main__.py#L17'>disnake/__main__.py:17:5:</a> RUF066 Variable `sys_ver` is assigned once and is missing `Final`
+ <a href='https://github.com/DisnakeDev/disnake/blob/0d09e360b9a8b6249988984b7ea766a67e051742/disnake/__main__.py#L21'>disnake/__main__.py:21:5:</a> RUF066 Variable `disnake_ver` is assigned once and is missing `Final`
... 5221 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+14057 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/.github/scripts/download_pretrained.py#L103'>.github/scripts/download_pretrained.py:103:5:</a> RUF066 Variable `parser` is assigned once and is missing `Final`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/.github/scripts/download_pretrained.py#L118'>.github/scripts/download_pretrained.py:118:5:</a> RUF066 Variable `arg_parser` is assigned once and is missing `Final`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/.github/scripts/download_pretrained.py#L119'>.github/scripts/download_pretrained.py:119:5:</a> RUF066 Variable `cmdline_args` is assigned once and is missing `Final`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/.github/scripts/download_pretrained.py#L14'>.github/scripts/download_pretrained.py:14:1:</a> RUF066 Variable `logger` is assigned once and is missing `Final`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/.github/scripts/download_pretrained.py#L16'>.github/scripts/download_pretrained.py:16:1:</a> RUF066 Variable `COMP_NAME` is assigned once and is missing `Final`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/.github/scripts/download_pretrained.py#L17'>.github/scripts/download_pretrained.py:17:1:</a> RUF066 Variable `DEFAULT_MODEL_NAME` is assigned once and is missing `Final`
... 14052 additional changes omitted for rule RUF066
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/test.py#L462'>rasa/core/test.py:462:97:</a> RUF100 [*] Unused blanket `noqa` directive
... 14051 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+89322 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/bf39582e63df28866fe4ca78e70b41df572564cb/airflow-core/docs/conf.py#L110'>airflow-core/docs/conf.py:110:1:</a> RUF066 Variable `exclude_patterns` is assigned once and is missing `Final`
+ <a href='https://github.com/apache/airflow/blob/bf39582e63df28866fe4ca78e70b41df572564cb/airflow-core/docs/conf.py#L119'>airflow-core/docs/conf.py:119:1:</a> RUF066 Variable `ALLOWED_TOP_LEVEL_FILES` is assigned once and is missing `Final`
+ <a href='https://github.com/apache/airflow/blob/bf39582e63df28866fe4ca78e70b41df572564cb/airflow-core/docs/conf.py#L121'>airflow-core/docs/conf.py:121:1:</a> RUF066 Variable `PACKAGES_THAT_WE_SHOULD_ADD_TO_API_DOCS` is assigned once and is missing `Final`
+ <a href='https://github.com/apache/airflow/blob/bf39582e63df28866fe4ca78e70b41df572564cb/airflow-core/docs/conf.py#L136'>airflow-core/docs/conf.py:136:1:</a> RUF066 Variable `UTIL_MODULES_THAT_SHOULD_BE_INCLUDED_IN_API_DOCS` is assigned once and is missing `Final`
+ <a href='https://github.com/apache/airflow/blob/bf39582e63df28866fe4ca78e70b41df572564cb/airflow-core/docs/conf.py#L140'>airflow-core/docs/conf.py:140:1:</a> RUF066 Variable `MODELS_THAT_SHOULD_BE_INCLUDED_IN_API_DOCS` is assigned once and is missing `Final`
+ <a href='https://github.com/apache/airflow/blob/bf39582e63df28866fe4ca78e70b41df572564cb/airflow-core/docs/conf.py#L159'>airflow-core/docs/conf.py:159:5:</a> RUF066 Variable `root` is assigned once and is missing `Final`
+ <a href='https://github.com/apache/airflow/blob/bf39582e63df28866fe4ca78e70b41df572564cb/airflow-core/docs/conf.py#L179'>airflow-core/docs/conf.py:179:9:</a> RUF066 Variable `f` is assigned once and is missing `Final`
+ <a href='https://github.com/apache/airflow/blob/bf39582e63df28866fe4ca78e70b41df572564cb/airflow-core/docs/conf.py#L183'>airflow-core/docs/conf.py:183:1:</a> RUF066 Variable `templates_path` is assigned once and is missing `Final`
+ <a href='https://github.com/apache/airflow/blob/bf39582e63df28866fe4ca78e70b41df572564cb/airflow-core/docs/conf.py#L186'>airflow-core/docs/conf.py:186:1:</a> RUF066 Variable `keep_warnings` is assigned once and is missing `Final`
+ <a href='https://github.com/apache/airflow/blob/bf39582e63df28866fe4ca78e70b41df572564cb/airflow-core/docs/conf.py#L190'>airflow-core/docs/conf.py:190:1:</a> RUF066 Variable `html_theme` is assigned once and is missing `Final`
... 89312 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+29010 -141 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/08c1d034790e38b6d891de8dcc34bcad5f9725cc/RELEASING/changelog.py#L110'>RELEASING/changelog.py:110:9:</a> RUF066 Variable `author_name` is assigned once and is missing `Final`
+ <a href='https://github.com/apache/superset/blob/08c1d034790e38b6d891de8dcc34bcad5f9725cc/RELEASING/changelog.py#L115'>RELEASING/changelog.py:115:13:</a> RUF066 Variable `pr_info` is assigned once and is missing `Final`
+ <a href='https://github.com/apache/superset/blob/08c1d034790e38b6d891de8dcc34bcad5f9725cc/RELEASING/changelog.py#L125'>RELEASING/changelog.py:125:9:</a> RUF066 Variable `commit` is assigned once and is missing `Final`
+ <a href='https://github.com/apache/superset/blob/08c1d034790e38b6d891de8dcc34bcad5f9725cc/RELEASING/changelog.py#L127'>RELEASING/changelog.py:127:66:</a> RUF066 Variable `file` is assigned once and is missing `Final`
+ <a href='https://github.com/apache/superset/blob/08c1d034790e38b6d891de8dcc34bcad5f9725cc/RELEASING/changelog.py#L131'>RELEASING/changelog.py:131:9:</a> RUF066 Variable `pr_number` is assigned once and is missing `Final`
+ <a href='https://github.com/apache/superset/blob/08c1d034790e38b6d891de8dcc34bcad5f9725cc/RELEASING/changelog.py#L136'>RELEASING/changelog.py:136:13:</a> RUF066 Variable `pr_info` is assigned once and is missing `Final`
... 28864 additional changes omitted for rule RUF066
+ <a href='https://github.com/apache/superset/blob/08c1d034790e38b6d891de8dcc34bcad5f9725cc/RELEASING/changelog.py#L380'>RELEASING/changelog.py:380:5:</a> D400 First line should end with a period
- <a href='https://github.com/apache/superset/blob/08c1d034790e38b6d891de8dcc34bcad5f9725cc/RELEASING/changelog.py#L380'>RELEASING/changelog.py:380:5:</a> D400 First line should end with a period
- <a href='https://github.com/apache/superset/blob/08c1d034790e38b6d891de8dcc34bcad5f9725cc/scripts/benchmark_migration.py#L141'>scripts/benchmark_migration.py:141:5:</a> D103 Missing docstring in public function
+ <a href='https://github.com/apache/superset/blob/08c1d034790e38b6d891de8dcc34bcad5f9725cc/scripts/benchmark_migration.py#L141'>scripts/benchmark_migration.py:141:5:</a> D103 Missing docstring in public function
... 29141 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+5326 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L132'>check_proxy.py:132:5:</a> RUF066 Variable `path_new_version` is assigned once and is missing `Final`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L15'>check_proxy.py:15:5:</a> RUF066 Variable `proxies_https` is assigned once and is missing `Final`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L182'>check_proxy.py:182:9:</a> RUF066 Variable `proxies` is assigned once and is missing `Final`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L185'>check_proxy.py:185:9:</a> RUF066 Variable `remote_json_data` is assigned once and is missing `Final`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L186'>check_proxy.py:186:9:</a> RUF066 Variable `remote_version` is assigned once and is missing `Final`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L18'>check_proxy.py:18:9:</a> RUF066 Variable `response` is assigned once and is missing `Final`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L198'>check_proxy.py:198:13:</a> RUF066 Variable `user_instruction` is assigned once and is missing `Final`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L19'>check_proxy.py:19:9:</a> RUF066 Variable `data` is assigned once and is missing `Final`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L200'>check_proxy.py:200:17:</a> RUF066 Variable `path` is assigned once and is missing `Final`
+ <a href='https://github.com/binary-husky/gpt_academic/blob/0aa0472da44edc0a888c7f83b564c6d0d1089366/check_proxy.py#L21'>check_proxy.py:21:13:</a> RUF066 Variable `country` is assigned once and is missing `Final`
... 5316 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+15719 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/docserver.py#L44'>docs/bokeh/docserver.py:44:1:</a> RUF066 Variable `IOLOOP` is assigned once and is missing `Final`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/docserver.py#L45'>docs/bokeh/docserver.py:45:1:</a> RUF066 Variable `HOST` is assigned once and is missing `Final`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/docserver.py#L46'>docs/bokeh/docserver.py:46:1:</a> RUF066 Variable `PORT` is assigned once and is missing `Final`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/docserver.py#L47'>docs/bokeh/docserver.py:47:1:</a> RUF066 Variable `VISIT_URL` is assigned once and is missing `Final`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/docserver.py#L48'>docs/bokeh/docserver.py:48:1:</a> RUF066 Variable `SPHINX_TOP` is assigned once and is missing `Final`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/docserver.py#L50'>docs/bokeh/docserver.py:50:1:</a> RUF066 Variable `app` is assigned once and is missing `Final`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/docserver.py#L74'>docs/bokeh/docserver.py:74:5:</a> RUF066 Variable `IOLOOP` is assigned once and is missing `Final`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/docserver.py#L75'>docs/bokeh/docserver.py:75:5:</a> RUF066 Variable `http_server` is assigned once and is missing `Final`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/docserver.py#L86'>docs/bokeh/docserver.py:86:5:</a> RUF066 Variable `server` is assigned once and is missing `Final`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/docs/bokeh/docserver.py#L91'>docs/bokeh/docserver.py:91:5:</a> RUF066 Variable `browser` is assigned once and is missing `Final`
... 15709 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+15518 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/6459497bdc529fc79ccb45b279411ca1025863e2/.github/workflows/algolia/configure-algolia-api.py#L14'>.github/workflows/algolia/configure-algolia-api.py:14:1:</a> RUF066 Variable `ONE_WAY_SYNONYMS` is assigned once and is missing `Final`
+ <a href='https://github.com/ibis-project/ibis/blob/6459497bdc529fc79ccb45b279411ca1025863e2/.github/workflows/algolia/configure-algolia-api.py#L23'>.github/workflows/algolia/configure-algolia-api.py:23:5:</a> RUF066 Variable `client` is assigned once and is missing `Final`
+ <a href='https://github.com/ibis-project/ibis/blob/6459497bdc529fc79ccb45b279411ca1025863e2/.github/workflows/algolia/configure-algolia-api.py#L24'>.github/workflows/algolia/configure-algolia-api.py:24:5:</a> RUF066 Variable `index` is assigned once and is missing `Final`
+ <a href='https://github.com/ibis-project/ibis/blob/6459497bdc529fc79ccb45b279411ca1025863e2/.github/workflows/algolia/configure-algolia-api.py#L29'>.github/workflows/algolia/configure-algolia-api.py:29:5:</a> RUF066 Variable `override_default_settings` is assigned once and is missing `Final`
+ <a href='https://github.com/ibis-project/ibis/blob/6459497bdc529fc79ccb45b279411ca1025863e2/.github/workflows/algolia/configure-algolia-api.py#L7'>.github/workflows/algolia/configure-algolia-api.py:7:1:</a> RUF066 Variable `api_key` is assigned once and is missing `Final`
+ <a href='https://github.com/ibis-project/ibis/blob/6459497bdc529fc79ccb45b279411ca1025863e2/.github/workflows/algolia/configure-algolia-api.py#L8'>.github/workflows/algolia/configure-algolia-api.py:8:1:</a> RUF066 Variable `app_id` is assigned once and is missing `Final`
+ <a href='https://github.com/ibis-project/ibis/blob/6459497bdc529fc79ccb45b279411ca1025863e2/.github/workflows/algolia/configure-algolia-api.py#L9'>.github/workflows/algolia/configure-algolia-api.py:9:1:</a> RUF066 Variable `index_name` is assigned once and is missing `Final`
+ <a href='https://github.com/ibis-project/ibis/blob/6459497bdc529fc79ccb45b279411ca1025863e2/.github/workflows/algolia/upload-algolia-api.py#L104'>.github/workflows/algolia/upload-algolia-api.py:104:9:</a> RUF066 Variable `start` is assigned once and is missing `Final`
+ <a href='https://github.com/ibis-project/ibis/blob/6459497bdc529fc79ccb45b279411ca1025863e2/.github/workflows/algolia/upload-algolia-api.py#L105'>.github/workflows/algolia/upload-algolia-api.py:105:9:</a> RUF066 Variable `end` is assigned once and is missing `Final`
+ <a href='https://github.com/ibis-project/ibis/blob/6459497bdc529fc79ccb45b279411ca1025863e2/.github/workflows/algolia/upload-algolia-api.py#L106'>.github/workflows/algolia/upload-algolia-api.py:106:9:</a> RUF066 Variable `text_blob` is assigned once and is missing `Final`
... 15508 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+20291 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/f070217c3b7630ad47f228abd39e8886d289af83/libs/cli/langchain_cli/cli.py#L16'>libs/cli/langchain_cli/cli.py:16:1:</a> RUF066 Variable `app` is assigned once and is missing `Final`
+ <a href='https://github.com/langchain-ai/langchain/blob/f070217c3b7630ad47f228abd39e8886d289af83/libs/cli/langchain_cli/cli.py#L76'>libs/cli/langchain_cli/cli.py:76:9:</a> RUF066 Variable `project_dir` is assigned once and is missing `Final`
+ <a href='https://github.com/langchain-ai/langchain/blob/f070217c3b7630ad47f228abd39e8886d289af83/libs/cli/langchain_cli/cli.py#L77'>libs/cli/langchain_cli/cli.py:77:9:</a> RUF066 Variable `pyproject` is assigned once and is missing `Final`
+ <a href='https://github.com/langchain-ai/langchain/blob/f070217c3b7630ad47f228abd39e8886d289af83/libs/cli/langchain_cli/constants.py#L3'>libs/cli/langchain_cli/constants.py:3:1:</a> RUF066 Variable `DEFAULT_GIT_REPO` is assigned once and is missing `Final`
+ <a href='https://github.com/langchain-ai/langchain/blob/f070217c3b7630ad47f228abd39e8886d289af83/libs/cli/langchain_cli/constants.py#L4'>libs/cli/langchain_cli/constants.py:4:1:</a> RUF066 Variable `DEFAULT_GIT_SUBDIRECTORY` is assigned once and is missing `Final`
+ <a href='https://github.com/langchain-ai/langchain/blob/f070217c3b7630ad47f228abd39e8886d289af83/libs/cli/langchain_cli/constants.py#L5'>libs/cli/langchain_cli/constants.py:5:1:</a> RUF066 Variable `DEFAULT_GIT_REF` is assigned once and is missing `Final`
... 20280 additional changes omitted for rule RUF066
+ <a href='https://github.com/langchain-ai/langchain/blob/f070217c3b7630ad47f228abd39e8886d289af83/libs/core/langchain_core/document_loaders/base.py#L91'>libs/core/langchain_core/document_loaders/base.py:91:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/langchain-ai/langchain/blob/f070217c3b7630ad47f228abd39e8886d289af83/libs/core/langchain_core/document_loaders/base.py#L91'>libs/core/langchain_core/document_loaders/base.py:91:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/langchain-ai/langchain/blob/f070217c3b7630ad47f228abd39e8886d289af83/libs/core/langchain_core/runnables/graph_mermaid.py#L414'>libs/core/langchain_core/runnables/graph_mermaid.py:414:5:</a> DOC501 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/langchain-ai/langchain/blob/f070217c3b7630ad47f228abd39e8886d289af83/libs/core/langchain_core/runnables/graph_mermaid.py#L414'>libs/core/langchain_core/runnables/graph_mermaid.py:414:5:</a> DOC501 Raised exception `ValueError` missing from docstring
... 20287 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2110 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/fcea7aeab73189c0f4c4cd09439fa8c4c44a2635/docs/source/conf.py#L23'>docs/source/conf.py:23:1:</a> RUF066 Variable `project` is assigned once and is missing `Final`
+ <a href='https://github.com/latchbio/latch/blob/fcea7aeab73189c0f4c4cd09439fa8c4c44a2635/docs/source/conf.py#L24'>docs/source/conf.py:24:1:</a> RUF066 Variable `copyright` is assigned once and is missing `Final`
+ <a href='https://github.com/latchbio/latch/blob/fcea7aeab73189c0f4c4cd09439fa8c4c44a2635/docs/source/conf.py#L28'>docs/source/conf.py:28:1:</a> RUF066 Variable `nitpicky` is assigned once and is missing `Final`
+ <a href='https://github.com/latchbio/latch/blob/fcea7aeab73189c0f4c4cd09439fa8c4c44a2635/docs/source/conf.py#L33'>docs/source/conf.py:33:1:</a> RUF066 Variable `extensions` is assigned once and is missing `Final`
+ <a href='https://github.com/latchbio/latch/blob/fcea7aeab73189c0f4c4cd09439fa8c4c44a2635/docs/source/conf.py#L45'>docs/source/conf.py:45:1:</a> RUF066 Variable `redirects` is assigned once and is missing `Final`
+ <a href='https://github.com/latchbio/latch/blob/fcea7aeab73189c0f4c4cd09439fa8c4c44a2635/docs/source/conf.py#L49'>docs/source/conf.py:49:1:</a> RUF066 Variable `intersphinx_mapping` is assigned once and is missing `Final`
+ <a href='https://github.com/latchbio/latch/blob/fcea7aeab73189c0f4c4cd09439fa8c4c44a2635/docs/source/conf.py#L51'>docs/source/conf.py:51:1:</a> RUF066 Variable `autodoc_member_order` is assigned once and is missing `Final`
+ <a href='https://github.com/latchbio/latch/blob/fcea7aeab73189c0f4c4cd09439fa8c4c44a2635/docs/source/conf.py#L54'>docs/source/conf.py:54:1:</a> RUF066 Variable `templates_path` is assigned once and is missing `Final`
+ <a href='https://github.com/latchbio/latch/blob/fcea7aeab73189c0f4c4cd09439fa8c4c44a2635/docs/source/conf.py#L59'>docs/source/conf.py:59:1:</a> RUF066 Variable `exclude_patterns` is assigned once and is missing `Final`
+ <a href='https://github.com/latchbio/latch/blob/fcea7aeab73189c0f4c4cd09439fa8c4c44a2635/docs/source/conf.py#L67'>docs/source/conf.py:67:1:</a> RUF066 Variable `html_theme` is assigned once and is missing `Final`
... 2100 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (19 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF066 | 420854 | 420854 | 0 | 0 | 0 |
| ANN201 | 200 | 100 | 100 | 0 | 0 |
| DOC201 | 22 | 11 | 11 | 0 | 0 |
| D400 | 20 | 10 | 10 | 0 | 0 |
| D415 | 12 | 6 | 6 | 0 | 0 |
| D200 | 10 | 5 | 5 | 0 | 0 |
| D103 | 8 | 4 | 4 | 0 | 0 |
| DOC501 | 4 | 2 | 2 | 0 | 0 |
| D401 | 4 | 2 | 2 | 0 | 0 |
| CPY001 | 4 | 2 | 2 | 0 | 0 |
| D417 | 2 | 1 | 1 | 0 | 0 |
| D202 | 2 | 1 | 1 | 0 | 0 |
| PLR0912 | 2 | 1 | 1 | 0 | 0 |
| D100 | 2 | 1 | 1 | 0 | 0 |
| PLR6301 | 2 | 1 | 1 | 0 | 0 |
| ARG001 | 2 | 1 | 1 | 0 | 0 |
| PLR0914 | 2 | 1 | 1 | 0 | 0 |
| PLW1514 | 2 | 1 | 1 | 0 | 0 |
| RUF100 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>





---

_Comment by @MichaReiser on 2025-11-24 12:10_

Thanks for your contribution. I'll close this PR for now until we've come to a decision whether we want to add this rule to Ruff. 

The high number of violations makes me very warry and highly suggests that the rule needs to be further refined.

---

_Closed by @MichaReiser on 2025-11-24 12:10_

---

_Branch deleted on 2025-11-24 17:21_

---

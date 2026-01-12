```yaml
number: 22143
title: "[`ruff`] Add `non-empty-init-module` (`RUF067`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: brent/eim
created_at: 2025-12-22T17:46:55Z
updated_at: 2025-12-30T16:32:12Z
url: https://github.com/astral-sh/ruff/pull/22143
synced_at: 2026-01-12T15:57:42Z
```

# [`ruff`] Add `non-empty-init-module` (`RUF067`)

---

_@ntBre_

Summary
--

This PR adds a new rule, `non-empty-init-module`, which restricts the kind of
code that can be included in an `__init__.py` file. By default, docstrings,
imports, and assignments to `__all__` are allowed. When the new configuration
option `lint.ruff.strictly-empty-init-modules` is enabled, no code at all is
allowed.

This closes #9848, where these two variants correspond to different rules in the
[`flake8-empty-init-modules`](https://github.com/samueljsb/flake8-empty-init-modules/)
linter. The upstream rules are EIM001, which bans all code, and EIM002, which
bans non-import/docstring/`__all__` code. Since we discussed folding these into
one rule on [Discord], I just added the rule to the `RUF` group instead of
adding a new `EIM` plugin.

I'm not really sure we need to flag docstrings even when the strict setting is
enabled, but I just followed upstream for now. Similarly, as I noted in a TODO
comment, we could also allow more statements involving `__all__`, such as
`__all__.append(...)` or `__all__.extend(...)`. The current version only allows
assignments, like upstream, as well as annotated and augmented assignments,
unlike upstream.

I think when we discussed this previously, we considered flagging the module
itself as containing code, but for now I followed the upstream implementation of
flagging each statement in the module that breaks the rule (actually the
upstream linter flags each _line_, including comments). This will obviously be a
bit noisier, emitting many diagnostics for the same module. But this also seems
preferable because it flags every statement that needs to be fixed up front
instead of only emitting one diagnostic for the whole file that persists as you
keep removing more lines. It was also easy to implement in `analyze::statement`
without a separate visitor.

The first commit adds the rule and baseline tests, the second commit adds the
option and a diff test showing the additional diagnostics when the setting is
enabled.

I noticed a small (~2%) performance regression on our largest benchmark, so I also added a cached `Checker::in_init_module` field and method instead of the `Checker::path` method. This was almost the only reason for the `Checker::path` field at all, but there's one remaining reference in a `warn_user!` [call](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/checkers/ast/analyze/definitions.rs#L188).

Test Plan
--

New tests adapted from the upstream linter

## Ecosystem Report

I've spot-checked the ecosystem report, and the results look "correct." This is obviously a very noisy rule if you do include code in `__init__.py` files. We could make it less noisy by adding more exceptions (e.g. allowing `if TYPE_CHECKING` blocks, allowing `__getattr__` functions, allowing imports from `importlib` assignments), but I'm sort of inclined just to start simple and see what users need.

[Discord]: https://discord.com/channels/1039017663004942429/1082324250112823306/1440086001035771985


---

_Label `rule` added by @ntBre on 2025-12-22 17:47_

---

_Label `preview` added by @ntBre on 2025-12-22 17:47_

---

_Comment by @astral-sh-bot[bot] on 2025-12-22 17:55_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2016 -0 violations, +0 -0 fixes in 24 projects; 31 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+28 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/__init__.py#L13'>disnake/__init__.py:13:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/__init__.py#L15'>disnake/__init__.py:15:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/__init__.py#L16'>disnake/__init__.py:16:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/__init__.py#L83'>disnake/__init__.py:83:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/ext/mypy_plugin/__init__.py#L11'>disnake/ext/mypy_plugin/__init__.py:11:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/ext/mypy_plugin/__init__.py#L7'>disnake/ext/mypy_plugin/__init__.py:7:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/ext/tasks/__init__.py#L38'>disnake/ext/tasks/__init__.py:38:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/ext/tasks/__init__.py#L39'>disnake/ext/tasks/__init__.py:39:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/ext/tasks/__init__.py#L40'>disnake/ext/tasks/__init__.py:40:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/DisnakeDev/disnake/blob/394227ce57cc5245784c2125f5689cdc68a675c1/disnake/ext/tasks/__init__.py#L41'>disnake/ext/tasks/__init__.py:41:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
... 18 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/__init__.py#L10'>rasa/__init__.py:10:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/cli/__init__.py#L5'>rasa/cli/__init__.py:5:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/__init__.py#L5'>rasa/core/__init__.py:5:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/channels/__init__.py#L28'>rasa/core/channels/__init__.py:28:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/channels/__init__.py#L47'>rasa/core/channels/__init__.py:47:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/training/__init__.py#L10'>rasa/core/training/__init__.py:10:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/core/training/__init__.py#L33'>rasa/core/training/__init__.py:33:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/__init__.py#L5'>rasa/nlu/__init__.py:5:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/classifiers/__init__.py#L3'>rasa/nlu/classifiers/__init__.py:3:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/RasaHQ/rasa/blob/c4069568b4fe2adb5d5a1e55d17ce8cb9dda27fc/rasa/nlu/utils/__init__.py#L11'>rasa/nlu/utils/__init__.py:11:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+389 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/cf80ae19840f1d03e16323e7bca819550633db97/airflow-core/src/airflow/__init__.py#L130'>airflow-core/src/airflow/__init__.py:130:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/apache/airflow/blob/cf80ae19840f1d03e16323e7bca819550633db97/airflow-core/src/airflow/__init__.py#L137'>airflow-core/src/airflow/__init__.py:137:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/apache/airflow/blob/cf80ae19840f1d03e16323e7bca819550633db97/airflow-core/src/airflow/__init__.py#L38'>airflow-core/src/airflow/__init__.py:38:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/apache/airflow/blob/cf80ae19840f1d03e16323e7bca819550633db97/airflow-core/src/airflow/__init__.py#L46'>airflow-core/src/airflow/__init__.py:46:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/apache/airflow/blob/cf80ae19840f1d03e16323e7bca819550633db97/airflow-core/src/airflow/__init__.py#L80'>airflow-core/src/airflow/__init__.py:80:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/apache/airflow/blob/cf80ae19840f1d03e16323e7bca819550633db97/airflow-core/src/airflow/__init__.py#L84'>airflow-core/src/airflow/__init__.py:84:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/apache/airflow/blob/cf80ae19840f1d03e16323e7bca819550633db97/airflow-core/src/airflow/api/client/__init__.py#L25'>airflow-core/src/airflow/api/client/__init__.py:25:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/apache/airflow/blob/cf80ae19840f1d03e16323e7bca819550633db97/airflow-core/src/airflow/api_fastapi/core_api/routes/public/__init__.py#L53'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/__init__.py:53:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/apache/airflow/blob/cf80ae19840f1d03e16323e7bca819550633db97/airflow-core/src/airflow/api_fastapi/core_api/routes/public/__init__.py#L56'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/__init__.py:56:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/apache/airflow/blob/cf80ae19840f1d03e16323e7bca819550633db97/airflow-core/src/airflow/api_fastapi/core_api/routes/public/__init__.py#L61'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/__init__.py:61:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
... 379 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+70 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/02411ffde09e51f30e1db44493d6415ac7a63460/superset-core/src/superset_core/mcp/__init__.py#L100'>superset-core/src/superset_core/mcp/__init__.py:100:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/apache/superset/blob/02411ffde09e51f30e1db44493d6415ac7a63460/superset-core/src/superset_core/mcp/__init__.py#L41'>superset-core/src/superset_core/mcp/__init__.py:41:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/apache/superset/blob/02411ffde09e51f30e1db44493d6415ac7a63460/superset-core/src/superset_core/mcp/__init__.py#L44'>superset-core/src/superset_core/mcp/__init__.py:44:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/apache/superset/blob/02411ffde09e51f30e1db44493d6415ac7a63460/superset/__init__.py#L37'>superset/__init__.py:37:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/apache/superset/blob/02411ffde09e51f30e1db44493d6415ac7a63460/superset/__init__.py#L38'>superset/__init__.py:38:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/apache/superset/blob/02411ffde09e51f30e1db44493d6415ac7a63460/superset/__init__.py#L39'>superset/__init__.py:39:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/apache/superset/blob/02411ffde09e51f30e1db44493d6415ac7a63460/superset/__init__.py#L40'>superset/__init__.py:40:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/apache/superset/blob/02411ffde09e51f30e1db44493d6415ac7a63460/superset/__init__.py#L41'>superset/__init__.py:41:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/apache/superset/blob/02411ffde09e51f30e1db44493d6415ac7a63460/superset/__init__.py#L42'>superset/__init__.py:42:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/apache/superset/blob/02411ffde09e51f30e1db44493d6415ac7a63460/superset/__init__.py#L45'>superset/__init__.py:45:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
... 60 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+56 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L101'>src/bokeh/__init__.py:101:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L102'>src/bokeh/__init__.py:102:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L107'>src/bokeh/__init__.py:107:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L109'>src/bokeh/__init__.py:109:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L110'>src/bokeh/__init__.py:110:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L111'>src/bokeh/__init__.py:111:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L37'>src/bokeh/__init__.py:37:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L62'>src/bokeh/__init__.py:62:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L85'>src/bokeh/__init__.py:85:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L92'>src/bokeh/__init__.py:92:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
... 46 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+142 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/a528450ab803a3a70fdcde222f4b7ad26ebc334b/ibis/__init__.py#L31'>ibis/__init__.py:31:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/ibis-project/ibis/blob/a528450ab803a3a70fdcde222f4b7ad26ebc334b/ibis/__init__.py#L42'>ibis/__init__.py:42:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/ibis-project/ibis/blob/a528450ab803a3a70fdcde222f4b7ad26ebc334b/ibis/backends/__init__.py#L1517'>ibis/backends/__init__.py:1517:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/ibis-project/ibis/blob/a528450ab803a3a70fdcde222f4b7ad26ebc334b/ibis/backends/__init__.py#L1530'>ibis/backends/__init__.py:1530:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/ibis-project/ibis/blob/a528450ab803a3a70fdcde222f4b7ad26ebc334b/ibis/backends/__init__.py#L1550'>ibis/backends/__init__.py:1550:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/ibis-project/ibis/blob/a528450ab803a3a70fdcde222f4b7ad26ebc334b/ibis/backends/__init__.py#L1557'>ibis/backends/__init__.py:1557:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/ibis-project/ibis/blob/a528450ab803a3a70fdcde222f4b7ad26ebc334b/ibis/backends/__init__.py#L1567'>ibis/backends/__init__.py:1567:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/ibis-project/ibis/blob/a528450ab803a3a70fdcde222f4b7ad26ebc334b/ibis/backends/__init__.py#L1601'>ibis/backends/__init__.py:1601:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/ibis-project/ibis/blob/a528450ab803a3a70fdcde222f4b7ad26ebc334b/ibis/backends/__init__.py#L1648'>ibis/backends/__init__.py:1648:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/ibis-project/ibis/blob/a528450ab803a3a70fdcde222f4b7ad26ebc334b/ibis/backends/__init__.py#L1673'>ibis/backends/__init__.py:1673:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
... 132 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+274 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/721bf15430ee231bfe50a8b7c6015d91a8d9a24a/libs/cli/tests/unit_tests/migrate/cli_runner/cases/__init__.py#L14'>libs/cli/tests/unit_tests/migrate/cli_runner/cases/__init__.py:14:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/langchain-ai/langchain/blob/721bf15430ee231bfe50a8b7c6015d91a8d9a24a/libs/cli/tests/unit_tests/migrate/cli_runner/cases/__init__.py#L15'>libs/cli/tests/unit_tests/migrate/cli_runner/cases/__init__.py:15:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/langchain-ai/langchain/blob/721bf15430ee231bfe50a8b7c6015d91a8d9a24a/libs/cli/tests/unit_tests/migrate/cli_runner/cases/__init__.py#L6'>libs/cli/tests/unit_tests/migrate/cli_runner/cases/__init__.py:6:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/langchain-ai/langchain/blob/721bf15430ee231bfe50a8b7c6015d91a8d9a24a/libs/core/langchain_core/__init__.py#L19'>libs/core/langchain_core/__init__.py:19:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/langchain-ai/langchain/blob/721bf15430ee231bfe50a8b7c6015d91a8d9a24a/libs/core/langchain_core/__init__.py#L20'>libs/core/langchain_core/__init__.py:20:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/langchain-ai/langchain/blob/721bf15430ee231bfe50a8b7c6015d91a8d9a24a/libs/core/langchain_core/_api/__init__.py#L45'>libs/core/langchain_core/_api/__init__.py:45:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/langchain-ai/langchain/blob/721bf15430ee231bfe50a8b7c6015d91a8d9a24a/libs/core/langchain_core/callbacks/__init__.py#L86'>libs/core/langchain_core/callbacks/__init__.py:86:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/langchain-ai/langchain/blob/721bf15430ee231bfe50a8b7c6015d91a8d9a24a/libs/core/langchain_core/document_loaders/__init__.py#L21'>libs/core/langchain_core/document_loaders/__init__.py:21:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/langchain-ai/langchain/blob/721bf15430ee231bfe50a8b7c6015d91a8d9a24a/libs/core/langchain_core/documents/__init__.py#L39'>libs/core/langchain_core/documents/__init__.py:39:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/langchain-ai/langchain/blob/721bf15430ee231bfe50a8b7c6015d91a8d9a24a/libs/core/langchain_core/embeddings/__init__.py#L16'>libs/core/langchain_core/embeddings/__init__.py:16:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
... 264 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+52 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/docker_utils/__init__.py#L19'>src/latch_cli/docker_utils/__init__.py:19:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/docker_utils/__init__.py#L27'>src/latch_cli/docker_utils/__init__.py:27:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/docker_utils/__init__.py#L38'>src/latch_cli/docker_utils/__init__.py:38:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/docker_utils/__init__.py#L401'>src/latch_cli/docker_utils/__init__.py:401:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/docker_utils/__init__.py#L427'>src/latch_cli/docker_utils/__init__.py:427:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/services/init/assemble_and_sort/__init__.py#L13'>src/latch_cli/services/init/assemble_and_sort/__init__.py:13:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/services/init/assemble_and_sort/__init__.py#L14'>src/latch_cli/services/init/assemble_and_sort/__init__.py:14:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/services/init/assemble_and_sort/__init__.py#L58'>src/latch_cli/services/init/assemble_and_sort/__init__.py:58:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/services/init/assemble_and_sort/__init__.py#L81'>src/latch_cli/services/init/assemble_and_sort/__init__.py:81:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
+ <a href='https://github.com/latchbio/latch/blob/dc91e5b22db76d1dcab1acf91c3afbcc597c0c3a/src/latch_cli/services/init/assemble_and_sort/__init__.py#L86'>src/latch_cli/services/init/assemble_and_sort/__init__.py:86:1:</a> RUF067 `__init__` module should only contain docstrings and re-exports
... 42 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF067 | 2016 | 2016 | 0 | 0 | 0 |

</p>
</details>





---

_@ntBre reviewed on 2025-12-22 18:56_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/non_empty_init_module.rs`:64 on 2025-12-22 18:56_

Definitely open to suggestions here, this feels a bit too short. Adding `in an __init__.py file` seems redundant with the filename in the output, though. The upstream linter at least customizes the message based on the strictness (e.g. `Non-import code detected` for the default).

---

_Marked ready for review by @ntBre on 2025-12-22 18:59_

---

_Review requested from @MichaReiser by @ntBre on 2025-12-22 18:59_

---

_Comment by @ntBre on 2025-12-23 17:54_

Based on the ecosystem results, and my conversation with Micha today, I added a few more allowed statement types in non-strict mode:
- Module-level `__getattr__` functions
- `TYPE_CHECKING` blocks
- Assignments to `__path__` (to support legacy namespace packages)

I also fixed my top-level statement check, which was previously only checking the scope and thus emitting diagnostics for statements within `if` bodies, as just one example.

That will clear out 10/17 of the airflow diagnostics rendered in the comment, so I expect it to drop the total number of ecosystem hits substantially too.

---

_Comment by @ntBre on 2025-12-23 20:34_

A few remaining common patterns in the ecosystem:
- Initializing loggers
- Setting `__version__`
- `__dir__` functions

I think I'll add support for the last two at least since `__dir__` was added in the same PEP as `__getattr__` and often appears with it, and it seems safe enough to allow `__version__` too.

I'm less convinced about the loggers since they can be hard to detect and also seem a bit less common.

---

_Review requested from @amyreese by @MichaReiser on 2025-12-26 08:54_

---

_Comment by @MichaReiser on 2025-12-26 12:12_

Haven't started reviewing but we should probably allow all listed here https://peps.python.org/pep-0008/#module-level-dunder-names

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:492 on 2025-12-26 12:21_

What's the reason that this doesn't check for `__init__.pyi`?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:1063 on 2025-12-26 12:21_

Nit: I prefer avoiding "holes" and instead assign rule ids on the rule that comes first

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/non_empty_init_module.rs`:38 on 2025-12-26 12:23_

```suggestion
/// When [`lint.ruff.strictly-empty-init-modules`] is enabled, all code, including imports, docstrings, and `__all__`, will be
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/non_empty_init_module.rs`:77 on 2025-12-26 12:26_

Maybe use `semantic_model.at_top_level()`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/non_empty_init_module.rs`:113 on 2025-12-26 12:27_

Can you say more about the reason for allowing attribute docstrings, given that attributes are denied by default?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/non_empty_init_module.rs`:113 on 2025-12-26 12:30_

Might be worth being slighlty more strict and asserting that this is an `extend_path` call (ignore the left side, just the `.extend_path(__path__, __name__)`)

See https://github.com/astral-sh/ruff/blob/b4c2825afdd8c1010c3a5859521629d2d1e0a6df/crates/ty_module_resolver/src/resolve.rs#L1627

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/non_empty_init_module.rs`:64 on 2025-12-26 12:36_

I agree that the message is a bit surprising. It's like, yeah, this is code, what's wrong with it?

Using separate messages based on strictness probably makes sense. For strict mode, maybe `init module must be empty` (although it doesn't really say what's wrong)?

---

_@MichaReiser approved on 2025-12-26 12:36_

Thank you. This is great. The bigest open question for me is whether this rule should also apply to `__init__.pyi` files

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:492 on 2025-12-26 16:31_

This was the pattern in the existing checks I factored this out of, but to be honest I hadn't thought too much about the `pyi` version. For this rule in particular, if the motivation is avoiding surprising runtime behavior, I think it makes sense to ignore `pyi` files, though. Stubs aren't executed, and I think stub authors often have less control over the actual code they're defining the interface for, if I understand correctly.

---

_@ntBre reviewed on 2025-12-26 16:31_

---

_@ntBre reviewed on 2025-12-26 16:32_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/non_empty_init_module.rs`:38 on 2025-12-26 16:32_

Ah thank you! I felt that I needed to expand the docs with all the exceptions, and this is a really nice way to do it.

---

_@ntBre reviewed on 2025-12-26 16:35_

---

_Review comment by @ntBre on `crates/ruff_linter/src/codes.rs`:1063 on 2025-12-26 16:35_

I just felt bad because another rule currently using RUF067 is sitting in my review queue (along with RUF069), but I'll update this.

---

_@ntBre reviewed on 2025-12-26 16:41_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/non_empty_init_module.rs`:113 on 2025-12-26 16:41_

Hmm yeah that makes sense. I was just following upstream again here without too much thought. I guess it would be nice if you did want to allow one attribute that you only have to write:

```py
CONST = 1  # noqa: RUF070
"My docstring"
```

instead of a noqa on both lines. But the flip side is that removing the constant adds a new diagnostic on the string. So I'm happy to remove this exception.

---

_@ntBre reviewed on 2025-12-26 16:51_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/non_empty_init_module.rs`:113 on 2025-12-26 16:51_

Makes sense, thanks for the link!



---

_Comment by @ntBre on 2025-12-26 17:42_

> Haven't started reviewing but we should probably allow all listed here [peps.python.org/pep-0008#module-level-dunder-names](https://peps.python.org/pep-0008/#module-level-dunder-names)

I'm not sure that this is an exhaustive list, and from when I searched around last week, even `__version__` doesn't strictly seem to be a standard ([one reference](https://discuss.python.org/t/please-make-package-version-go-away/58501)). But I've added `__author__`, which I think was the only one missing from that section.

---

_@MichaReiser reviewed on 2025-12-29 09:38_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:492 on 2025-12-29 09:38_

Makes sense. That's the conclusion I came to as well on my way to the groccery store :)

---

_@MichaReiser reviewed on 2025-12-29 09:39_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/non_empty_init_module.rs`:113 on 2025-12-29 09:39_

This makes sense to me. Might be worth an inline comment explaining the reason why we allow this.

---

_Renamed from "[`ruff`] Add `non-empty-init-module` (`RUF070`)" to "[`ruff`] Add `non-empty-init-module` (`RUF067`)" by @ntBre on 2025-12-29 15:18_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/non_empty_init_module.rs`:113 on 2025-12-29 15:20_

I ended up removing the exception for attribute docstrings, but I can put it back if you prefer. I can see an argument in either direction.

---

_@ntBre reviewed on 2025-12-29 15:20_

---

_@MichaReiser reviewed on 2025-12-30 08:26_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:1063 on 2025-12-30 08:26_

What I do is that I change the rule code myself if I end up approving a contributor PR, so that we don't need to feel bad about it

---

_@MichaReiser reviewed on 2025-12-30 08:27_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/non_empty_init_module.rs`:113 on 2025-12-30 08:27_

Not having to use two suppressions seems desirable (for as long as it doesn't result in any false negatives)

---

_Comment by @dscorbett on 2025-12-30 14:55_

Part of the rationale is currently that “Including code in `__init__.py` files can cause surprising slowdowns because the code is run at import time.” If it is slow to define a class in \_\_init\_\_.py, it is just as slow (or marginally slower) to define a class elsewhere and import it in \_\_init\_\_.py. Moving code to a different module that \_\_init\_\_.py imports doesn’t stop that code from being run at import time. So that part of the rationale does not make sense.

---

_Comment by @ntBre on 2025-12-30 15:33_

Good catch, thanks! That rationale works for the strict form of the rule, but I agree that it doesn't make much sense next to the first example I have here. I'll try to come up with something better.

---

_Merged by @ntBre on 2025-12-30 16:32_

---

_Closed by @ntBre on 2025-12-30 16:32_

---

_Branch deleted on 2025-12-30 16:32_

---

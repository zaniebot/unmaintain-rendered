```yaml
number: 18949
title: "[`pylint`] Update `missing-maxsplit-arg` docs and error to suggest proper usage (`PLC0207`)"
type: pull_request
state: merged
author: junhsonjb
labels:
  - documentation
assignees: []
merged: true
base: main
head: jjb-missing-maxsplit-message
created_at: 2025-06-26T00:41:59Z
updated_at: 2025-07-16T18:21:11Z
url: https://github.com/astral-sh/ruff/pull/18949
synced_at: 2026-01-12T15:56:28Z
```

# [`pylint`] Update `missing-maxsplit-arg` docs and error to suggest proper usage (`PLC0207`)

---

_@junhsonjb_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fix #18383 by updating the documentation and error message to explain that users should use `rsplit` in order to access the last element of the result with `maxsplit=1`

## Test Plan

<!-- How was it tested? -->

Only documentation and an error message was changed. As such, snapshots were updated to reflect the new error message. With this change, all existing tests pass.

---

_Comment by @github-actions[bot] on 2025-06-26 00:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+46 -46 violations, +0 -0 fixes in 13 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/PlasmaPy/PlasmaPy/blob/91b65432060de002d592bdd3b65c65a2cb2f8be9/noxfile.py#L453'>noxfile.py:453:16:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/91b65432060de002d592bdd3b65c65a2cb2f8be9/noxfile.py#L453'>noxfile.py:453:16:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+9 -9 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/cf43b561c0dcc34b3003463c6e38fcaace288725/dev/backport/update_backport_status.py#L78'>dev/backport/update_backport_status.py:78:21:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/apache/airflow/blob/cf43b561c0dcc34b3003463c6e38fcaace288725/dev/backport/update_backport_status.py#L78'>dev/backport/update_backport_status.py:78:21:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
- <a href='https://github.com/apache/airflow/blob/cf43b561c0dcc34b3003463c6e38fcaace288725/dev/breeze/src/airflow_breeze/commands/common_options.py#L482'>dev/breeze/src/airflow_breeze/commands/common_options.py:482:26:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/apache/airflow/blob/cf43b561c0dcc34b3003463c6e38fcaace288725/dev/breeze/src/airflow_breeze/commands/common_options.py#L482'>dev/breeze/src/airflow_breeze/commands/common_options.py:482:26:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
- <a href='https://github.com/apache/airflow/blob/cf43b561c0dcc34b3003463c6e38fcaace288725/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2631'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2631:50:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/apache/airflow/blob/cf43b561c0dcc34b3003463c6e38fcaace288725/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2631'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2631:50:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
- <a href='https://github.com/apache/airflow/blob/cf43b561c0dcc34b3003463c6e38fcaace288725/dev/breeze/src/airflow_breeze/prepare_providers/provider_documentation.py#L701'>dev/breeze/src/airflow_breeze/prepare_providers/provider_documentation.py:701:34:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/apache/airflow/blob/cf43b561c0dcc34b3003463c6e38fcaace288725/dev/breeze/src/airflow_breeze/prepare_providers/provider_documentation.py#L701'>dev/breeze/src/airflow_breeze/prepare_providers/provider_documentation.py:701:34:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
- <a href='https://github.com/apache/airflow/blob/cf43b561c0dcc34b3003463c6e38fcaace288725/dev/breeze/src/airflow_breeze/utils/run_utils.py#L120'>dev/breeze/src/airflow_breeze/utils/run_utils.py:120:25:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/apache/airflow/blob/cf43b561c0dcc34b3003463c6e38fcaace288725/dev/breeze/src/airflow_breeze/utils/run_utils.py#L120'>dev/breeze/src/airflow_breeze/utils/run_utils.py:120:25:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
... 8 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/308007f909b5a3c1cdd6fc5632ef27ca7eb97ce0/tests/common/query_context_generator.py#L214'>tests/common/query_context_generator.py:214:29:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/apache/superset/blob/308007f909b5a3c1cdd6fc5632ef27ca7eb97ce0/tests/common/query_context_generator.py#L214'>tests/common/query_context_generator.py:214:29:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
- <a href='https://github.com/apache/superset/blob/308007f909b5a3c1cdd6fc5632ef27ca7eb97ce0/tests/common/query_context_generator.py#L255'>tests/common/query_context_generator.py:255:22:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/apache/superset/blob/308007f909b5a3c1cdd6fc5632ef27ca7eb97ce0/tests/common/query_context_generator.py#L255'>tests/common/query_context_generator.py:255:22:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/notebook.py#L663'>src/bokeh/io/notebook.py:663:26:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/notebook.py#L663'>src/bokeh/io/notebook.py:663:26:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/token.py#L142'>src/bokeh/util/token.py:142:41:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/token.py#L142'>src/bokeh/util/token.py:142:41:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/token.py#L155'>src/bokeh/util/token.py:155:41:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/token.py#L155'>src/bokeh/util/token.py:155:41:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/freedomofpress/securedrop/blob/525c04d9bbe8738a6b37f37e653c5e7a41b9d894/securedrop/store.py#L345'>securedrop/store.py:345:49:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/freedomofpress/securedrop/blob/525c04d9bbe8738a6b37f37e653c5e7a41b9d894/securedrop/store.py#L345'>securedrop/store.py:345:49:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/d72d8d13bfd87466288d2a79921fdd1059fd7697/ibis/examples/gen_registry.py#L125'>ibis/examples/gen_registry.py:125:52:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/ibis-project/ibis/blob/d72d8d13bfd87466288d2a79921fdd1059fd7697/ibis/examples/gen_registry.py#L125'>ibis/examples/gen_registry.py:125:52:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+9 -9 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/langchain-ai/langchain/blob/eb122945832eae9b9df7c70ccd8d51fcd7a1899b/libs/core/langchain_core/_api/deprecation.py#L360'>libs/core/langchain_core/_api/deprecation.py:360:17:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/langchain-ai/langchain/blob/eb122945832eae9b9df7c70ccd8d51fcd7a1899b/libs/core/langchain_core/_api/deprecation.py#L360'>libs/core/langchain_core/_api/deprecation.py:360:17:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
- <a href='https://github.com/langchain-ai/langchain/blob/eb122945832eae9b9df7c70ccd8d51fcd7a1899b/libs/core/langchain_core/_api/deprecation.py#L360'>libs/core/langchain_core/_api/deprecation.py:360:56:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/langchain-ai/langchain/blob/eb122945832eae9b9df7c70ccd8d51fcd7a1899b/libs/core/langchain_core/_api/deprecation.py#L360'>libs/core/langchain_core/_api/deprecation.py:360:56:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
- <a href='https://github.com/langchain-ai/langchain/blob/eb122945832eae9b9df7c70ccd8d51fcd7a1899b/libs/core/langchain_core/_api/deprecation.py#L368'>libs/core/langchain_core/_api/deprecation.py:368:17:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/langchain-ai/langchain/blob/eb122945832eae9b9df7c70ccd8d51fcd7a1899b/libs/core/langchain_core/_api/deprecation.py#L368'>libs/core/langchain_core/_api/deprecation.py:368:17:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
- <a href='https://github.com/langchain-ai/langchain/blob/eb122945832eae9b9df7c70ccd8d51fcd7a1899b/libs/core/langchain_core/_api/deprecation.py#L369'>libs/core/langchain_core/_api/deprecation.py:369:16:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/langchain-ai/langchain/blob/eb122945832eae9b9df7c70ccd8d51fcd7a1899b/libs/core/langchain_core/_api/deprecation.py#L369'>libs/core/langchain_core/_api/deprecation.py:369:16:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
- <a href='https://github.com/langchain-ai/langchain/blob/eb122945832eae9b9df7c70ccd8d51fcd7a1899b/libs/core/langchain_core/_api/deprecation.py#L473'>libs/core/langchain_core/_api/deprecation.py:473:24:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/langchain-ai/langchain/blob/eb122945832eae9b9df7c70ccd8d51fcd7a1899b/libs/core/langchain_core/_api/deprecation.py#L473'>libs/core/langchain_core/_api/deprecation.py:473:24:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
... 8 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/7817cb25047152d02908ab61801f5a97b5e379ff/pandas/compat/_optional.py#L166'>pandas/compat/_optional.py:166:14:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/pandas-dev/pandas/blob/7817cb25047152d02908ab61801f5a97b5e379ff/pandas/compat/_optional.py#L166'>pandas/compat/_optional.py:166:14:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pypa/cibuildwheel/blob/40de3fe51083fa91bc8804c5a8de5c496f61ed52/bin/update_pythons.py#L150'>bin/update_pythons.py:150:22:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/pypa/cibuildwheel/blob/40de3fe51083fa91bc8804c5a8de5c496f61ed52/bin/update_pythons.py#L150'>bin/update_pythons.py:150:22:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
- <a href='https://github.com/pypa/cibuildwheel/blob/40de3fe51083fa91bc8804c5a8de5c496f61ed52/cibuildwheel/platforms/macos.py#L153'>cibuildwheel/platforms/macos.py:153:31:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/pypa/cibuildwheel/blob/40de3fe51083fa91bc8804c5a8de5c496f61ed52/cibuildwheel/platforms/macos.py#L153'>cibuildwheel/platforms/macos.py:153:31:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
- <a href='https://github.com/pypa/cibuildwheel/blob/40de3fe51083fa91bc8804c5a8de5c496f61ed52/cibuildwheel/selector.py#L79'>cibuildwheel/selector.py:79:26:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/pypa/cibuildwheel/blob/40de3fe51083fa91bc8804c5a8de5c496f61ed52/cibuildwheel/selector.py#L79'>cibuildwheel/selector.py:79:26:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+11 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/1e4b0bdfea2c05b4c540f96bf311661e199ff34e/zerver/lib/send_email.py#L287'>zerver/lib/send_email.py:287:16:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/zulip/zulip/blob/1e4b0bdfea2c05b4c540f96bf311661e199ff34e/zerver/lib/send_email.py#L287'>zerver/lib/send_email.py:287:16:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
- <a href='https://github.com/zulip/zulip/blob/1e4b0bdfea2c05b4c540f96bf311661e199ff34e/zerver/lib/send_email.py#L429'>zerver/lib/send_email.py:429:21:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/zulip/zulip/blob/1e4b0bdfea2c05b4c540f96bf311661e199ff34e/zerver/lib/send_email.py#L429'>zerver/lib/send_email.py:429:21:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
- <a href='https://github.com/zulip/zulip/blob/1e4b0bdfea2c05b4c540f96bf311661e199ff34e/zerver/lib/upload/local.py#L67'>zerver/lib/upload/local.py:67:17:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/zulip/zulip/blob/1e4b0bdfea2c05b4c540f96bf311661e199ff34e/zerver/lib/upload/local.py#L67'>zerver/lib/upload/local.py:67:17:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
- <a href='https://github.com/zulip/zulip/blob/1e4b0bdfea2c05b4c540f96bf311661e199ff34e/zerver/lib/upload/s3.py#L176'>zerver/lib/upload/s3.py:176:25:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/zulip/zulip/blob/1e4b0bdfea2c05b4c540f96bf311661e199ff34e/zerver/lib/upload/s3.py#L176'>zerver/lib/upload/s3.py:176:25:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
- <a href='https://github.com/zulip/zulip/blob/1e4b0bdfea2c05b4c540f96bf311661e199ff34e/zerver/lib/webhooks/common.py#L242'>zerver/lib/webhooks/common.py:242:26:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/zulip/zulip/blob/1e4b0bdfea2c05b4c540f96bf311661e199ff34e/zerver/lib/webhooks/common.py#L242'>zerver/lib/webhooks/common.py:242:26:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
- <a href='https://github.com/zulip/zulip/blob/1e4b0bdfea2c05b4c540f96bf311661e199ff34e/zerver/lib/webhooks/common.py#L269'>zerver/lib/webhooks/common.py:269:16:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
... 11 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/wntrblm/nox">wntrblm/nox</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/wntrblm/nox/blob/ddd05e800c202d1d0dc037d350680e31fd79ceb4/tests/test_sessions.py#L1199'>tests/test_sessions.py:1199:44:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/wntrblm/nox/blob/ddd05e800c202d1d0dc037d350680e31fd79ceb4/tests/test_sessions.py#L1199'>tests/test_sessions.py:1199:44:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pytest-dev/pytest/blob/c5a75f2498c86850c4ce13bcf10d56efc92394a4/src/_pytest/config/findpaths.py#L160'>src/_pytest/config/findpaths.py:160:16:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/pytest-dev/pytest/blob/c5a75f2498c86850c4ce13bcf10d56efc92394a4/src/_pytest/config/findpaths.py#L160'>src/_pytest/config/findpaths.py:160:16:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
- <a href='https://github.com/pytest-dev/pytest/blob/c5a75f2498c86850c4ce13bcf10d56efc92394a4/src/_pytest/terminal.py#L1012'>src/_pytest/terminal.py:1012:40:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/pytest-dev/pytest/blob/c5a75f2498c86850c4ce13bcf10d56efc92394a4/src/_pytest/terminal.py#L1012'>src/_pytest/terminal.py:1012:40:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
- <a href='https://github.com/pytest-dev/pytest/blob/c5a75f2498c86850c4ce13bcf10d56efc92394a4/src/_pytest/terminal.py#L462'>src/_pytest/terminal.py:462:41:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/pytest-dev/pytest/blob/c5a75f2498c86850c4ce13bcf10d56efc92394a4/src/_pytest/terminal.py#L462'>src/_pytest/terminal.py:462:41:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/astropy/astropy/blob/98e9e9fd2736462866c9103f3ec9c82f821b5d74/astropy/io/ascii/latex.py#L438'>astropy/io/ascii/latex.py:438:16:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/astropy/astropy/blob/98e9e9fd2736462866c9103f3ec9c82f821b5d74/astropy/io/ascii/latex.py#L438'>astropy/io/ascii/latex.py:438:16:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC0207 | 92 | 46 | 46 | 0 | 0 |

</p>
</details>




---

_Review requested from @ntBre by @ntBre on 2025-06-26 01:56_

---

_Label `documentation` added by @ntBre on 2025-06-26 01:56_

---

_@ntBre requested changes on 2025-06-26 18:33_

I don't think this really addresses the problem in the issue. The confusion isn't from the rule applying to either `split` or `rsplit` calls, it's the fact that the _fix_ might change which call you need.

This is the confusing case from the issue:

```python
url = "www.example.com"
url.split(".")[-1]  # result is "com"
```

And the diagnostic from Ruff:

```
-:2:1: PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
  |
1 | url = "www.example.com"
2 | url.split(".")[-1]  # result is "com"
  | ^^^^^^^^^^^^^^^^^^ PLC0207
  |
```

The error message implies that the fix is simply to add `maxsplit=1`, but that gives a different result:

```python
```python
url = "www.example.com"
url.split(".", maxsplit=1)[-1]  # result is now "example.com" instead of "com"
```

The correct fix is to add `maxplit=1` _and_ change `split` to `rsplit`, restoring the original behavior.

That's the kind of information we need to include in the rule documentation and in the error message. In the latter case, we'll need to inspect the slice (`[0] or [-1]`) and call type (`split` or `rsplit`) and customize the error message accordingly so that we actually suggest the right thing for the specific situation.

---

_Comment by @junhsonjb on 2025-06-30 23:27_

Thanks for the comments, that makes sense! I just patched some updates to address them!

---

_Review requested from @ntBre by @junhsonjb on 2025-06-30 23:27_

---

_Review comment by @MeGaGiGaGon on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:52 on 2025-06-30 23:33_

This should probably be an `unreachable!` instead, since if that case is hit it should cause a failure instead of a silent issue.

---

_Review comment by @MeGaGiGaGon on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLC0207_missing_maxsplit_arg.py.snap`:24 on 2025-06-30 23:41_

I think you need to have both the split type and split index in `MissingMaxsplitArg`, since this is currently making the message backwards (first should stay as `split`, second should recommend switching to `rsplit`)
Edit: Looks like I failed at selecting the range, but "first" references the change above this one.

---

_@MeGaGiGaGon reviewed on 2025-06-30 23:41_

---

_@junhsonjb reviewed on 2025-07-01 11:38_

---

_Review comment by @junhsonjb on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLC0207_missing_maxsplit_arg.py.snap`:24 on 2025-07-01 11:38_

Just so that I'm clear, which cases do you mean by "first" and "second"? This particular error message (line 24 of the `.snap` file) recommends `split` because the python code is accessing the first element. With `maxsplit=1` (which we're also recommending), we need to call `split` or else we'd get the wrong output.

`rsplit` would give us something like this:
```python
>>> "1,2,3".rsplit(",", maxsplit=1)
['1,2', '3']
```
So, getting slice index `0` would yield `'1,2'` instead of the presumably desired `'1'`.

---

Also, the split type is being passed into `MissingMaxsplitArg`. It's currently called `function` (but I'm thinking that this could be named better, so I'll go in and change that)

---

_Review requested from @MeGaGiGaGon by @junhsonjb on 2025-07-01 14:07_

---

_@MeGaGiGaGon reviewed on 2025-07-01 16:02_

---

_Review comment by @MeGaGiGaGon on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLC0207_missing_maxsplit_arg.py.snap`:24 on 2025-07-01 16:02_

Condensed down to just the error and message:

```snap
14 | "1,2,3".split(",")[0]  # [missing-maxsplit-arg]
   | ^^^^^^^^^^^^^^^^^^^^^ PLC0207
missing_maxsplit_arg.py:15:1: PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`


15 | "1,2,3".split(",")[-1]  # [missing-maxsplit-arg]
   | ^^^^^^^^^^^^^^^^^^^^^^ PLC0207
missing_maxsplit_arg.py:16:1: PLC0207 Instead of `str.rsplit()`, call `str.split()` and pass `maxsplit=1`
```
The messages are backwards. `"1,2,3".split(",")[0]` should stay as `split`, but the message says to use `rsplit`. `"1,2,3".split(",")[-1]` should switch to `rsplit`, but the message says to use `split` instead of `rsplit` dispite the code not having `rsplit` to begin with.

---

_@ntBre reviewed on 2025-07-01 16:17_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:52 on 2025-07-01 16:17_

I think we should probably define a simple enum instead of using an `i64`. That way we can check for known values up front and bail out of the whole diagnostic. Then we can `match` exhaustively here.

---

_@junhsonjb reviewed on 2025-07-01 23:26_

---

_Review comment by @junhsonjb on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLC0207_missing_maxsplit_arg.py.snap`:24 on 2025-07-01 23:26_

Thank you! I've just taken a closer look at the snapshot file and I believe that each listed error pertains to the code snippet __below__ it, rather than above. For example, the first error message in the code block above refers to line 15 of the fixture (`missing_maxsplit_arg.py:15:1`). The code snippet above that error message is for line 14, meanwhile the code snippet _below it_ is for line 15 and the split types do match.

To help illustrate, here's an excerpt of the snapshot with a few comments for clarity. I've prefixed each with `# NOTE:` to differentiate from comments originally in the fixture:
```
---
source: crates/ruff_linter/src/rules/pylint/mod.rs
---
missing_maxsplit_arg.py:14:1: PLC0207 Pass `maxsplit=1` into `str.split()`  # NOTE: this error is for the call to `split()` on line 14
   |
12 | # Errors
13 | ## Test split called directly on string literal
14 | "1,2,3".split(",")[0]  # [missing-maxsplit-arg]. # NOTE: in line 14, we call `split()`
   | ^^^^^^^^^^^^^^^^^^^^^ PLC0207
15 | "1,2,3".split(",")[-1]  # [missing-maxsplit-arg]
16 | "1,2,3".rsplit(",")[0]  # [missing-maxsplit-arg]
   |

missing_maxsplit_arg.py:15:1: PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`  # NOTE: on line 15 we call `split()` as noted by this error message ("instead of `str.split()` ...")
   |
13 | ## Test split called directly on string literal
14 | "1,2,3".split(",")[0]  # [missing-maxsplit-arg]
15 | "1,2,3".split(",")[-1]  # [missing-maxsplit-arg]  # NOTE: call to `split()` on line 15
   | ^^^^^^^^^^^^^^^^^^^^^^ PLC0207
16 | "1,2,3".rsplit(",")[0]  # [missing-maxsplit-arg]
17 | "1,2,3".rsplit(",")[-1]  # [missing-maxsplit-arg]
   |

missing_maxsplit_arg.py:16:1: PLC0207 Instead of `str.rsplit()`, call `str.split()` and pass `maxsplit=1`  # NOTE: on line 16 we call `rsplit()`, as noted by this error message
   |
14 | "1,2,3".split(",")[0]  # [missing-maxsplit-arg]
15 | "1,2,3".split(",")[-1]  # [missing-maxsplit-arg]
16 | "1,2,3".rsplit(",")[0]  # [missing-maxsplit-arg]  # NOTE: call to `rsplit()` on line 16
   | ^^^^^^^^^^^^^^^^^^^^^^ PLC0207
17 | "1,2,3".rsplit(",")[-1]  # [missing-maxsplit-arg]
   |
```

So based on this structure, I think the split types are consistent with the messages, rather than swapped. Let me know what you think! 🙂 

---

_@MeGaGiGaGon reviewed on 2025-07-02 00:17_

---

_Review comment by @MeGaGiGaGon on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLC0207_missing_maxsplit_arg.py.snap`:24 on 2025-07-02 00:17_

Yep, you're right, I was misreading it

---

_Renamed from "Update `missing-maxsplit-arg` docs and error to explain `rsplit` usage" to "Update `missing-maxsplit-arg` docs and error to suggest proper `split`/`rsplit` usage along with `maxsplit=1`" by @junhsonjb on 2025-07-02 23:01_

---

_Renamed from "Update `missing-maxsplit-arg` docs and error to suggest proper `split`/`rsplit` usage along with `maxsplit=1`" to "Update `missing-maxsplit-arg` docs and error to suggest proper usage" by @junhsonjb on 2025-07-02 23:01_

---

_Comment by @junhsonjb on 2025-07-08 14:39_

Hey @ntBre — just wanted to check in on this. I’ve pushed some updates addressing your feedback above. When you get the chance, let me know if there’s anything else you’d like me to adjust. Thanks again! 🙂

---

_@ntBre approved on 2025-07-08 16:51_

This looks great, thank you! And thanks for the ping.

I'm pretty sure these changes give us all the information we need to generate an autofix too. Feel free to work on that in a follow-up PR if you're interested :)

---

_Renamed from "Update `missing-maxsplit-arg` docs and error to suggest proper usage" to "[`pylint`] Update `missing-maxsplit-arg` docs and error to suggest proper usage (`PLC0207`)" by @ntBre on 2025-07-08 16:53_

---

_Merged by @ntBre on 2025-07-08 16:53_

---

_Closed by @ntBre on 2025-07-08 16:53_

---

_Comment by @junhsonjb on 2025-07-16 18:21_

> This looks great, thank you! And thanks for the ping.
> 
> I'm pretty sure these changes give us all the information we need to generate an autofix too. Feel free to work on that in a follow-up PR if you're interested :)

@ntBre Great idea! PR is [here](https://github.com/astral-sh/ruff/pull/19387)

---

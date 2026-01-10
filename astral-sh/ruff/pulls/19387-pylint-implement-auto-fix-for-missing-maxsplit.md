```yaml
number: 19387
title: "[`pylint`] Implement auto-fix for `missing-maxsplit-arg` (`PLC0207`)"
type: pull_request
state: merged
author: junhsonjb
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: jjb-maxsplit-autofix
created_at: 2025-07-16T18:16:14Z
updated_at: 2025-07-28T14:45:26Z
url: https://github.com/astral-sh/ruff/pull/19387
synced_at: 2026-01-10T17:58:13Z
```

# [`pylint`] Implement auto-fix for `missing-maxsplit-arg` (`PLC0207`)

---

_Pull request opened by @junhsonjb on 2025-07-16 18:16_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
As a follow-up to #18949 (suggested [here](https://github.com/astral-sh/ruff/pull/18949#pullrequestreview-2998417889)), this PR implements auto-fix logic for `PLC0207`.

## Test Plan

<!-- How was it tested? -->

Existing tests pass, with updates to the snapshot so that it expects the new output that comes along with the auto-fix.


---

_@junhsonjb reviewed on 2025-07-16 18:18_

---

_Review comment by @junhsonjb on `crates/ruff/tests/lint.rs`:5723 on 2025-07-16 18:18_

I had to add this because on macOS, `tempdir` puts temp files in a directory that starts with `/private`, which wasn't previously included in the filters. 

Please let me know if I should be handling this differently! 🙂

---

_Comment by @github-actions[bot] on 2025-07-16 18:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+62 -62 violations, +0 -0 fixes in 13 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/PlasmaPy/PlasmaPy/blob/9b3014e1ec145b8221bf4231699c446787f87598/noxfile.py#L453'>noxfile.py:453:16:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/9b3014e1ec145b8221bf4231699c446787f87598/noxfile.py#L453'>noxfile.py:453:16:</a> PLC0207 [*] Replace with `rsplit(..., maxsplit=1)`.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+11 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/f02b8a865325344923a74c99165a765ae160232a/dev/backport/update_backport_status.py#L78'>dev/backport/update_backport_status.py:78:21:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
+ <a href='https://github.com/apache/airflow/blob/f02b8a865325344923a74c99165a765ae160232a/dev/backport/update_backport_status.py#L78'>dev/backport/update_backport_status.py:78:21:</a> PLC0207 [*] Replace with `rsplit(..., maxsplit=1)`.
- <a href='https://github.com/apache/airflow/blob/f02b8a865325344923a74c99165a765ae160232a/dev/breeze/src/airflow_breeze/commands/common_options.py#L502'>dev/breeze/src/airflow_breeze/commands/common_options.py:502:26:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
+ <a href='https://github.com/apache/airflow/blob/f02b8a865325344923a74c99165a765ae160232a/dev/breeze/src/airflow_breeze/commands/common_options.py#L502'>dev/breeze/src/airflow_breeze/commands/common_options.py:502:26:</a> PLC0207 [*] Replace with `split(..., maxsplit=1)`.
- <a href='https://github.com/apache/airflow/blob/f02b8a865325344923a74c99165a765ae160232a/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2633'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2633:50:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
+ <a href='https://github.com/apache/airflow/blob/f02b8a865325344923a74c99165a765ae160232a/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2633'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2633:50:</a> PLC0207 [*] Replace with `rsplit(..., maxsplit=1)`.
- <a href='https://github.com/apache/airflow/blob/f02b8a865325344923a74c99165a765ae160232a/dev/breeze/src/airflow_breeze/prepare_providers/provider_documentation.py#L714'>dev/breeze/src/airflow_breeze/prepare_providers/provider_documentation.py:714:34:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
+ <a href='https://github.com/apache/airflow/blob/f02b8a865325344923a74c99165a765ae160232a/dev/breeze/src/airflow_breeze/prepare_providers/provider_documentation.py#L714'>dev/breeze/src/airflow_breeze/prepare_providers/provider_documentation.py:714:34:</a> PLC0207 [*] Replace with `split(..., maxsplit=1)`.
- <a href='https://github.com/apache/airflow/blob/f02b8a865325344923a74c99165a765ae160232a/dev/breeze/src/airflow_breeze/utils/run_utils.py#L120'>dev/breeze/src/airflow_breeze/utils/run_utils.py:120:25:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
+ <a href='https://github.com/apache/airflow/blob/f02b8a865325344923a74c99165a765ae160232a/dev/breeze/src/airflow_breeze/utils/run_utils.py#L120'>dev/breeze/src/airflow_breeze/utils/run_utils.py:120:25:</a> PLC0207 [*] Replace with `rsplit(..., maxsplit=1)`.
... 12 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/9c771fb2baf78488f85dd3acad5af3538fb46077/tests/common/query_context_generator.py#L214'>tests/common/query_context_generator.py:214:29:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
+ <a href='https://github.com/apache/superset/blob/9c771fb2baf78488f85dd3acad5af3538fb46077/tests/common/query_context_generator.py#L214'>tests/common/query_context_generator.py:214:29:</a> PLC0207 [*] Replace with `split(..., maxsplit=1)`.
- <a href='https://github.com/apache/superset/blob/9c771fb2baf78488f85dd3acad5af3538fb46077/tests/common/query_context_generator.py#L255'>tests/common/query_context_generator.py:255:22:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
+ <a href='https://github.com/apache/superset/blob/9c771fb2baf78488f85dd3acad5af3538fb46077/tests/common/query_context_generator.py#L255'>tests/common/query_context_generator.py:255:22:</a> PLC0207 [*] Replace with `split(..., maxsplit=1)`.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/notebook.py#L663'>src/bokeh/io/notebook.py:663:26:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/notebook.py#L663'>src/bokeh/io/notebook.py:663:26:</a> PLC0207 [*] Replace with `split(..., maxsplit=1)`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/token.py#L142'>src/bokeh/util/token.py:142:41:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/token.py#L142'>src/bokeh/util/token.py:142:41:</a> PLC0207 [*] Replace with `split(..., maxsplit=1)`.
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/token.py#L155'>src/bokeh/util/token.py:155:41:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/token.py#L155'>src/bokeh/util/token.py:155:41:</a> PLC0207 [*] Replace with `split(..., maxsplit=1)`.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/freedomofpress/securedrop/blob/375c3a8a25cdadf854f2caa37bcbe7fe3ca72cd4/securedrop/store.py#L345'>securedrop/store.py:345:49:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
+ <a href='https://github.com/freedomofpress/securedrop/blob/375c3a8a25cdadf854f2caa37bcbe7fe3ca72cd4/securedrop/store.py#L345'>securedrop/store.py:345:49:</a> PLC0207 [*] Replace with `split(..., maxsplit=1)`.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/226877059b6e0d7f00ca2c38a4a19f9a9c8efab1/ibis/examples/gen_registry.py#L125'>ibis/examples/gen_registry.py:125:52:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
+ <a href='https://github.com/ibis-project/ibis/blob/226877059b6e0d7f00ca2c38a4a19f9a9c8efab1/ibis/examples/gen_registry.py#L125'>ibis/examples/gen_registry.py:125:52:</a> PLC0207 [*] Replace with `split(..., maxsplit=1)`.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+22 -22 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/langchain-ai/langchain/blob/0d6f9154420652ccd61cf3ff6e86393816d7520f/libs/core/langchain_core/_api/deprecation.py#L360'>libs/core/langchain_core/_api/deprecation.py:360:17:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
+ <a href='https://github.com/langchain-ai/langchain/blob/0d6f9154420652ccd61cf3ff6e86393816d7520f/libs/core/langchain_core/_api/deprecation.py#L360'>libs/core/langchain_core/_api/deprecation.py:360:17:</a> PLC0207 [*] Replace with `rsplit(..., maxsplit=1)`.
- <a href='https://github.com/langchain-ai/langchain/blob/0d6f9154420652ccd61cf3ff6e86393816d7520f/libs/core/langchain_core/_api/deprecation.py#L360'>libs/core/langchain_core/_api/deprecation.py:360:56:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
+ <a href='https://github.com/langchain-ai/langchain/blob/0d6f9154420652ccd61cf3ff6e86393816d7520f/libs/core/langchain_core/_api/deprecation.py#L360'>libs/core/langchain_core/_api/deprecation.py:360:56:</a> PLC0207 [*] Replace with `rsplit(..., maxsplit=1)`.
- <a href='https://github.com/langchain-ai/langchain/blob/0d6f9154420652ccd61cf3ff6e86393816d7520f/libs/core/langchain_core/_api/deprecation.py#L368'>libs/core/langchain_core/_api/deprecation.py:368:17:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
+ <a href='https://github.com/langchain-ai/langchain/blob/0d6f9154420652ccd61cf3ff6e86393816d7520f/libs/core/langchain_core/_api/deprecation.py#L368'>libs/core/langchain_core/_api/deprecation.py:368:17:</a> PLC0207 [*] Replace with `rsplit(..., maxsplit=1)`.
- <a href='https://github.com/langchain-ai/langchain/blob/0d6f9154420652ccd61cf3ff6e86393816d7520f/libs/core/langchain_core/_api/deprecation.py#L369'>libs/core/langchain_core/_api/deprecation.py:369:16:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
+ <a href='https://github.com/langchain-ai/langchain/blob/0d6f9154420652ccd61cf3ff6e86393816d7520f/libs/core/langchain_core/_api/deprecation.py#L369'>libs/core/langchain_core/_api/deprecation.py:369:16:</a> PLC0207 [*] Replace with `rsplit(..., maxsplit=1)`.
- <a href='https://github.com/langchain-ai/langchain/blob/0d6f9154420652ccd61cf3ff6e86393816d7520f/libs/core/langchain_core/_api/deprecation.py#L473'>libs/core/langchain_core/_api/deprecation.py:473:24:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
+ <a href='https://github.com/langchain-ai/langchain/blob/0d6f9154420652ccd61cf3ff6e86393816d7520f/libs/core/langchain_core/_api/deprecation.py#L473'>libs/core/langchain_core/_api/deprecation.py:473:24:</a> PLC0207 [*] Replace with `split(..., maxsplit=1)`.
- <a href='https://github.com/langchain-ai/langchain/blob/0d6f9154420652ccd61cf3ff6e86393816d7520f/libs/core/langchain_core/_api/deprecation.py#L492'>libs/core/langchain_core/_api/deprecation.py:492:27:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
+ <a href='https://github.com/langchain-ai/langchain/blob/0d6f9154420652ccd61cf3ff6e86393816d7520f/libs/core/langchain_core/_api/deprecation.py#L492'>libs/core/langchain_core/_api/deprecation.py:492:27:</a> PLC0207 [*] Replace with `split(..., maxsplit=1)`.
- <a href='https://github.com/langchain-ai/langchain/blob/0d6f9154420652ccd61cf3ff6e86393816d7520f/libs/core/langchain_core/runnables/graph_mermaid.py#L157'>libs/core/langchain_core/runnables/graph_mermaid.py:157:24:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
+ <a href='https://github.com/langchain-ai/langchain/blob/0d6f9154420652ccd61cf3ff6e86393816d7520f/libs/core/langchain_core/runnables/graph_mermaid.py#L157'>libs/core/langchain_core/runnables/graph_mermaid.py:157:24:</a> PLC0207 [*] Replace with `rsplit(..., maxsplit=1)`.
- <a href='https://github.com/langchain-ai/langchain/blob/0d6f9154420652ccd61cf3ff6e86393816d7520f/libs/core/langchain_core/utils/mustache.py#L85'>libs/core/langchain_core/utils/mustache.py:85:19:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
+ <a href='https://github.com/langchain-ai/langchain/blob/0d6f9154420652ccd61cf3ff6e86393816d7520f/libs/core/langchain_core/utils/mustache.py#L85'>libs/core/langchain_core/utils/mustache.py:85:19:</a> PLC0207 [*] Replace with `rsplit(..., maxsplit=1)`.
- <a href='https://github.com/langchain-ai/langchain/blob/0d6f9154420652ccd61cf3ff6e86393816d7520f/libs/core/langchain_core/utils/utils.py#L140'>libs/core/langchain_core/utils/utils.py:140:32:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
... 27 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/e954f19a0b68414107c3847f0af9bb76051a719b/pandas/compat/_optional.py#L165'>pandas/compat/_optional.py:165:14:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
+ <a href='https://github.com/pandas-dev/pandas/blob/e954f19a0b68414107c3847f0af9bb76051a719b/pandas/compat/_optional.py#L165'>pandas/compat/_optional.py:165:14:</a> PLC0207 [*] Replace with `split(..., maxsplit=1)`.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pypa/cibuildwheel/blob/13b086e3c6a8a225ee99a028fcf5b820a9d2660f/bin/update_pythons.py#L158'>bin/update_pythons.py:158:22:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
+ <a href='https://github.com/pypa/cibuildwheel/blob/13b086e3c6a8a225ee99a028fcf5b820a9d2660f/bin/update_pythons.py#L158'>bin/update_pythons.py:158:22:</a> PLC0207 [*] Replace with `split(..., maxsplit=1)`.
- <a href='https://github.com/pypa/cibuildwheel/blob/13b086e3c6a8a225ee99a028fcf5b820a9d2660f/cibuildwheel/platforms/macos.py#L153'>cibuildwheel/platforms/macos.py:153:31:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
+ <a href='https://github.com/pypa/cibuildwheel/blob/13b086e3c6a8a225ee99a028fcf5b820a9d2660f/cibuildwheel/platforms/macos.py#L153'>cibuildwheel/platforms/macos.py:153:31:</a> PLC0207 [*] Replace with `rsplit(..., maxsplit=1)`.
- <a href='https://github.com/pypa/cibuildwheel/blob/13b086e3c6a8a225ee99a028fcf5b820a9d2660f/cibuildwheel/selector.py#L79'>cibuildwheel/selector.py:79:26:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
+ <a href='https://github.com/pypa/cibuildwheel/blob/13b086e3c6a8a225ee99a028fcf5b820a9d2660f/cibuildwheel/selector.py#L79'>cibuildwheel/selector.py:79:26:</a> PLC0207 [*] Replace with `split(..., maxsplit=1)`.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+12 -12 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/25fbb05fea48e3deb3ce30078cba0d7bad856da5/zerver/lib/send_email.py#L287'>zerver/lib/send_email.py:287:16:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
+ <a href='https://github.com/zulip/zulip/blob/25fbb05fea48e3deb3ce30078cba0d7bad856da5/zerver/lib/send_email.py#L287'>zerver/lib/send_email.py:287:16:</a> PLC0207 [*] Replace with `rsplit(..., maxsplit=1)`.
- <a href='https://github.com/zulip/zulip/blob/25fbb05fea48e3deb3ce30078cba0d7bad856da5/zerver/lib/send_email.py#L429'>zerver/lib/send_email.py:429:21:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
+ <a href='https://github.com/zulip/zulip/blob/25fbb05fea48e3deb3ce30078cba0d7bad856da5/zerver/lib/send_email.py#L429'>zerver/lib/send_email.py:429:21:</a> PLC0207 [*] Replace with `rsplit(..., maxsplit=1)`.
- <a href='https://github.com/zulip/zulip/blob/25fbb05fea48e3deb3ce30078cba0d7bad856da5/zerver/lib/upload/local.py#L67'>zerver/lib/upload/local.py:67:17:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
+ <a href='https://github.com/zulip/zulip/blob/25fbb05fea48e3deb3ce30078cba0d7bad856da5/zerver/lib/upload/local.py#L67'>zerver/lib/upload/local.py:67:17:</a> PLC0207 [*] Replace with `rsplit(..., maxsplit=1)`.
- <a href='https://github.com/zulip/zulip/blob/25fbb05fea48e3deb3ce30078cba0d7bad856da5/zerver/lib/upload/s3.py#L176'>zerver/lib/upload/s3.py:176:25:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
+ <a href='https://github.com/zulip/zulip/blob/25fbb05fea48e3deb3ce30078cba0d7bad856da5/zerver/lib/upload/s3.py#L176'>zerver/lib/upload/s3.py:176:25:</a> PLC0207 [*] Replace with `rsplit(..., maxsplit=1)`.
- <a href='https://github.com/zulip/zulip/blob/25fbb05fea48e3deb3ce30078cba0d7bad856da5/zerver/lib/webhooks/common.py#L249'>zerver/lib/webhooks/common.py:249:26:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
+ <a href='https://github.com/zulip/zulip/blob/25fbb05fea48e3deb3ce30078cba0d7bad856da5/zerver/lib/webhooks/common.py#L249'>zerver/lib/webhooks/common.py:249:26:</a> PLC0207 [*] Replace with `split(..., maxsplit=1)`.
... 14 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/wntrblm/nox">wntrblm/nox</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/wntrblm/nox/blob/7f93235c78b48024bfb3d23c48c8952bac270f44/tests/test_sessions.py#L1199'>tests/test_sessions.py:1199:44:</a> PLC0207 Instead of `str.split()`, call `str.rsplit()` and pass `maxsplit=1`
+ <a href='https://github.com/wntrblm/nox/blob/7f93235c78b48024bfb3d23c48c8952bac270f44/tests/test_sessions.py#L1199'>tests/test_sessions.py:1199:44:</a> PLC0207 [*] Replace with `rsplit(..., maxsplit=1)`.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pytest-dev/pytest/blob/cfc66062cb298e8b90a2849127e2104cd5070293/src/_pytest/config/findpaths.py#L160'>src/_pytest/config/findpaths.py:160:16:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
+ <a href='https://github.com/pytest-dev/pytest/blob/cfc66062cb298e8b90a2849127e2104cd5070293/src/_pytest/config/findpaths.py#L160'>src/_pytest/config/findpaths.py:160:16:</a> PLC0207 [*] Replace with `split(..., maxsplit=1)`.
- <a href='https://github.com/pytest-dev/pytest/blob/cfc66062cb298e8b90a2849127e2104cd5070293/src/_pytest/terminal.py#L1012'>src/_pytest/terminal.py:1012:40:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
+ <a href='https://github.com/pytest-dev/pytest/blob/cfc66062cb298e8b90a2849127e2104cd5070293/src/_pytest/terminal.py#L1012'>src/_pytest/terminal.py:1012:40:</a> PLC0207 [*] Replace with `split(..., maxsplit=1)`.
- <a href='https://github.com/pytest-dev/pytest/blob/cfc66062cb298e8b90a2849127e2104cd5070293/src/_pytest/terminal.py#L462'>src/_pytest/terminal.py:462:41:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
+ <a href='https://github.com/pytest-dev/pytest/blob/cfc66062cb298e8b90a2849127e2104cd5070293/src/_pytest/terminal.py#L462'>src/_pytest/terminal.py:462:41:</a> PLC0207 [*] Replace with `split(..., maxsplit=1)`.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/astropy/astropy/blob/db7a3e2ed3a28ea86594d92da686453d0184b242/astropy/io/ascii/latex.py#L438'>astropy/io/ascii/latex.py:438:16:</a> PLC0207 Pass `maxsplit=1` into `str.split()`
+ <a href='https://github.com/astropy/astropy/blob/db7a3e2ed3a28ea86594d92da686453d0184b242/astropy/io/ascii/latex.py#L438'>astropy/io/ascii/latex.py:438:16:</a> PLC0207 [*] Replace with `split(..., maxsplit=1)`.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC0207 | 124 | 62 | 62 | 0 | 0 |

</p>
</details>




---

_Review requested from @ntBre by @ntBre on 2025-07-16 19:12_

---

_@junhsonjb reviewed on 2025-07-18 11:44_

---

_Review comment by @junhsonjb on `crates/ruff/tests/lint.rs`:5723 on 2025-07-18 11:44_

This was changed in separate PR (linked/mentioned below), so I rebased and this is no longer an issue! Resolving this comment 🙂

---

_@ntBre reviewed on 2025-07-18 13:05_

---

_Review comment by @ntBre on `crates/ruff/tests/lint.rs`:5723 on 2025-07-18 13:05_

Yeah, good catch on that! I noticed  your comment and mentioned it in the PR but never responded. Thank you!

---

_Label `fixes` added by @ntBre on 2025-07-21 20:33_

---

_Label `preview` added by @ntBre on 2025-07-21 20:33_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:78 on 2025-07-21 20:49_

These look quite repetitive with the `message` above, especially the first case, which is exactly the same. How hard would it be to make a message like:

``Replace with `{suggested_split_type}(..., maxsplit=1)`.``

I don't really love that either, but I don't think we should duplicate the message. Ideally we could include the actual variable name and pattern, but that would require a bit of extra work.

Another option might be changing `message` to something more general instead. I think the more detailed suggestion is a better fit here in `fix_title`, especially since it's `AlwaysFixable`.

What do you think?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:176 on 2025-07-21 20:56_

I was checking if other calls to `add_argument` were safe or not, and I found a few examples like this:

https://github.com/astral-sh/ruff/blob/cabdd969ec3b7807a777ac87de94771a67525777/crates/ruff_linter/src/rules/pylint/rules/subprocess_run_without_check.rs#L77-L93

I think we should probably do the same here and also add a `## Fix safety` section to the docs. I can't think of another reason it would be unsafe because we shouldn't be able to delete any comments with these edits.

Here's a PR adding one of these, for a `## Fix safety` example too:

https://github.com/astral-sh/ruff/pull/9176/files

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLC0207_missing_maxsplit_arg.py.snap`:655 on 2025-07-21 21:02_

Nice, we already have a test for this. This should become an unsafe fix because we can't be sure that `maxsplit` isn't already in `kwargs_without_maxsplit`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLC0207_missing_maxsplit_arg.py.snap`:676 on 2025-07-21 21:04_

We could even consider bailing out with no fix at all if `**kwargs` are present because this case will cause a runtime error. I don't feel too strongly either way, we have rules that do both. I think this is fairly rare in general.

---

_@ntBre reviewed on 2025-07-21 21:06_

Thanks, this is looking good! I just had some ideas/questions about the diagnostic messages and fix safety.

---

_@junhsonjb reviewed on 2025-07-23 13:49_

---

_Review comment by @junhsonjb on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:78 on 2025-07-23 13:49_

I agree that the `message` and `fix_title` are redundant and that it's better to have more detail in the `fix_title`. I also agree that including the actual variable name and pattern is ideal, but I think that might fall out of the scope for this PR. Perhaps we go with a more general message and leave that as a potential follow-up? 

I think since this is `AlwaysFixable` we can get away with a general `message` especially because the `fix_title` will always be there to provide more detail. How does that sound to you?

---

_@ntBre reviewed on 2025-07-23 13:59_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:78 on 2025-07-23 13:59_

That sounds good to me!

---

_Comment by @junhsonjb on 2025-07-25 14:11_

Thanks for the comments, in addition to the updated `message` that we discussed, I added the `## Fix Safety` like you suggested and marked the fix as unsafe when `**kwargs` is present! Let me know what you think of the changes!

---

_Review requested from @ntBre by @junhsonjb on 2025-07-25 14:11_

---

_@ntBre approved on 2025-07-28 14:45_

Thank you!

---

_Merged by @ntBre on 2025-07-28 14:45_

---

_Closed by @ntBre on 2025-07-28 14:45_

---

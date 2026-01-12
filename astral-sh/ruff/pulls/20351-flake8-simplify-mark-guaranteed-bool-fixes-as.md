```yaml
number: 20351
title: "[`flake8-simplify`] Mark guaranteed-bool fixes as safe `SIM103`"
type: pull_request
state: open
author: TaKO8Ki
labels: []
assignees: []
base: main
head: sim103-safe-applicability
created_at: 2025-09-11T16:49:15Z
updated_at: 2025-11-21T07:59:33Z
url: https://github.com/astral-sh/ruff/pull/20351
synced_at: 2026-01-12T15:56:59Z
```

# [`flake8-simplify`] Mark guaranteed-bool fixes as safe `SIM103`

---

_@TaKO8Ki_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #20287

Mark guaranteed-bool fixes as safe.

- Mark fixes as `Applicability::Safe` when the replacement is guaranteed boolean
    - not <expr>, identity/membership compares (is/is not/in/not in), builtin bool(...)
- Keep `Applicability::Unsafe` for equality/inequality (==/!=) and other non‑guaranteed cases
- Avoid offering a fix when bool is shadowed and expression isn’t guaranteed boolean

## Test Plan

<!-- How was it tested? -->

Update the snapshot of `SIM103.py`


---

_Review comment by @TaKO8Ki on `crates/ruff_linter/src/rules/flake8_simplify/rules/needless_bool.rs`:287 on 2025-09-11 16:50_

While some `if/else` chains were refactored into match expressions, the core logic for constructing the condition expression remains unchanged. This change only annotates applicability.

---

_@TaKO8Ki reviewed on 2025-09-11 16:50_

---

_Comment by @github-actions[bot] on 2025-09-11 17:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +180 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +74 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/api_fastapi/core_api/services/ui/calendar.py#L326'>airflow-core/src/airflow/api_fastapi/core_api/services/ui/calendar.py:326:9:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/api_fastapi/core_api/services/ui/calendar.py#L326'>airflow-core/src/airflow/api_fastapi/core_api/services/ui/calendar.py:326:9:</a> SIM103 [*] Return the negated condition directly
- <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/configuration.py#L1370'>airflow-core/src/airflow/configuration.py:1370:13:</a> SIM103 Return the condition `value is not None` directly
+ <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/configuration.py#L1370'>airflow-core/src/airflow/configuration.py:1370:13:</a> SIM103 [*] Return the condition `value is not None` directly
- <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/executors/executor_utils.py#L48'>airflow-core/src/airflow/executors/executor_utils.py:48:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/executors/executor_utils.py#L48'>airflow-core/src/airflow/executors/executor_utils.py:48:9:</a> SIM103 [*] Return the condition directly
- <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/migrations/env.py#L37'>airflow-core/src/airflow/migrations/env.py:37:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/migrations/env.py#L37'>airflow-core/src/airflow/migrations/env.py:37:5:</a> SIM103 [*] Return the negated condition directly
- <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/settings.py#L331'>airflow-core/src/airflow/settings.py:331:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/settings.py#L331'>airflow-core/src/airflow/settings.py:331:5:</a> SIM103 [*] Return the negated condition directly
- <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/ti_deps/deps/trigger_rule_dep.py#L222'>airflow-core/src/airflow/ti_deps/deps/trigger_rule_dep.py:222:13:</a> SIM103 Return the condition directly
+ <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/ti_deps/deps/trigger_rule_dep.py#L222'>airflow-core/src/airflow/ti_deps/deps/trigger_rule_dep.py:222:13:</a> SIM103 [*] Return the condition directly
- <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/utils/platform.py#L50'>airflow-core/src/airflow/utils/platform.py:50:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/utils/platform.py#L50'>airflow-core/src/airflow/utils/platform.py:50:5:</a> SIM103 [*] Return the condition directly
- <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/utils/sqlalchemy.py#L439'>airflow-core/src/airflow/utils/sqlalchemy.py:439:5:</a> SIM103 Return the condition `db_err_code in ("55P03", 1205, 3572)` directly
+ <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/utils/sqlalchemy.py#L439'>airflow-core/src/airflow/utils/sqlalchemy.py:439:5:</a> SIM103 [*] Return the condition `db_err_code in ("55P03", 1205, 3572)` directly
- <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/utils/strings.py#L37'>airflow-core/src/airflow/utils/strings.py:37:5:</a> SIM103 Return the condition `astring.strip().lower() in TRUE_LIKE_VALUES` directly
+ <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/utils/strings.py#L37'>airflow-core/src/airflow/utils/strings.py:37:5:</a> SIM103 [*] Return the condition `astring.strip().lower() in TRUE_LIKE_VALUES` directly
- <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/dev/breeze/src/airflow_breeze/commands/common_options.py#L507'>dev/breeze/src/airflow_breeze/commands/common_options.py:507:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/dev/breeze/src/airflow_breeze/commands/common_options.py#L507'>dev/breeze/src/airflow_breeze/commands/common_options.py:507:5:</a> SIM103 [*] Return the negated condition directly
... 54 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +10 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/b42060c8808778e86ca9f55bd9cfe74d1124793a/superset/commands/importers/v1/utils.py#L223'>superset/commands/importers/v1/utils.py:223:5:</a> SIM103 Return the condition `path.suffix.lower() in {".yaml", ".yml"}` directly
+ <a href='https://github.com/apache/superset/blob/b42060c8808778e86ca9f55bd9cfe74d1124793a/superset/commands/importers/v1/utils.py#L223'>superset/commands/importers/v1/utils.py:223:5:</a> SIM103 [*] Return the condition `path.suffix.lower() in {".yaml", ".yml"}` directly
- <a href='https://github.com/apache/superset/blob/b42060c8808778e86ca9f55bd9cfe74d1124793a/superset/db_engine_specs/dremio.py#L80'>superset/db_engine_specs/dremio.py:80:9:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/apache/superset/blob/b42060c8808778e86ca9f55bd9cfe74d1124793a/superset/db_engine_specs/dremio.py#L80'>superset/db_engine_specs/dremio.py:80:9:</a> SIM103 [*] Return the negated condition directly
- <a href='https://github.com/apache/superset/blob/b42060c8808778e86ca9f55bd9cfe74d1124793a/superset/extensions/metastore_cache.py#L121'>superset/extensions/metastore_cache.py:121:9:</a> SIM103 Return the condition `bool(entry)` directly
+ <a href='https://github.com/apache/superset/blob/b42060c8808778e86ca9f55bd9cfe74d1124793a/superset/extensions/metastore_cache.py#L121'>superset/extensions/metastore_cache.py:121:9:</a> SIM103 [*] Return the condition `bool(entry)` directly
- <a href='https://github.com/apache/superset/blob/b42060c8808778e86ca9f55bd9cfe74d1124793a/superset/security/manager.py#L585'>superset/security/manager.py:585:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/apache/superset/blob/b42060c8808778e86ca9f55bd9cfe74d1124793a/superset/security/manager.py#L585'>superset/security/manager.py:585:9:</a> SIM103 [*] Return the condition directly
- <a href='https://github.com/apache/superset/blob/b42060c8808778e86ca9f55bd9cfe74d1124793a/superset/themes/utils.py#L85'>superset/themes/utils.py:85:9:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/apache/superset/blob/b42060c8808778e86ca9f55bd9cfe74d1124793a/superset/themes/utils.py#L85'>superset/themes/utils.py:85:9:</a> SIM103 [*] Return the negated condition directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+0 -0 violations, +18 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/crazy_utils.py#L176'>crazy_functions/crazy_utils.py:176:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/crazy_utils.py#L176'>crazy_functions/crazy_utils.py:176:9:</a> SIM103 [*] Return the condition directly
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/crazy_utils.py#L506'>crazy_functions/crazy_utils.py:506:17:</a> SIM103 Return the condition `bool(match)` directly
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/crazy_utils.py#L506'>crazy_functions/crazy_utils.py:506:17:</a> SIM103 [*] Return the condition `bool(match)` directly
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/doc_fns/content_folder.py#L81'>crazy_functions/doc_fns/content_folder.py:81:13:</a> SIM103 Return the condition `not self.size < 0` directly
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/doc_fns/content_folder.py#L81'>crazy_functions/doc_fns/content_folder.py:81:13:</a> SIM103 [*] Return the condition `not self.size < 0` directly
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/paper_fns/document_structure_extractor.py#L379'>crazy_functions/paper_fns/document_structure_extractor.py:379:13:</a> SIM103 Return the condition directly
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/paper_fns/document_structure_extractor.py#L379'>crazy_functions/paper_fns/document_structure_extractor.py:379:13:</a> SIM103 [*] Return the condition directly
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/request_llms/bridge_spark.py#L12'>request_llms/bridge_spark.py:12:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/request_llms/bridge_spark.py#L12'>request_llms/bridge_spark.py:12:5:</a> SIM103 [*] Return the negated condition directly
... 8 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/server_auth/auth.py#L40'>examples/server/app/server_auth/auth.py:40:9:</a> SIM103 Return the condition `bool(username == "bokeh" and password == "bokeh")` directly
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/server_auth/auth.py#L40'>examples/server/app/server_auth/auth.py:40:9:</a> SIM103 [*] Return the condition `bool(username == "bokeh" and password == "bokeh")` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/277eff6188e9cd65e02988c449d3ec5bb4134648/src/latch/types/utils.py#L21'>src/latch/types/utils.py:21:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/latchbio/latch/blob/277eff6188e9cd65e02988c449d3ec5bb4134648/src/latch/types/utils.py#L21'>src/latch/types/utils.py:21:5:</a> SIM103 [*] Return the negated condition directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +74 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/corporate/lib/stripe.py#L1032'>corporate/lib/stripe.py:1032:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/corporate/lib/stripe.py#L1032'>corporate/lib/stripe.py:1032:9:</a> SIM103 [*] Return the condition directly
- <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/corporate/lib/stripe.py#L4257'>corporate/lib/stripe.py:4257:9:</a> SIM103 Return the condition `plan_tier in implemented_plan_tiers` directly
+ <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/corporate/lib/stripe.py#L4257'>corporate/lib/stripe.py:4257:9:</a> SIM103 [*] Return the condition `plan_tier in implemented_plan_tiers` directly
- <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/corporate/lib/stripe.py#L4673'>corporate/lib/stripe.py:4673:9:</a> SIM103 Return the condition `plan_tier in implemented_plan_tiers` directly
+ <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/corporate/lib/stripe.py#L4673'>corporate/lib/stripe.py:4673:9:</a> SIM103 [*] Return the condition `plan_tier in implemented_plan_tiers` directly
- <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/corporate/lib/stripe.py#L5119'>corporate/lib/stripe.py:5119:9:</a> SIM103 Return the condition `plan_tier in implemented_plan_tiers` directly
+ <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/corporate/lib/stripe.py#L5119'>corporate/lib/stripe.py:5119:9:</a> SIM103 [*] Return the condition `plan_tier in implemented_plan_tiers` directly
- <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/scripts/lib/zulip_tools.py#L151'>scripts/lib/zulip_tools.py:151:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/scripts/lib/zulip_tools.py#L151'>scripts/lib/zulip_tools.py:151:5:</a> SIM103 [*] Return the condition directly
- <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/scripts/lib/zulip_tools.py#L549'>scripts/lib/zulip_tools.py:549:5:</a> SIM103 Return the condition `bool("posix" in os.name and os.geteuid() == 0)` directly
+ <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/scripts/lib/zulip_tools.py#L549'>scripts/lib/zulip_tools.py:549:5:</a> SIM103 [*] Return the condition `bool("posix" in os.name and os.geteuid() == 0)` directly
- <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/tools/lib/provision_inner.py#L110'>tools/lib/provision_inner.py:110:5:</a> SIM103 Return the condition `"microsoft" in result.stdout.lower()` directly
+ <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/tools/lib/provision_inner.py#L110'>tools/lib/provision_inner.py:110:5:</a> SIM103 [*] Return the condition `"microsoft" in result.stdout.lower()` directly
- <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/zerver/lib/channel_folders.py#L89'>zerver/lib/channel_folders.py:89:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/zerver/lib/channel_folders.py#L89'>zerver/lib/channel_folders.py:89:5:</a> SIM103 [*] Return the condition directly
- <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/zerver/lib/compatibility.py#L156'>zerver/lib/compatibility.py:156:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/zerver/lib/compatibility.py#L156'>zerver/lib/compatibility.py:156:5:</a> SIM103 [*] Return the negated condition directly
- <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/zerver/lib/markdown/__init__.py#L1968'>zerver/lib/markdown/__init__.py:1968:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/zerver/lib/markdown/__init__.py#L1968'>zerver/lib/markdown/__init__.py:1968:9:</a> SIM103 [*] Return the condition directly
... 54 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM103 | 180 | 0 | 0 | 180 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +180 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +74 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/api_fastapi/core_api/services/ui/calendar.py#L326'>airflow-core/src/airflow/api_fastapi/core_api/services/ui/calendar.py:326:9:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/api_fastapi/core_api/services/ui/calendar.py#L326'>airflow-core/src/airflow/api_fastapi/core_api/services/ui/calendar.py:326:9:</a> SIM103 [*] Return the negated condition directly
- <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/configuration.py#L1370'>airflow-core/src/airflow/configuration.py:1370:13:</a> SIM103 Return the condition `value is not None` directly
+ <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/configuration.py#L1370'>airflow-core/src/airflow/configuration.py:1370:13:</a> SIM103 [*] Return the condition `value is not None` directly
- <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/executors/executor_utils.py#L48'>airflow-core/src/airflow/executors/executor_utils.py:48:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/executors/executor_utils.py#L48'>airflow-core/src/airflow/executors/executor_utils.py:48:9:</a> SIM103 [*] Return the condition directly
- <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/migrations/env.py#L37'>airflow-core/src/airflow/migrations/env.py:37:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/migrations/env.py#L37'>airflow-core/src/airflow/migrations/env.py:37:5:</a> SIM103 [*] Return the negated condition directly
- <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/settings.py#L331'>airflow-core/src/airflow/settings.py:331:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/settings.py#L331'>airflow-core/src/airflow/settings.py:331:5:</a> SIM103 [*] Return the negated condition directly
- <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/ti_deps/deps/trigger_rule_dep.py#L222'>airflow-core/src/airflow/ti_deps/deps/trigger_rule_dep.py:222:13:</a> SIM103 Return the condition directly
+ <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/ti_deps/deps/trigger_rule_dep.py#L222'>airflow-core/src/airflow/ti_deps/deps/trigger_rule_dep.py:222:13:</a> SIM103 [*] Return the condition directly
- <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/utils/platform.py#L50'>airflow-core/src/airflow/utils/platform.py:50:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/utils/platform.py#L50'>airflow-core/src/airflow/utils/platform.py:50:5:</a> SIM103 [*] Return the condition directly
- <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/utils/sqlalchemy.py#L439'>airflow-core/src/airflow/utils/sqlalchemy.py:439:5:</a> SIM103 Return the condition `db_err_code in ("55P03", 1205, 3572)` directly
+ <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/utils/sqlalchemy.py#L439'>airflow-core/src/airflow/utils/sqlalchemy.py:439:5:</a> SIM103 [*] Return the condition `db_err_code in ("55P03", 1205, 3572)` directly
- <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/utils/strings.py#L37'>airflow-core/src/airflow/utils/strings.py:37:5:</a> SIM103 Return the condition `astring.strip().lower() in TRUE_LIKE_VALUES` directly
+ <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/airflow-core/src/airflow/utils/strings.py#L37'>airflow-core/src/airflow/utils/strings.py:37:5:</a> SIM103 [*] Return the condition `astring.strip().lower() in TRUE_LIKE_VALUES` directly
- <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/dev/breeze/src/airflow_breeze/commands/common_options.py#L507'>dev/breeze/src/airflow_breeze/commands/common_options.py:507:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/apache/airflow/blob/ece5fcd90d3b352527bac865a305f9c5f3c6bae1/dev/breeze/src/airflow_breeze/commands/common_options.py#L507'>dev/breeze/src/airflow_breeze/commands/common_options.py:507:5:</a> SIM103 [*] Return the negated condition directly
... 54 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +10 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/b42060c8808778e86ca9f55bd9cfe74d1124793a/superset/commands/importers/v1/utils.py#L223'>superset/commands/importers/v1/utils.py:223:5:</a> SIM103 Return the condition `path.suffix.lower() in {".yaml", ".yml"}` directly
+ <a href='https://github.com/apache/superset/blob/b42060c8808778e86ca9f55bd9cfe74d1124793a/superset/commands/importers/v1/utils.py#L223'>superset/commands/importers/v1/utils.py:223:5:</a> SIM103 [*] Return the condition `path.suffix.lower() in {".yaml", ".yml"}` directly
- <a href='https://github.com/apache/superset/blob/b42060c8808778e86ca9f55bd9cfe74d1124793a/superset/db_engine_specs/dremio.py#L80'>superset/db_engine_specs/dremio.py:80:9:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/apache/superset/blob/b42060c8808778e86ca9f55bd9cfe74d1124793a/superset/db_engine_specs/dremio.py#L80'>superset/db_engine_specs/dremio.py:80:9:</a> SIM103 [*] Return the negated condition directly
- <a href='https://github.com/apache/superset/blob/b42060c8808778e86ca9f55bd9cfe74d1124793a/superset/extensions/metastore_cache.py#L121'>superset/extensions/metastore_cache.py:121:9:</a> SIM103 Return the condition `bool(entry)` directly
+ <a href='https://github.com/apache/superset/blob/b42060c8808778e86ca9f55bd9cfe74d1124793a/superset/extensions/metastore_cache.py#L121'>superset/extensions/metastore_cache.py:121:9:</a> SIM103 [*] Return the condition `bool(entry)` directly
- <a href='https://github.com/apache/superset/blob/b42060c8808778e86ca9f55bd9cfe74d1124793a/superset/security/manager.py#L585'>superset/security/manager.py:585:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/apache/superset/blob/b42060c8808778e86ca9f55bd9cfe74d1124793a/superset/security/manager.py#L585'>superset/security/manager.py:585:9:</a> SIM103 [*] Return the condition directly
- <a href='https://github.com/apache/superset/blob/b42060c8808778e86ca9f55bd9cfe74d1124793a/superset/themes/utils.py#L85'>superset/themes/utils.py:85:9:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/apache/superset/blob/b42060c8808778e86ca9f55bd9cfe74d1124793a/superset/themes/utils.py#L85'>superset/themes/utils.py:85:9:</a> SIM103 [*] Return the negated condition directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+0 -0 violations, +18 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/crazy_utils.py#L176'>crazy_functions/crazy_utils.py:176:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/crazy_utils.py#L176'>crazy_functions/crazy_utils.py:176:9:</a> SIM103 [*] Return the condition directly
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/crazy_utils.py#L506'>crazy_functions/crazy_utils.py:506:17:</a> SIM103 Return the condition `bool(match)` directly
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/crazy_utils.py#L506'>crazy_functions/crazy_utils.py:506:17:</a> SIM103 [*] Return the condition `bool(match)` directly
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/doc_fns/content_folder.py#L81'>crazy_functions/doc_fns/content_folder.py:81:13:</a> SIM103 Return the condition `not self.size < 0` directly
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/doc_fns/content_folder.py#L81'>crazy_functions/doc_fns/content_folder.py:81:13:</a> SIM103 [*] Return the condition `not self.size < 0` directly
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/paper_fns/document_structure_extractor.py#L379'>crazy_functions/paper_fns/document_structure_extractor.py:379:13:</a> SIM103 Return the condition directly
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/crazy_functions/paper_fns/document_structure_extractor.py#L379'>crazy_functions/paper_fns/document_structure_extractor.py:379:13:</a> SIM103 [*] Return the condition directly
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/request_llms/bridge_spark.py#L12'>request_llms/bridge_spark.py:12:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/request_llms/bridge_spark.py#L12'>request_llms/bridge_spark.py:12:5:</a> SIM103 [*] Return the negated condition directly
... 8 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/server_auth/auth.py#L40'>examples/server/app/server_auth/auth.py:40:9:</a> SIM103 Return the condition `bool(username == "bokeh" and password == "bokeh")` directly
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/server_auth/auth.py#L40'>examples/server/app/server_auth/auth.py:40:9:</a> SIM103 [*] Return the condition `bool(username == "bokeh" and password == "bokeh")` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/277eff6188e9cd65e02988c449d3ec5bb4134648/src/latch/types/utils.py#L21'>src/latch/types/utils.py:21:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/latchbio/latch/blob/277eff6188e9cd65e02988c449d3ec5bb4134648/src/latch/types/utils.py#L21'>src/latch/types/utils.py:21:5:</a> SIM103 [*] Return the negated condition directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +74 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/corporate/lib/stripe.py#L1032'>corporate/lib/stripe.py:1032:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/corporate/lib/stripe.py#L1032'>corporate/lib/stripe.py:1032:9:</a> SIM103 [*] Return the condition directly
- <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/corporate/lib/stripe.py#L4257'>corporate/lib/stripe.py:4257:9:</a> SIM103 Return the condition `plan_tier in implemented_plan_tiers` directly
+ <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/corporate/lib/stripe.py#L4257'>corporate/lib/stripe.py:4257:9:</a> SIM103 [*] Return the condition `plan_tier in implemented_plan_tiers` directly
- <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/corporate/lib/stripe.py#L4673'>corporate/lib/stripe.py:4673:9:</a> SIM103 Return the condition `plan_tier in implemented_plan_tiers` directly
+ <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/corporate/lib/stripe.py#L4673'>corporate/lib/stripe.py:4673:9:</a> SIM103 [*] Return the condition `plan_tier in implemented_plan_tiers` directly
- <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/corporate/lib/stripe.py#L5119'>corporate/lib/stripe.py:5119:9:</a> SIM103 Return the condition `plan_tier in implemented_plan_tiers` directly
+ <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/corporate/lib/stripe.py#L5119'>corporate/lib/stripe.py:5119:9:</a> SIM103 [*] Return the condition `plan_tier in implemented_plan_tiers` directly
- <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/scripts/lib/zulip_tools.py#L151'>scripts/lib/zulip_tools.py:151:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/scripts/lib/zulip_tools.py#L151'>scripts/lib/zulip_tools.py:151:5:</a> SIM103 [*] Return the condition directly
- <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/scripts/lib/zulip_tools.py#L549'>scripts/lib/zulip_tools.py:549:5:</a> SIM103 Return the condition `bool("posix" in os.name and os.geteuid() == 0)` directly
+ <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/scripts/lib/zulip_tools.py#L549'>scripts/lib/zulip_tools.py:549:5:</a> SIM103 [*] Return the condition `bool("posix" in os.name and os.geteuid() == 0)` directly
- <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/tools/lib/provision_inner.py#L110'>tools/lib/provision_inner.py:110:5:</a> SIM103 Return the condition `"microsoft" in result.stdout.lower()` directly
+ <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/tools/lib/provision_inner.py#L110'>tools/lib/provision_inner.py:110:5:</a> SIM103 [*] Return the condition `"microsoft" in result.stdout.lower()` directly
- <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/zerver/lib/channel_folders.py#L89'>zerver/lib/channel_folders.py:89:5:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/zerver/lib/channel_folders.py#L89'>zerver/lib/channel_folders.py:89:5:</a> SIM103 [*] Return the condition directly
- <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/zerver/lib/compatibility.py#L156'>zerver/lib/compatibility.py:156:5:</a> SIM103 Return the negated condition directly
+ <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/zerver/lib/compatibility.py#L156'>zerver/lib/compatibility.py:156:5:</a> SIM103 [*] Return the negated condition directly
- <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/zerver/lib/markdown/__init__.py#L1968'>zerver/lib/markdown/__init__.py:1968:9:</a> SIM103 Return the condition directly
+ <a href='https://github.com/zulip/zulip/blob/57b7ada2e4446afa817fe3bc0d76b99100cffd47/zerver/lib/markdown/__init__.py#L1968'>zerver/lib/markdown/__init__.py:1968:9:</a> SIM103 [*] Return the condition directly
... 54 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM103 | 180 | 0 | 0 | 180 | 0 |

</p>
</details>




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/needless_bool.rs`:342 on 2025-09-19 13:46_

Should we make this a guard like in the original code? That should allow us to combine the `_ => ` cases on lines 370 and 382 again.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/needless_bool.rs`:326 on 2025-09-19 13:49_

I think we could make this a guard too:


```suggestion
            Expr::Compare(ast::ExprCompare { ops, .. }) if is_identity_or_membership_ops(ops.as_ref()) => {
                (Some((**operand).clone()), Applicability::Safe)
```

and fall through to the unsafe case below if the condition is false.

---

_@ntBre reviewed on 2025-09-19 14:04_

Thank you! This looks good to me with only a couple of small nits on the code.

However, I think we need to make this a preview change because SIM103 is a stable rule. We can only demote fix safety in a patch release, not upgrade unsafe to safe. You can see [`preview.rs`](https://github.com/astral-sh/ruff/blob/c0fb235a703684bca1cc1767015bb7e19ab1c22f/crates/ruff_linter/src/preview.rs#L16-L19) for some examples of preview functions.


We'll need to update both the implementation and the docs to reflect the preview behavior. I like your new docs section, so I would probably recommend leaving the old one and just adding your new paragraphs, starting with `In preview, ...`.


---

_Comment by @dscorbett on 2025-10-03 22:32_

I now think that the fix should stay unsafe when it inserts a `bool` call. Although it preserves the original behavior, it is often unnecessary and is probably not wanted. Keeping it unsafe encourages manual review. For example, this:
```python
def is_odd_or_zero(x):
    if x % 2 != 0 or x == 0:
        return True
    return False
```
is fixed to this:
```python
def is_odd_or_zero(x):
    return bool(x % 2 != 0 or x == 0)
```
but what was almost certainly intended was this:
```python
def is_odd_or_zero(x):
    return x % 2 != 0 or x == 0
```


---

_Comment by @MichaReiser on 2025-11-21 07:59_

@TaKO8Ki sorry for another ping. Do you think you'll have time in the near future to look into the PR feedback? If not, that's completely fine, I'm just trying to understand the status of this PR

---

```yaml
number: 12051
title: "[Ruff v0.5] Stabilise 15 pylint rules"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: ruff-0.5
head: pylint-stabilisations
created_at: 2024-06-26T15:39:47Z
updated_at: 2024-06-27T10:24:47Z
url: https://github.com/astral-sh/ruff/pull/12051
synced_at: 2026-01-10T21:56:00Z
```

# [Ruff v0.5] Stabilise 15 pylint rules

---

_Pull request opened by @AlexWaygood on 2024-06-26 15:39_

## Summary

Stabilise the following rules:
- `bad-open-mode` (`PLW1501`)
- `empty-comment` (`PLR2044`)
- `global-at-module-level` (`PLW0604`)
- ~~`literal-membership` (`PLR6201`)~~
- `misplaced-bare-raise` (`PLE0704`)
- ~~`nan-comparison` (`PLW0177`)~~
- `non-ascii-import-name` (`PLC2403`)
- `non-ascii-name` (`PLC2401`)
- `nonlocal-and-global` (`PLE0115`)
- `potential-index-error` (`PLE0643`)
- `redeclared-assigned-name` (`PLW0128`)
- `redefined-argument-from-locals` (`PLR1704`)
- `repeated-keyword-argument` (`PLE1132`)
- `super-without-brackets` (`PLW0245`)
- `unnecessary-list-index-lookup` (`PLR1736`)
- `useless-exception-statement` (`PLW0133`)
- `useless-with-lock` (`PLW2101`)

These rules have all been in preview for over 3 months; there are no open issues about them, and there haven't been issues about them for months; and their recommendations seem pretty sane and uncontroversial. For some of them, the docs or messages to the user were slightly suboptimal; I've made a few touch-ups as part of this PR.

Many other pylint rules have somewhat opinionated or controversial changes; this PR deliberately leaves quite a few pylint rules still in `preview`.

## Test Plan

`cargo test`, but the ecosystem check will be the real judge...


---

_Label `rule` added by @AlexWaygood on 2024-06-26 15:39_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-26 15:39_

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-06-26 15:39_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-06-26 15:39_

---

_@charliermarsh approved on 2024-06-26 15:40_

---

_Comment by @github-actions[bot] on 2024-06-26 15:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+66 -4 violations, +0 -0 fixes in 11 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/42634f8262a6fdb8eacdef16a064b5839bba7485/disnake/state.py#L456'>disnake/state.py:456:13:</a> PLW0133 Missing `raise` statement on exception
+ <a href='https://github.com/DisnakeDev/disnake/blob/42634f8262a6fdb8eacdef16a064b5839bba7485/disnake/state.py#L476'>disnake/state.py:476:13:</a> PLW0133 Missing `raise` statement on exception
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/15eaa4320dc8fb1adaa6ee2b2879aa0ec711769f/src/plasmapy/analysis/nullpoint.py#L29'>src/plasmapy/analysis/nullpoint.py:29:1:</a> PLW0604 `global` at module level is redundant
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/15eaa4320dc8fb1adaa6ee2b2879aa0ec711769f/src/plasmapy/dispersion/analytical/two_fluid_.py#L316'>src/plasmapy/dispersion/analytical/two_fluid_.py:316:9:</a> PLC2401 Variable name `ω` contains a non-ASCII character
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/15eaa4320dc8fb1adaa6ee2b2879aa0ec711769f/src/plasmapy/dispersion/numerical/kinetic_alfven_.py#L234'>src/plasmapy/dispersion/numerical/kinetic_alfven_.py:234:9:</a> PLC2401 Variable name `θ` contains a non-ASCII character
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/15eaa4320dc8fb1adaa6ee2b2879aa0ec711769f/src/plasmapy/formulary/radiation.py#L123'>src/plasmapy/formulary/radiation.py:123:5:</a> PLC2401 Variable name `ω` contains a non-ASCII character
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/15eaa4320dc8fb1adaa6ee2b2879aa0ec711769f/src/plasmapy/formulary/radiation.py#L124'>src/plasmapy/formulary/radiation.py:124:5:</a> PLC2401 Variable name `ω_pe` contains a non-ASCII character
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/15eaa4320dc8fb1adaa6ee2b2879aa0ec711769f/src/plasmapy/formulary/relativity.py#L187'>src/plasmapy/formulary/relativity.py:187:5:</a> PLC2401 Variable name `γ` contains a non-ASCII character
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/15eaa4320dc8fb1adaa6ee2b2879aa0ec711769f/src/plasmapy/formulary/relativity.py#L496'>src/plasmapy/formulary/relativity.py:496:30:</a> PLC2401 Argument name `γ` contains a non-ASCII character
... 4 additional changes omitted for rule PLC2401
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/15eaa4320dc8fb1adaa6ee2b2879aa0ec711769f/tests/dispersion/test_dispersion_functions.py#L10'>tests/dispersion/test_dispersion_functions.py:10:25:</a> PLC2403 Module alias `π` contains a non-ASCII character
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/15eaa4320dc8fb1adaa6ee2b2879aa0ec711769f/tests/dispersion/test_dispersion_functions.py#L11'>tests/dispersion/test_dispersion_functions.py:11:36:</a> PLC2403 Module alias `Γ` contains a non-ASCII character
... 3 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+30 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/cli/commands/task_command.py#L702'>airflow/cli/commands/task_command.py:702:38:</a> PLR1704 Redefining argument with the local name `session`
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/decorators/base.py#L157'>airflow/decorators/base.py:157:13:</a> PLR1704 Redefining argument with the local name `task_id`
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/commands/ci_image_commands.py#L444'>dev/breeze/src/airflow_breeze/commands/ci_image_commands.py:444:17:</a> PLR1704 Redefining argument with the local name `platform`
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/commands/ci_image_commands.py#L459'>dev/breeze/src/airflow_breeze/commands/ci_image_commands.py:459:17:</a> PLR1704 Redefining argument with the local name `python`
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/commands/ci_image_commands.py#L646'>dev/breeze/src/airflow_breeze/commands/ci_image_commands.py:646:13:</a> PLR1704 Redefining argument with the local name `python`
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L689'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:689:51:</a> PLR1704 Redefining argument with the local name `python`
... 13 additional changes omitted for rule PLR1704
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/parallel.py#L297'>dev/breeze/src/airflow_breeze/utils/parallel.py:297:57:</a> PLR1736 [*] List index lookup in `enumerate()` loop
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/parallel.py#L299'>dev/breeze/src/airflow_breeze/utils/parallel.py:299:40:</a> PLR1736 [*] List index lookup in `enumerate()` loop
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/docs/exts/docroles.py#L21'>docs/exts/docroles.py:21:1:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/docs/exts/docroles.py#L22'>docs/exts/docroles.py:22:1:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/scripts/ci/pre_commit/check_provider_airflow_compatibility.py#L51'>scripts/ci/pre_commit/check_provider_airflow_compatibility.py:51:20:</a> PLR1736 [*] List index lookup in `enumerate()` loop
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/tests/providers/common/sql/hooks/test_sql.py#L18'>tests/providers/common/sql/hooks/test_sql.py:18:1:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/tests/providers/common/sql/operators/test_sql.py#L268'>tests/providers/common/sql/operators/test_sql.py:268:20:</a> PLW0128 Redeclared variable `operator` in assignment
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/tests/providers/databricks/hooks/test_databricks_sql.py#L18'>tests/providers/databricks/hooks/test_databricks_sql.py:18:1:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/tests/providers/exasol/hooks/test_sql.py#L18'>tests/providers/exasol/hooks/test_sql.py:18:1:</a> PLR2044 [*] Line with empty comment
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/tests/providers/google/suite/transfers/test_gcs_to_gdrive.py#L123'>tests/providers/google/suite/transfers/test_gcs_to_gdrive.py:123:5:</a> PLR2044 [*] Line with empty comment
... 3 additional changes omitted for rule PLR2044
... 14 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/ece1945f83d839796e88e4915e92be823c7d68c2/samcli/local/docker/lambda_container.py#L157'>samcli/local/docker/lambda_container.py:157:9:</a> PLR2044 [*] Line with empty comment
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/document/callbacks.py#L361'>src/bokeh/document/callbacks.py:361:13:</a> PLR1704 Redefining argument with the local name `callback_obj`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L387'>src/bokeh/plotting/_figure.py:387:13:</a> PLR1704 Redefining argument with the local name `kw`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L428'>src/bokeh/plotting/_figure.py:428:13:</a> PLR1704 Redefining argument with the local name `kw`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L478'>src/bokeh/plotting/_figure.py:478:17:</a> PLR1704 Redefining argument with the local name `kw`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L567'>src/bokeh/plotting/_figure.py:567:13:</a> PLR1704 Redefining argument with the local name `kw`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_figure.py#L609'>src/bokeh/plotting/_figure.py:609:13:</a> PLR1704 Redefining argument with the local name `kw`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/examples.py#L161'>tests/support/util/examples.py:161:9:</a> PLR1704 Redefining argument with the local name `path`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/7660b1ae5a0061663620321a74916d8bb1b2b24b/ibis/__init__.py#L126'>ibis/__init__.py:126:9:</a> PLR1704 Redefining argument with the local name `name`
+ <a href='https://github.com/ibis-project/ibis/blob/7660b1ae5a0061663620321a74916d8bb1b2b24b/ibis/backends/dask/helpers.py#L116'>ibis/backends/dask/helpers.py:116:13:</a> PLR1704 Redefining argument with the local name `name`
+ <a href='https://github.com/ibis-project/ibis/blob/7660b1ae5a0061663620321a74916d8bb1b2b24b/ibis/backends/sql/rewrites.py#L258'>ibis/backends/sql/rewrites.py:258:9:</a> PLR1704 Redefining argument with the local name `node`
+ <a href='https://github.com/ibis-project/ibis/blob/7660b1ae5a0061663620321a74916d8bb1b2b24b/ibis/expr/types/joins.py#L372'>ibis/expr/types/joins.py:372:13:</a> PLR1704 Redefining argument with the local name `right`
+ <a href='https://github.com/ibis-project/ibis/blob/7660b1ae5a0061663620321a74916d8bb1b2b24b/ibis/expr/types/relations.py#L1884'>ibis/expr/types/relations.py:1884:13:</a> PLR1704 Redefining argument with the local name `table`
+ <a href='https://github.com/ibis-project/ibis/blob/7660b1ae5a0061663620321a74916d8bb1b2b24b/ibis/tests/expr/test_struct.py#L75'>ibis/tests/expr/test_struct.py:75:32:</a> PLR1704 Redefining argument with the local name `t`
+ <a href='https://github.com/ibis-project/ibis/blob/7660b1ae5a0061663620321a74916d8bb1b2b24b/ibis/tests/expr/test_struct.py#L75'>ibis/tests/expr/test_struct.py:75:35:</a> PLR1704 Redefining argument with the local name `s`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/0318663b84e1efe19d22b356ff067df6ee4fd61c/pymilvus/client/types.py#L136'>pymilvus/client/types.py:136:5:</a> PLR2044 [*] Line with empty comment
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pypa/pip/blob/006026fe03cd2d09f0eb0a5aa31ffb2a3ad975dc/src/pip/_internal/utils/misc.py#L152'>src/pip/_internal/utils/misc.py:152:5:</a> PLE0704 Bare `raise` statement is not inside an exception handler
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/c31cebbf68a93927d41e9947427c2dd4d46503e3/zerver/lib/scim.py#L232'>zerver/lib/scim.py:232:13:</a> PLR1704 Redefining argument with the local name `path`
+ <a href='https://github.com/zulip/zulip/blob/c31cebbf68a93927d41e9947427c2dd4d46503e3/zerver/webhooks/clubhouse/view.py#L178'>zerver/webhooks/clubhouse/view.py:178:9:</a> PLR1704 Redefining argument with the local name `action`
+ <a href='https://github.com/zulip/zulip/blob/c31cebbf68a93927d41e9947427c2dd4d46503e3/zerver/webhooks/clubhouse/view.py#L467'>zerver/webhooks/clubhouse/view.py:467:13:</a> PLR1704 Redefining argument with the local name `action`
+ <a href='https://github.com/zulip/zulip/blob/c31cebbf68a93927d41e9947427c2dd4d46503e3/zerver/webhooks/clubhouse/view.py#L669'>zerver/webhooks/clubhouse/view.py:669:13:</a> PLR1704 Redefining argument with the local name `action`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/indico/indico/blob/1765904436c20241561602053d8cf137ec64ce6e/indico/core/db/sqlalchemy/core.py#L53'>indico/core/db/sqlalchemy/core.py:53:16:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `PLE0704`)
- <a href='https://github.com/indico/indico/blob/1765904436c20241561602053d8cf137ec64ce6e/indico/web/flask/errors.py#L112'>indico/web/flask/errors.py:112:16:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `PLE0704`)
- <a href='https://github.com/indico/indico/blob/1765904436c20241561602053d8cf137ec64ce6e/indico/web/flask/errors.py#L126'>indico/web/flask/errors.py:126:12:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `PLE0704`)
- <a href='https://github.com/indico/indico/blob/1765904436c20241561602053d8cf137ec64ce6e/indico/web/flask/errors.py#L74'>indico/web/flask/errors.py:74:16:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `PLE0704`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/f74e947c1fdfef238235b7dd18c8fe52108268f2/src/_pytest/logging.py#L402'>src/_pytest/logging.py:402:13:</a> PLE0704 Bare `raise` statement is not inside an exception handler
</pre>

</p>
</details>
<details><summary>Changes by rule (10 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1704 | 36 | 36 | 0 | 0 | 0 |
| PLR2044 | 10 | 10 | 0 | 0 | 0 |
| PLC2401 | 9 | 9 | 0 | 0 | 0 |
| RUF100 | 4 | 0 | 4 | 0 | 0 |
| PLR1736 | 3 | 3 | 0 | 0 | 0 |
| PLW0133 | 2 | 2 | 0 | 0 | 0 |
| PLC2403 | 2 | 2 | 0 | 0 | 0 |
| PLE0704 | 2 | 2 | 0 | 0 | 0 |
| PLW0604 | 1 | 1 | 0 | 0 | 0 |
| PLW0128 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+852 -852 violations, +0 -0 fixes in 12 projects; 1 project error; 37 projects unchanged)

<details><summary><a href="https://github.com/aiven/aiven-client">aiven/aiven-client</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aiven/aiven-client/blob/ce9cfd5f992087a61e1ac48031eceee8eacda5ae/aiven/client/argx.py#L111'>aiven/client/argx.py:111:57:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/aiven/aiven-client/blob/ce9cfd5f992087a61e1ac48031eceee8eacda5ae/aiven/client/argx.py#L111'>aiven/client/argx.py:111:57:</a> PLR6201 Use a set literal when testing for membership
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+67 -67 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/PlasmaPy/PlasmaPy/blob/15eaa4320dc8fb1adaa6ee2b2879aa0ec711769f/src/plasmapy/dispersion/analytical/mhd_waves_.py#L121'>src/plasmapy/dispersion/analytical/mhd_waves_.py:121:26:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/15eaa4320dc8fb1adaa6ee2b2879aa0ec711769f/src/plasmapy/dispersion/analytical/mhd_waves_.py#L121'>src/plasmapy/dispersion/analytical/mhd_waves_.py:121:26:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/PlasmaPy/PlasmaPy/blob/15eaa4320dc8fb1adaa6ee2b2879aa0ec711769f/src/plasmapy/dispersion/analytical/mhd_waves_.py#L131'>src/plasmapy/dispersion/analytical/mhd_waves_.py:131:30:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/15eaa4320dc8fb1adaa6ee2b2879aa0ec711769f/src/plasmapy/dispersion/analytical/mhd_waves_.py#L131'>src/plasmapy/dispersion/analytical/mhd_waves_.py:131:30:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/PlasmaPy/PlasmaPy/blob/15eaa4320dc8fb1adaa6ee2b2879aa0ec711769f/src/plasmapy/dispersion/analytical/stix_.py#L192'>src/plasmapy/dispersion/analytical/stix_.py:192:24:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/15eaa4320dc8fb1adaa6ee2b2879aa0ec711769f/src/plasmapy/dispersion/analytical/stix_.py#L192'>src/plasmapy/dispersion/analytical/stix_.py:192:24:</a> PLR6201 Use a set literal when testing for membership
... 107 additional changes omitted for rule PLR6201
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/15eaa4320dc8fb1adaa6ee2b2879aa0ec711769f/src/plasmapy/dispersion/analytical/two_fluid_.py#L316'>src/plasmapy/dispersion/analytical/two_fluid_.py:316:9:</a> PLC2401 Variable name `ω` contains a non-ASCII character
- <a href='https://github.com/PlasmaPy/PlasmaPy/blob/15eaa4320dc8fb1adaa6ee2b2879aa0ec711769f/src/plasmapy/dispersion/analytical/two_fluid_.py#L316'>src/plasmapy/dispersion/analytical/two_fluid_.py:316:9:</a> PLC2401 Variable name `ω` contains a non-ASCII character, consider renaming it
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/15eaa4320dc8fb1adaa6ee2b2879aa0ec711769f/src/plasmapy/dispersion/numerical/kinetic_alfven_.py#L234'>src/plasmapy/dispersion/numerical/kinetic_alfven_.py:234:9:</a> PLC2401 Variable name `θ` contains a non-ASCII character
- <a href='https://github.com/PlasmaPy/PlasmaPy/blob/15eaa4320dc8fb1adaa6ee2b2879aa0ec711769f/src/plasmapy/dispersion/numerical/kinetic_alfven_.py#L234'>src/plasmapy/dispersion/numerical/kinetic_alfven_.py:234:9:</a> PLC2401 Variable name `θ` contains a non-ASCII character, consider renaming it
... 124 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+327 -327 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/__init__.py#L98'>airflow/__init__.py:98:65:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/__init__.py#L98'>airflow/__init__.py:98:65:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/__main__.py#L49'>airflow/__main__.py:49:31:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/__main__.py#L49'>airflow/__main__.py:49:31:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/__main__.py#L56'>airflow/__main__.py:56:31:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/__main__.py#L56'>airflow/__main__.py:56:31:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/api_connexion/endpoints/task_instance_endpoint.py#L277'>airflow/api_connexion/endpoints/task_instance_endpoint.py:277:26:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/api_connexion/endpoints/task_instance_endpoint.py#L277'>airflow/api_connexion/endpoints/task_instance_endpoint.py:277:26:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/api_connexion/endpoints/task_instance_endpoint.py#L729'>airflow/api_connexion/endpoints/task_instance_endpoint.py:729:20:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/airflow/api_connexion/endpoints/task_instance_endpoint.py#L729'>airflow/api_connexion/endpoints/task_instance_endpoint.py:729:20:</a> PLR6201 Use a set literal when testing for membership
... 639 additional changes omitted for rule PLR6201
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/parallel.py#L297'>dev/breeze/src/airflow_breeze/utils/parallel.py:297:57:</a> PLR1736 [*] List index lookup in `enumerate()` loop
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/parallel.py#L297'>dev/breeze/src/airflow_breeze/utils/parallel.py:297:57:</a> PLR1736 [*] Unnecessary lookup of list item by index
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/parallel.py#L299'>dev/breeze/src/airflow_breeze/utils/parallel.py:299:40:</a> PLR1736 [*] List index lookup in `enumerate()` loop
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/dev/breeze/src/airflow_breeze/utils/parallel.py#L299'>dev/breeze/src/airflow_breeze/utils/parallel.py:299:40:</a> PLR1736 [*] Unnecessary lookup of list item by index
+ <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/scripts/ci/pre_commit/check_provider_airflow_compatibility.py#L51'>scripts/ci/pre_commit/check_provider_airflow_compatibility.py:51:20:</a> PLR1736 [*] List index lookup in `enumerate()` loop
- <a href='https://github.com/apache/airflow/blob/d0e4b8d95992936d8c89a5224107514392f918fb/scripts/ci/pre_commit/check_provider_airflow_compatibility.py#L51'>scripts/ci/pre_commit/check_provider_airflow_compatibility.py:51:20:</a> PLR1736 [*] Unnecessary lookup of list item by index
... 638 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+42 -42 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/ece1945f83d839796e88e4915e92be823c7d68c2/samcli/cli/main.py#L89'>samcli/cli/main.py:89:27:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/aws/aws-sam-cli/blob/ece1945f83d839796e88e4915e92be823c7d68c2/samcli/cli/main.py#L89'>samcli/cli/main.py:89:27:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/aws/aws-sam-cli/blob/ece1945f83d839796e88e4915e92be823c7d68c2/samcli/cli/types.py#L58'>samcli/cli/types.py:58:56:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/aws/aws-sam-cli/blob/ece1945f83d839796e88e4915e92be823c7d68c2/samcli/cli/types.py#L58'>samcli/cli/types.py:58:56:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/aws/aws-sam-cli/blob/ece1945f83d839796e88e4915e92be823c7d68c2/samcli/cli/types.py#L58'>samcli/cli/types.py:58:84:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/aws/aws-sam-cli/blob/ece1945f83d839796e88e4915e92be823c7d68c2/samcli/cli/types.py#L58'>samcli/cli/types.py:58:84:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/aws/aws-sam-cli/blob/ece1945f83d839796e88e4915e92be823c7d68c2/samcli/commands/_utils/custom_options/hook_name_option.py#L46'>samcli/commands/_utils/custom_options/hook_name_option.py:46:28:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/aws/aws-sam-cli/blob/ece1945f83d839796e88e4915e92be823c7d68c2/samcli/commands/_utils/custom_options/hook_name_option.py#L46'>samcli/commands/_utils/custom_options/hook_name_option.py:46:28:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/aws/aws-sam-cli/blob/ece1945f83d839796e88e4915e92be823c7d68c2/samcli/commands/_utils/template.py#L162'>samcli/commands/_utils/template.py:162:34:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/aws/aws-sam-cli/blob/ece1945f83d839796e88e4915e92be823c7d68c2/samcli/commands/_utils/template.py#L162'>samcli/commands/_utils/template.py:162:34:</a> PLR6201 Use a set literal when testing for membership
... 74 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+28 -28 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/file_html.py#L39'>examples/output/apis/file_html.py:39:48:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/file_html.py#L39'>examples/output/apis/file_html.py:39:48:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/sprint.py#L23'>examples/plotting/sprint.py:23:48:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/plotting/sprint.py#L23'>examples/plotting/sprint.py:23:48:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/checks.py#L69'>release/checks.py:69:30:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/checks.py#L69'>release/checks.py:69:30:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L104'>src/bokeh/__init__.py:104:24:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L104'>src/bokeh/__init__.py:104:24:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/__init__.py#L56'>src/bokeh/command/subcommands/__init__.py:56:48:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/__init__.py#L56'>src/bokeh/command/subcommands/__init__.py:56:48:</a> PLR6201 Use a set literal when testing for membership
... 46 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+40 -40 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/devops/scripts/verify-mo.py#L140'>devops/scripts/verify-mo.py:140:35:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/devops/scripts/verify-mo.py#L140'>devops/scripts/verify-mo.py:140:35:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/molecule/ansible-config/tests/test_play_configuration.py#L34'>molecule/ansible-config/tests/test_play_configuration.py:34:25:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/molecule/ansible-config/tests/test_play_configuration.py#L34'>molecule/ansible-config/tests/test_play_configuration.py:34:25:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/securedrop/manage.py#L170'>securedrop/manage.py:170:22:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/securedrop/manage.py#L170'>securedrop/manage.py:170:22:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/securedrop/manage.py#L172'>securedrop/manage.py:172:24:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/securedrop/manage.py#L172'>securedrop/manage.py:172:24:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/securedrop/pretty_bad_protocol/_parsers.py#L1057'>securedrop/pretty_bad_protocol/_parsers.py:1057:25:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/freedomofpress/securedrop/blob/22ff5276582c0c415a8ae9bf817609a6e1c5ffd5/securedrop/pretty_bad_protocol/_parsers.py#L1057'>securedrop/pretty_bad_protocol/_parsers.py:1057:25:</a> PLR6201 Use a set literal when testing for membership
... 70 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/fronzbot/blinkpy/blob/2e48b0c8fb881c20a18f0c399f46d07f26a497be/blinkpy/auth.py#L150'>blinkpy/auth.py:150:35:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/fronzbot/blinkpy/blob/2e48b0c8fb881c20a18f0c399f46d07f26a497be/blinkpy/auth.py#L150'>blinkpy/auth.py:150:35:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/fronzbot/blinkpy/blob/2e48b0c8fb881c20a18f0c399f46d07f26a497be/blinkpy/camera.py#L144'>blinkpy/camera.py:144:41:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/fronzbot/blinkpy/blob/2e48b0c8fb881c20a18f0c399f46d07f26a497be/blinkpy/camera.py#L144'>blinkpy/camera.py:144:41:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/fronzbot/blinkpy/blob/2e48b0c8fb881c20a18f0c399f46d07f26a497be/blinkpy/camera.py#L157'>blinkpy/camera.py:157:25:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/fronzbot/blinkpy/blob/2e48b0c8fb881c20a18f0c399f46d07f26a497be/blinkpy/camera.py#L157'>blinkpy/camera.py:157:25:</a> PLR6201 Use a set literal when testing for membership
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+61 -61 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/7660b1ae5a0061663620321a74916d8bb1b2b24b/ibis/backends/__init__.py#L1378'>ibis/backends/__init__.py:1378:18:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/ibis-project/ibis/blob/7660b1ae5a0061663620321a74916d8bb1b2b24b/ibis/backends/__init__.py#L1378'>ibis/backends/__init__.py:1378:18:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/ibis-project/ibis/blob/7660b1ae5a0061663620321a74916d8bb1b2b24b/ibis/backends/bigquery/compiler.py#L609'>ibis/backends/bigquery/compiler.py:609:23:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/ibis-project/ibis/blob/7660b1ae5a0061663620321a74916d8bb1b2b24b/ibis/backends/bigquery/compiler.py#L609'>ibis/backends/bigquery/compiler.py:609:23:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/ibis-project/ibis/blob/7660b1ae5a0061663620321a74916d8bb1b2b24b/ibis/backends/bigquery/compiler.py#L635'>ibis/backends/bigquery/compiler.py:635:23:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/ibis-project/ibis/blob/7660b1ae5a0061663620321a74916d8bb1b2b24b/ibis/backends/bigquery/compiler.py#L635'>ibis/backends/bigquery/compiler.py:635:23:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/ibis-project/ibis/blob/7660b1ae5a0061663620321a74916d8bb1b2b24b/ibis/backends/bigquery/tests/system/test_client.py#L308'>ibis/backends/bigquery/tests/system/test_client.py:308:22:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/ibis-project/ibis/blob/7660b1ae5a0061663620321a74916d8bb1b2b24b/ibis/backends/bigquery/tests/system/test_client.py#L308'>ibis/backends/bigquery/tests/system/test_client.py:308:22:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/ibis-project/ibis/blob/7660b1ae5a0061663620321a74916d8bb1b2b24b/ibis/backends/clickhouse/compiler.py#L269'>ibis/backends/clickhouse/compiler.py:269:32:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/ibis-project/ibis/blob/7660b1ae5a0061663620321a74916d8bb1b2b24b/ibis/backends/clickhouse/compiler.py#L269'>ibis/backends/clickhouse/compiler.py:269:32:</a> PLR6201 Use a set literal when testing for membership
... 112 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+42 -42 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/0318663b84e1efe19d22b356ff067df6ee4fd61c/pymilvus/bulk_writer/remote_bulk_writer.py#L282'>pymilvus/bulk_writer/remote_bulk_writer.py:282:31:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/milvus-io/pymilvus/blob/0318663b84e1efe19d22b356ff067df6ee4fd61c/pymilvus/bulk_writer/remote_bulk_writer.py#L282'>pymilvus/bulk_writer/remote_bulk_writer.py:282:31:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/milvus-io/pymilvus/blob/0318663b84e1efe19d22b356ff067df6ee4fd61c/pymilvus/client/abstract.py#L443'>pymilvus/client/abstract.py:443:25:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/milvus-io/pymilvus/blob/0318663b84e1efe19d22b356ff067df6ee4fd61c/pymilvus/client/abstract.py#L443'>pymilvus/client/abstract.py:443:25:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/milvus-io/pymilvus/blob/0318663b84e1efe19d22b356ff067df6ee4fd61c/pymilvus/client/abstract.py#L553'>pymilvus/client/abstract.py:553:39:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/milvus-io/pymilvus/blob/0318663b84e1efe19d22b356ff067df6ee4fd61c/pymilvus/client/abstract.py#L553'>pymilvus/client/abstract.py:553:39:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/milvus-io/pymilvus/blob/0318663b84e1efe19d22b356ff067df6ee4fd61c/pymilvus/client/abstract.py#L560'>pymilvus/client/abstract.py:560:43:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/milvus-io/pymilvus/blob/0318663b84e1efe19d22b356ff067df6ee4fd61c/pymilvus/client/abstract.py#L560'>pymilvus/client/abstract.py:560:43:</a> PLR6201 Use a set literal when testing for membership
- <a href='https://github.com/milvus-io/pymilvus/blob/0318663b84e1efe19d22b356ff067df6ee4fd61c/pymilvus/client/abstract.py#L562'>pymilvus/client/abstract.py:562:45:</a> PLR6201 Use a `set` literal when testing for membership
+ <a href='https://github.com/milvus-io/pymilvus/blob/0318663b84e1efe19d22b356ff067df6ee4fd61c/pymilvus/client/abstract.py#L562'>pymilvus/client/abstract.py:562:45:</a> PLR6201 Use a set literal when testing for membership
... 74 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'
warning: `PGH001` has been remapped to `S307`.
warning: `PGH002` has been remapped to `G010`.
warning: `PLR1701` has been remapped to `SIM101`.
ruff failed
  Cause: Selection of deprecated rule `E999` is not allowed when preview is enabled.
```

</p>
</details>
<details><summary>Changes by rule (4 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR6201 | 1676 | 838 | 838 | 0 | 0 |
| PLC2401 | 18 | 9 | 9 | 0 | 0 |
| PLR1736 | 6 | 3 | 3 | 0 | 0 |
| PLC2403 | 4 | 2 | 2 | 0 | 0 |

</p>
</details>




---

_Renamed from "Stabilise 17 pylint rules" to "[Ruff v0.5] Stabilise 17 pylint rules" by @AlexWaygood on 2024-06-26 16:03_

---

_Comment by @airreality on 2024-06-27 06:34_

Are you sure that PLR6201 is really useful rule? I believe initializing set for only one operation is too expensive and has no sense. I would prefer using tuple. And if the reason is to avoid duplicating items maybe it's better to have the rule which checks such items instead?
I see there are a lot of violations in ecosystem. The same in my repoes. I guess most of users will add this rule in ignores.

---

_Comment by @AlexWaygood on 2024-06-27 09:53_

Thanks @airreality. While I think it's a useful rule, and probably one that I would enable on my own projects, I agree that there's a lot of ecosystem hits for PLR6201. The performance pros and cons _have_ already been discussed in some depth in https://github.com/astral-sh/ruff/issues/8758 and https://github.com/astral-sh/ruff/issues/9604; however, I missed that https://github.com/astral-sh/ruff/issues/8322 regarding this rule is still open. I think it'd be inappropriate to stabilise a rule while there's still open issues about its behaviour, so I'll axe that rule from the list.

I'm also slightly concerned about the number of hits for `nan-comparison` (PLW0177) on `pandas`, as that's a project I'd expect to follow best practices for this kind of thing. In the interests of getting the release out on schedule, I'll also axe that from the list of stabilisations for now, and I'll do an investigation later to see if those are false positives or true positives.

---

_Renamed from "[Ruff v0.5] Stabilise 17 pylint rules" to "[Ruff v0.5] Stabilise 15 pylint rules" by @AlexWaygood on 2024-06-27 09:55_

---

_@MichaReiser approved on 2024-06-27 09:55_

---

_@dhruvmanila approved on 2024-06-27 10:07_

---

_Merged by @AlexWaygood on 2024-06-27 10:24_

---

_Closed by @AlexWaygood on 2024-06-27 10:24_

---

_Branch deleted on 2024-06-27 10:24_

---

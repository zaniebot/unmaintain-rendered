```yaml
number: 15951
title: "[`flake8-builtins`] Make strict module name comparison optional (`A005`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - configuration
  - preview
assignees: []
merged: true
base: main
head: brent/a005
created_at: 2025-02-04T22:22:25Z
updated_at: 2025-02-11T13:20:50Z
url: https://github.com/astral-sh/ruff/pull/15951
synced_at: 2026-01-10T19:57:22Z
```

# [`flake8-builtins`] Make strict module name comparison optional (`A005`)

---

_Pull request opened by @ntBre on 2025-02-04 22:22_

## Summary

This PR adds the configuration option `lint.flake8-builtins.builtins-strict-checking`, which is used in A005 to determine whether the fully-qualified module name (relative to the project root or source directories) should be checked instead of just the final component as is currently the case.

As discussed in https://github.com/astral-sh/ruff/issues/15399#issuecomment-2587017147, the default value of the new option is `false` on preview, so modules like `utils.logging` from the initial report are no longer flagged by default. For non-preview the default is still strict checking.

## Test Plan

New A005 test module with the structure reported in #15399.

Fixes #15399 

---

_@ntBre reviewed on 2025-02-04 22:28_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:101 on 2025-02-04 22:28_

Before the last commit, I was doing more processing on the input `path` to build a fully-qualified module name below the package root and `src` directories to pass to `is_known_standard_library`, but then I noticed this and simplified the check to this parent code.

This was inspired by some of the checks in [`implicit_namespace_package.rs`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_no_pep420/rules/implicit_namespace_package.rs#L77), which also checks for shebang lines and PEP 723 scripts. Should we do that there too?

---

_Comment by @github-actions[bot] on 2025-02-04 22:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -105 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+0 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/e23b0a7f31fb88aba08424135af8fdf8185b9f93/src/snowflake/cli/_plugins/cortex/types.py#L1'>src/snowflake/cli/_plugins/cortex/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/e23b0a7f31fb88aba08424135af8fdf8185b9f93/src/snowflake/cli/_plugins/notebook/types.py#L1'>src/snowflake/cli/_plugins/notebook/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/e23b0a7f31fb88aba08424135af8fdf8185b9f93/src/snowflake/cli/api/console/abc.py#L1'>src/snowflake/cli/api/console/abc.py:1:1:</a> A005 Module `abc` shadows a Python standard-library module
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/e23b0a7f31fb88aba08424135af8fdf8185b9f93/src/snowflake/cli/api/console/enum.py#L1'>src/snowflake/cli/api/console/enum.py:1:1:</a> A005 Module `enum` shadows a Python standard-library module
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/e23b0a7f31fb88aba08424135af8fdf8185b9f93/src/snowflake/cli/api/errno.py#L1'>src/snowflake/cli/api/errno.py:1:1:</a> A005 Module `errno` shadows a Python standard-library module
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/e23b0a7f31fb88aba08424135af8fdf8185b9f93/src/snowflake/cli/api/output/types.py#L1'>src/snowflake/cli/api/output/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/e23b0a7f31fb88aba08424135af8fdf8185b9f93/src/snowflake/cli/api/utils/types.py#L1'>src/snowflake/cli/api/utils/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -49 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/airflow/api_connexion/types.py#L1'>airflow/api_connexion/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/airflow/api_fastapi/common/types.py#L1'>airflow/api_fastapi/common/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/airflow/api_fastapi/execution_api/datamodels/token.py#L1'>airflow/api_fastapi/execution_api/datamodels/token.py:1:1:</a> A005 Module `token` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/airflow/api_fastapi/logging/__init__.py#L1'>airflow/api_fastapi/logging/__init__.py:1:1:</a> A005 Module `logging` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/airflow/io/__init__.py#L1'>airflow/io/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/airflow/io/utils/stat.py#L1'>airflow/io/utils/stat.py:1:1:</a> A005 Module `stat` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/airflow/models/operator.py#L1'>airflow/models/operator.py:1:1:</a> A005 Module `operator` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/airflow/operators/email.py#L1'>airflow/operators/email.py:1:1:</a> A005 Module `email` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/airflow/secrets/__init__.py#L1'>airflow/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/airflow/serialization/serializers/datetime.py#L1'>airflow/serialization/serializers/datetime.py:1:1:</a> A005 Module `datetime` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/airflow/utils/email.py#L1'>airflow/utils/email.py:1:1:</a> A005 Module `email` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/airflow/utils/json.py#L1'>airflow/utils/json.py:1:1:</a> A005 Module `json` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/airflow/utils/platform.py#L1'>airflow/utils/platform.py:1:1:</a> A005 Module `platform` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/airflow/utils/types.py#L1'>airflow/utils/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/airflow/utils/warnings.py#L1'>airflow/utils/warnings.py:1:1:</a> A005 Module `warnings` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/operators/resource.py#L1'>providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/operators/resource.py:1:1:</a> A005 Module `resource` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/providers/common/io/src/airflow/providers/common/io/__init__.py#L1'>providers/common/io/src/airflow/providers/common/io/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/providers/common/io/tests/provider_tests/common/io/__init__.py#L1'>providers/common/io/tests/provider_tests/common/io/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/providers/common/io/tests/system/common/io/__init__.py#L1'>providers/common/io/tests/system/common/io/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/providers/fab/src/airflow/providers/fab/www/api_connexion/types.py#L1'>providers/fab/src/airflow/providers/fab/www/api_connexion/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/providers/google/src/airflow/providers/google/cloud/secrets/__init__.py#L1'>providers/google/src/airflow/providers/google/cloud/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/providers/google/src/airflow/providers/google/suite/hooks/calendar.py#L1'>providers/google/src/airflow/providers/google/suite/hooks/calendar.py:1:1:</a> A005 Module `calendar` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/c1d85bd689e4e17d5e64aeca798cf3258737164d/providers/google/tests/provider_tests/google/cloud/secrets/__init__.py#L1'>providers/google/tests/provider_tests/google/cloud/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
... 26 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -15 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/06f8f8e6082371df603571aa3fde8c763ed4f77e/superset/advanced_data_type/types.py#L1'>superset/advanced_data_type/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/06f8f8e6082371df603571aa3fde8c763ed4f77e/superset/commands/dashboard/copy.py#L1'>superset/commands/dashboard/copy.py:1:1:</a> A005 Module `copy` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/06f8f8e6082371df603571aa3fde8c763ed4f77e/superset/dashboards/permalink/types.py#L1'>superset/dashboards/permalink/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/06f8f8e6082371df603571aa3fde8c763ed4f77e/superset/databases/types.py#L1'>superset/databases/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/06f8f8e6082371df603571aa3fde8c763ed4f77e/superset/distributed_lock/types.py#L1'>superset/distributed_lock/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/06f8f8e6082371df603571aa3fde8c763ed4f77e/superset/explore/permalink/types.py#L1'>superset/explore/permalink/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/06f8f8e6082371df603571aa3fde8c763ed4f77e/superset/key_value/types.py#L1'>superset/key_value/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/06f8f8e6082371df603571aa3fde8c763ed4f77e/superset/reports/notifications/email.py#L1'>superset/reports/notifications/email.py:1:1:</a> A005 Module `email` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/06f8f8e6082371df603571aa3fde8c763ed4f77e/superset/reports/types.py#L1'>superset/reports/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/06f8f8e6082371df603571aa3fde8c763ed4f77e/superset/sqllab/permalink/types.py#L1'>superset/sqllab/permalink/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
... 5 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -17 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/widgets/fileinput.py#L1'>examples/interaction/widgets/fileinput.py:1:1:</a> A005 Module `fileinput` shadows a Python standard-library module
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L1'>src/bokeh/application/handlers/code.py:1:1:</a> A005 Module `code` shadows a Python standard-library module
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/json.py#L1'>src/bokeh/command/subcommands/json.py:1:1:</a> A005 Module `json` shadows a Python standard-library module
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/datetime.py#L1'>src/bokeh/core/property/datetime.py:1:1:</a> A005 Module `datetime` shadows a Python standard-library module
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/enum.py#L1'>src/bokeh/core/property/enum.py:1:1:</a> A005 Module `enum` shadows a Python standard-library module
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/json.py#L1'>src/bokeh/core/property/json.py:1:1:</a> A005 Module `json` shadows a Python standard-library module
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/string.py#L1'>src/bokeh/core/property/string.py:1:1:</a> A005 Module `string` shadows a Python standard-library module
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/struct.py#L1'>src/bokeh/core/property/struct.py:1:1:</a> A005 Module `struct` shadows a Python standard-library module
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/types.py#L1'>src/bokeh/core/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/validation/warnings.py#L1'>src/bokeh/core/validation/warnings.py:1:1:</a> A005 Module `warnings` shadows a Python standard-library module
... 7 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -10 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch/functions/secrets.py#L1'>src/latch/functions/secrets.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch/idl/core/types.py#L1'>src/latch/idl/core/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch/registry/types.py#L1'>src/latch/registry/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch/registry/upstream_types/types.py#L1'>src/latch/registry/upstream_types/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch/types/__init__.py#L1'>src/latch/types/__init__.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch/types/glob.py#L1'>src/latch/types/glob.py:1:1:</a> A005 Module `glob` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch/types/json.py#L1'>src/latch/types/json.py:1:1:</a> A005 Module `json` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch_cli/exceptions/traceback.py#L1'>src/latch_cli/exceptions/traceback.py:1:1:</a> A005 Module `traceback` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch_cli/services/cp/glob.py#L1'>src/latch_cli/services/cp/glob.py:1:1:</a> A005 Module `glob` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch_cli/snakemake/config/parser.py#L1'>src/latch_cli/snakemake/config/parser.py:1:1:</a> A005 Module `parser` shadows a Python standard-library module
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/zerver/actions/typing.py#L1'>zerver/actions/typing.py:1:1:</a> A005 Module `typing` shadows a Python standard-library module
- <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/zerver/lib/profile.py#L1'>zerver/lib/profile.py:1:1:</a> A005 Module `profile` shadows a Python standard-library module
- <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/zerver/lib/queue.py#L1'>zerver/lib/queue.py:1:1:</a> A005 Module `queue` shadows a Python standard-library module
- <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/zerver/lib/types.py#L1'>zerver/lib/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/zerver/lib/url_preview/types.py#L1'>zerver/lib/url_preview/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/zerver/views/typing.py#L1'>zerver/views/typing.py:1:1:</a> A005 Module `typing` shadows a Python standard-library module
- <a href='https://github.com/zulip/zulip/blob/d28f7d86223bab4f11629637d4237381943f6fc1/zerver/webhooks/json/__init__.py#L1'>zerver/webhooks/json/__init__.py:1:1:</a> A005 Module `json` shadows a Python standard-library module
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| A005 | 105 | 0 | 105 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @ntBre on 2025-02-04 22:44_

---

_Label `configuration` added by @ntBre on 2025-02-04 22:44_

---

_Comment by @MichaReiser on 2025-02-05 07:26_

Have you considered implementing the approach mentioned in https://github.com/astral-sh/ruff/issues/15399#issuecomment-2612005957 and if so, why did you decide against it?

---

_Comment by @ntBre on 2025-02-05 13:27_

I could certainly be missing something here, but from my reading of the comment, this implementation (with the bugfix I just added) should cover that case. Possibly this should be added as a test, but I just added this directrory structure locally:

```text
.
├── abc
│   └── __init__.py
├── collections
│   ├── __init__.py
│   ├── abc
│   │   └── __init__.py
│   └── foobar
│       └── __init__.py
├── foobar
│   ├── abc
│   │   └── __init__.py
│   └── collections
│       ├── __init__.py
│       ├── abc
│       │   └── __init__.py
│       └── foobar
│           └── __init__.py
├── ruff.toml
└── urlparse
    └── __init__.py
```

And ran my debug build of ruff:

```text
/home/brent/astral/ruff/target/debug/ruff check --no-cache --select A005  .
abc/__init__.py:1:1: A005 Module `abc` shadows a Python standard-library module
collections/__init__.py:1:1: A005 Module `collections` shadows a Python standard-library module
collections/abc/__init__.py:1:1: A005 Module `collections` shadows a Python standard-library module
collections/foobar/__init__.py:1:1: A005 Module `collections` shadows a Python standard-library module
Found 4 errors.
```

If I enable strict checking, I get 4 additional errors for the `foobar` subpackages, which is the behavior I understood from the comment.

Again, I could be missing something, but we don't really have to treat the fully-qualified path or worry about parent-child relationships mentioned in the comment directly because we check each of the files separately.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/filesystem.rs`:55 on 2025-02-05 13:52_

Nit: Should we just pass the entire `Settings` struct instead of picking individual fields?

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:1152 on 2025-02-05 13:55_

I think we have to keep the existing default and wait for the next minor to change it. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:80 on 2025-02-05 14:04_

Nit: the only difference between the two branches seems to be whether we call the operations on `package.path` or `path`. Could we change the `if` to either return `package.path` or `path` and then extract the `module_name` and `parent` outside the `if`?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:101 on 2025-02-05 14:06_

I don't think we should exclude PEP 723 scripts or scripts with shebang lines that are at the top level and have the same name as a standard library module because it doesn't change that they shadow the built-in module. Or what's your reasoning for wanting to ignore them?

---

_@MichaReiser approved on 2025-02-05 14:07_

---

_@ntBre reviewed on 2025-02-05 14:10_

---

_Review comment by @ntBre on `crates/ruff_workspace/src/options.rs`:1152 on 2025-02-05 14:10_

That makes sense, I'll set the default back to `true`. Is there a special label I need to apply here or otherwise track that for the next minor release?

---

_@ntBre reviewed on 2025-02-05 14:12_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:80 on 2025-02-05 14:12_

There's also `file_name` vs `file_stem` to remove the `.py` from the non-module file, but this did look a bit better before I made it a tuple. I'll think about an improvement.

---

_@MichaReiser reviewed on 2025-02-05 14:13_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:80 on 2025-02-05 14:13_

Oh, I missed that

---

_@ntBre reviewed on 2025-02-05 14:14_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:101 on 2025-02-05 14:14_

I thought it was related to this comment https://github.com/astral-sh/ruff/issues/15399#issuecomment-2582929030, but my main reasoning was just that it was handled specially by the other rule. I think I agree with you that it makes sense *not* to do it though.

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:1152 on 2025-02-05 14:14_

We can change the default for preview. Although I'm not sure if we did that before but it would give us some preview testing. 

The easiest for tracking is to create an issue and tag it with the next milestone. Or you can create a follow up PR, mark it as breaking, and add it to the milestone.

---

_@MichaReiser reviewed on 2025-02-05 14:14_

---

_Comment by @ntBre on 2025-02-05 15:32_

I think my example from earlier was actually misleading because I didn't have a `foobar/__init__.py` file. With that file, the existing behavior in ruff is already as described in the issue comment. We get the four errors above and don't error on the `foobar/` subtree. This is because the `package.path()` always seems to return the root package, rather than the leaf package. For example, when `path` is `foobar/abc/__init__.py`, the `package.path()` is still `foobar`.

Related to your `is_module_path` suggestion, I think if we want the strictness option to have any effect, we should actually check `path.file_name()` in both cases and not even use the `package` argument at all.

I noticed this when testing a default strict value and not being able to reproduce the eight-error case.

---

_Comment by @ntBre on 2025-02-05 19:45_

Sorry @MichaReiser, I now believe that you were right all along. I ended up writing a few integration tests with the exact module structure above (including `foobar/__init__.py`), and trying to pass those pushed me back toward the path-component implementation.

I also changed the default for `builtins_strict_checking` to `true`, so I expected the stable ecosystem check to show no changes and the preview check to match the previous report (-88), but apparently I've made the default even more strict than it was before. Is that okay, or should I try to get rid of the new violations? It looks like these are true positives, if our intention is to flag any module with a name from the standard library.

Once the non-strict behavior becomes default, there will still be a substantial net decrease in violations too.

---

_Label `preview` added by @ntBre on 2025-02-05 21:17_

---

_@MichaReiser reviewed on 2025-02-06 08:26_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:79 on 2025-02-06 08:26_

Shouldn't we break after we found the first path or we might end up stripping the same path more than once.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:74 on 2025-02-06 08:28_

This is unfortunate as it means we sort `src` for every file. I think we can avoid sorting by instead have a `prefix: Option<&Path>` variable and we only update it if the current checked `src` prefix is longer than `prefix`. This gives us the same behavior without sorting (sorting always allocates a vec!)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:72 on 2025-02-06 08:28_

Same as above. It's now possible that we strip the path more than once. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:91 on 2025-02-06 08:29_

We could use a `Cow` here to avoid allocating (we try to avoid allocations in the "check" path. Allocations in the fix path are mostly fine)

---

_Comment by @MichaReiser on 2025-02-06 09:34_

> but apparently I've made the default even more strict than it was before. Is that okay, or should I try to get rid of the new violations? 

Can you explain in what ways you made the check more strict than it used to be before? I ask because it depends if the changed behavior classifies as a bug fix (fine to do in a patch release) or a behavior change (it depends but generally no)

---

_@MichaReiser reviewed on 2025-02-06 09:35_

---

_Comment by @ntBre on 2025-02-06 13:23_

> Can you explain in what ways you made the check more strict than it used to be before? I ask because it depends if the changed behavior classifies as a bug fix (fine to do in a patch release) or a behavior change (it depends but generally no)

For modules, the original code used `package.path()` to get the `module_name`, which appears to give the top-level module name and I think could be considered a bug. This leads to the following output on `main`:

```text
path                                   module_name
----                                   ------------
foobar/collections/abc/__init__.py     foobar
foobar/abc/__init__.py                 foobar
collections/__init__.py                collections
abc/__init__.py                        abc
urlparse/__init__.py                   urlparse
collections/abc/__init__.py            collections
foobar/collections/__init__.py         foobar
foobar/collections/foobar/__init__.py  foobar
foobar/__init__.py                     foobar
collections/foobar/__init__.py         collections
foobar/logging.py                      logging      # new, not in tree above
```

The `package.path()` behavior is different from the treatment of `.py` files, where `main` uses the basename of the file. Instead of checking one or the other, we now consider each component of the module path separately in strict mode, so all of the `foobar/` subtree (`abc`, `collections`, `collections/foobar`) is marked with errors.

I think we're being more faithful to the `## What it does` section in the docs now:

> Checks for modules that use the same names as Python standard-library modules.

But I'm definitely wary of introducing *more* diagnostics, especially when the issue I set out to fix was for too many diagnostics.

---

_Comment by @ntBre on 2025-02-06 18:19_

Thanks again for the feedback here and on Discord! I think I've handled all of the review comments here and also revised the implementation to reflect the upstream behavior better. To help visualize the behavior, here's a table of the strict vs non-strict behavior, operating on the tree from above (again with `foobar/logging.py` added):

| Path                                    | Strict | Non-strict | Main | Flake8 |
|-----------------------------------------|--------|------------|------|--------|
| `abc/__init__.py`                       | ✅     | ✅         | ✅   | ✅     |
| `collections/__init__.py`               | ✅     | ✅         | ✅   | ✅     |
| `collections/abc/__init__.py`           | ✅     | ❌         | ✅   | ✅     |
| `collections/foobar/__init__.py`        | ❌     | ❌         | ✅   | ❌     |
| `foobar/__init__.py`                    | ❌     | ❌         | ❌   | ❌     |
| `foobar/abc/__init__.py`                | ✅     | ❌         | ❌   | ✅     |
| `foobar/collections/__init__.py`        | ✅     | ❌         | ❌   | ✅     |
| `foobar/collections/abc/__init__.py`    | ✅     | ❌         | ❌   | ✅     |
| `foobar/collections/foobar/__init__.py` | ❌     | ❌         | ❌   | ❌     |
| `urlparse/__init__.py`                  | ❌     | ❌         | ❌   | ❌     |
| `foobar/logging.py`                     | ✅     | ❌         | ✅   | ✅     |


Like https://github.com/astral-sh/ruff/issues/15399#issuecomment-2612005957, ✅ means we emit a diagnostic and ❌ means no diagnostic.

We now match the behavior of the upstream `flake8-builtins` in strict mode, but allow configuring a non-strict mode too.

---

_Comment by @MichaReiser on 2025-02-06 18:23_

Thanks for the nice table. I think we should split out the bug fix from the preview behavior change for a clear changelog entry. I hope it isn't too tedious (we can update the changelog manually if it is)

---

_Comment by @ntBre on 2025-02-06 19:02_

> I think we should split out the bug fix from the preview behavior change for a clear changelog entry.

Sure, no problem! Just to make sure, the bug fix is matching the behavior of upstream (so most of the changes in the rule file, minus the new option), and the preview change is everything to do with the new config option?

---

_Comment by @MichaReiser on 2025-02-06 21:13_

Exactly 

---

_Comment by @MichaReiser on 2025-02-07 08:07_

I think I'll have to wait to review this one until your other PR lands. I put it back in draft mode and you can signal that it's ready by marking it ready for review again.

---

_Converted to draft by @MichaReiser on 2025-02-07 08:08_

---

_Marked ready for review by @ntBre on 2025-02-08 18:06_

---

_Comment by @ntBre on 2025-02-08 18:08_

I think this should be ready to review again. It's now only a couple of lines changed in the rule itself, the addition of the config option, and adding the right option value to a couple of existing tests.

---

_@MichaReiser approved on 2025-02-09 18:47_

---

_Merged by @ntBre on 2025-02-10 00:33_

---

_Closed by @ntBre on 2025-02-10 00:33_

---

_Branch deleted on 2025-02-10 00:33_

---

_Comment by @DaniBodor on 2025-02-11 10:38_

I believe that the documentation for this was not yet updated, right? Was trying to read there what this new setting is without having to go through pages of discussion about it, but dont see it there.

---

_Comment by @MichaReiser on 2025-02-11 10:40_

The website should be updated ([see](https://docs.astral.sh/ruff/settings/#lint_flake8-builtins_builtins-strict-checking)) but yes, we should reference the option in the rule's documentation. @ntBre would you mind following up on this?

---

_Comment by @DaniBodor on 2025-02-11 10:44_

I indeed meant the rule documentation, which is where i (and i guess most people) normally first land when looking up how to change settings regarding a specific rule.

---

_Comment by @ntBre on 2025-02-11 13:20_

Will do!

---

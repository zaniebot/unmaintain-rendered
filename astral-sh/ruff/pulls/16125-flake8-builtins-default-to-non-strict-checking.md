```yaml
number: 16125
title: "[`flake8-builtins`] Default to non-strict checking (`A005`)"
type: pull_request
state: merged
author: ntBre
labels:
  - breaking
  - configuration
assignees: []
merged: true
base: micha/ruff-0.10
head: brent/a005-defaults
created_at: 2025-02-12T18:53:34Z
updated_at: 2025-03-11T18:10:04Z
url: https://github.com/astral-sh/ruff/pull/16125
synced_at: 2026-01-10T19:49:01Z
```

# [`flake8-builtins`] Default to non-strict checking (`A005`)

---

_Pull request opened by @ntBre on 2025-02-12 18:53_

## Summary

This PR changes the default value of `lint.flake8-builtins.builtins-strict-checking` added in https://github.com/astral-sh/ruff/pull/15951 from `true` to `false`. This also allows simplifying the default option logic and removes the dependence on preview mode.

https://github.com/astral-sh/ruff/issues/15399 was already closed by #15951, but this change will finalize the behavior mentioned in https://github.com/astral-sh/ruff/issues/15399#issuecomment-2587017147.

As an example, strict checking flags modules based on their last component, so `utils/logging.py` triggers A005. Non-strict checking checks the path to the module, so `utils/logging.py` is allowed (this is the example and desired behavior from #15399 exactly) but a top-level `logging.py` or `logging/__init__.py` is still disallowed.

## Test Plan

Existing tests from #15951 and #16006, with the snapshot updated in `a005_module_shadowing_strict_default` to reflect the new default.


---

_Label `breaking` added by @ntBre on 2025-02-12 18:53_

---

_Label `configuration` added by @ntBre on 2025-02-12 18:53_

---

_Added to milestone `v0.10` by @ntBre on 2025-02-12 18:53_

---

_Comment by @github-actions[bot] on 2025-02-12 19:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -103 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+0 -7 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/0f94e7ebc45eab594361c010012be11abce9cca0/src/snowflake/cli/_plugins/cortex/types.py#L1'>src/snowflake/cli/_plugins/cortex/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/0f94e7ebc45eab594361c010012be11abce9cca0/src/snowflake/cli/_plugins/notebook/types.py#L1'>src/snowflake/cli/_plugins/notebook/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/0f94e7ebc45eab594361c010012be11abce9cca0/src/snowflake/cli/api/console/abc.py#L1'>src/snowflake/cli/api/console/abc.py:1:1:</a> A005 Module `abc` shadows a Python standard-library module
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/0f94e7ebc45eab594361c010012be11abce9cca0/src/snowflake/cli/api/console/enum.py#L1'>src/snowflake/cli/api/console/enum.py:1:1:</a> A005 Module `enum` shadows a Python standard-library module
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/0f94e7ebc45eab594361c010012be11abce9cca0/src/snowflake/cli/api/errno.py#L1'>src/snowflake/cli/api/errno.py:1:1:</a> A005 Module `errno` shadows a Python standard-library module
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/0f94e7ebc45eab594361c010012be11abce9cca0/src/snowflake/cli/api/output/types.py#L1'>src/snowflake/cli/api/output/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/Snowflake-Labs/snowcli/blob/0f94e7ebc45eab594361c010012be11abce9cca0/src/snowflake/cli/api/utils/types.py#L1'>src/snowflake/cli/api/utils/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -47 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/airflow/api_fastapi/common/types.py#L1'>airflow/api_fastapi/common/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/airflow/api_fastapi/execution_api/datamodels/token.py#L1'>airflow/api_fastapi/execution_api/datamodels/token.py:1:1:</a> A005 Module `token` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/airflow/api_fastapi/logging/__init__.py#L1'>airflow/api_fastapi/logging/__init__.py:1:1:</a> A005 Module `logging` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/airflow/io/__init__.py#L1'>airflow/io/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/airflow/io/utils/stat.py#L1'>airflow/io/utils/stat.py:1:1:</a> A005 Module `stat` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/airflow/models/operator.py#L1'>airflow/models/operator.py:1:1:</a> A005 Module `operator` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/airflow/secrets/__init__.py#L1'>airflow/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/airflow/serialization/serializers/datetime.py#L1'>airflow/serialization/serializers/datetime.py:1:1:</a> A005 Module `datetime` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/airflow/utils/email.py#L1'>airflow/utils/email.py:1:1:</a> A005 Module `email` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/airflow/utils/json.py#L1'>airflow/utils/json.py:1:1:</a> A005 Module `json` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/airflow/utils/platform.py#L1'>airflow/utils/platform.py:1:1:</a> A005 Module `platform` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/airflow/utils/types.py#L1'>airflow/utils/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/airflow/utils/warnings.py#L1'>airflow/utils/warnings.py:1:1:</a> A005 Module `warnings` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/providers/amazon/src/airflow/providers/amazon/aws/secrets/__init__.py#L1'>providers/amazon/src/airflow/providers/amazon/aws/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/providers/amazon/tests/unit/amazon/aws/secrets/__init__.py#L1'>providers/amazon/tests/unit/amazon/aws/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/operators/resource.py#L1'>providers/cncf/kubernetes/src/airflow/providers/cncf/kubernetes/operators/resource.py:1:1:</a> A005 Module `resource` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/providers/common/io/src/airflow/providers/common/io/__init__.py#L1'>providers/common/io/src/airflow/providers/common/io/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/providers/common/io/tests/system/common/io/__init__.py#L1'>providers/common/io/tests/system/common/io/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/providers/common/io/tests/unit/common/io/__init__.py#L1'>providers/common/io/tests/unit/common/io/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/providers/edge/src/airflow/providers/edge/cli/dataclasses.py#L1'>providers/edge/src/airflow/providers/edge/cli/dataclasses.py:1:1:</a> A005 Module `dataclasses` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/providers/fab/src/airflow/providers/fab/www/api_connexion/types.py#L1'>providers/fab/src/airflow/providers/fab/www/api_connexion/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/3a9cce97fdb68deaab8f83453c9a03da5f3aa54d/providers/google/src/airflow/providers/google/cloud/secrets/__init__.py#L1'>providers/google/src/airflow/providers/google/cloud/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
... 25 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -15 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/advanced_data_type/types.py#L1'>superset/advanced_data_type/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/commands/dashboard/copy.py#L1'>superset/commands/dashboard/copy.py:1:1:</a> A005 Module `copy` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/dashboards/permalink/types.py#L1'>superset/dashboards/permalink/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/databases/types.py#L1'>superset/databases/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/distributed_lock/types.py#L1'>superset/distributed_lock/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/explore/permalink/types.py#L1'>superset/explore/permalink/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/key_value/types.py#L1'>superset/key_value/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/reports/notifications/email.py#L1'>superset/reports/notifications/email.py:1:1:</a> A005 Module `email` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/reports/types.py#L1'>superset/reports/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/apache/superset/blob/979f890cd5c8ef793825ecd7b9998197f2b7d140/superset/sqllab/permalink/types.py#L1'>superset/sqllab/permalink/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
... 5 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -17 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
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

<pre>
- <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch/functions/secrets.py#L1'>src/latch/functions/secrets.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch/idl/core/types.py#L1'>src/latch/idl/core/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch/registry/types.py#L1'>src/latch/registry/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch/registry/upstream_types/types.py#L1'>src/latch/registry/upstream_types/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch/types/__init__.py#L1'>src/latch/types/__init__.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch/types/glob.py#L1'>src/latch/types/glob.py:1:1:</a> A005 Module `glob` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch/types/json.py#L1'>src/latch/types/json.py:1:1:</a> A005 Module `json` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch_cli/exceptions/traceback.py#L1'>src/latch_cli/exceptions/traceback.py:1:1:</a> A005 Module `traceback` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch_cli/services/cp/glob.py#L1'>src/latch_cli/services/cp/glob.py:1:1:</a> A005 Module `glob` shadows a Python standard-library module
- <a href='https://github.com/latchbio/latch/blob/66920c3cd06ceb02109db8525c62b5569f5c459b/src/latch_cli/snakemake/config/parser.py#L1'>src/latch_cli/snakemake/config/parser.py:1:1:</a> A005 Module `parser` shadows a Python standard-library module
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/834e69d17c254827d9d797abc22d227c3ffcdadf/zerver/actions/typing.py#L1'>zerver/actions/typing.py:1:1:</a> A005 Module `typing` shadows a Python standard-library module
- <a href='https://github.com/zulip/zulip/blob/834e69d17c254827d9d797abc22d227c3ffcdadf/zerver/lib/profile.py#L1'>zerver/lib/profile.py:1:1:</a> A005 Module `profile` shadows a Python standard-library module
- <a href='https://github.com/zulip/zulip/blob/834e69d17c254827d9d797abc22d227c3ffcdadf/zerver/lib/queue.py#L1'>zerver/lib/queue.py:1:1:</a> A005 Module `queue` shadows a Python standard-library module
- <a href='https://github.com/zulip/zulip/blob/834e69d17c254827d9d797abc22d227c3ffcdadf/zerver/lib/types.py#L1'>zerver/lib/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/zulip/zulip/blob/834e69d17c254827d9d797abc22d227c3ffcdadf/zerver/lib/url_preview/types.py#L1'>zerver/lib/url_preview/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
- <a href='https://github.com/zulip/zulip/blob/834e69d17c254827d9d797abc22d227c3ffcdadf/zerver/views/typing.py#L1'>zerver/views/typing.py:1:1:</a> A005 Module `typing` shadows a Python standard-library module
- <a href='https://github.com/zulip/zulip/blob/834e69d17c254827d9d797abc22d227c3ffcdadf/zerver/webhooks/json/__init__.py#L1'>zerver/webhooks/json/__init__.py:1:1:</a> A005 Module `json` shadows a Python standard-library module
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| A005 | 103 | 0 | 103 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-02-13 08:36_

Did you look through the ecosystem check? 

We'll have to wait with merging this until the 0.10 release.

---

_Comment by @ntBre on 2025-02-13 15:57_

I just looked through the ecosystem check and it looks like what I expected. There are a lot of deeply nested `types` and `json` modules that are no longer marked as errors. And there are no new violations, which is as expected too.

I labeled with `breaking` and added to the 0.10 milestone. Should I mark it as a draft to prevent merging too?

---

_Label `do-not-merge` added by @MichaReiser on 2025-02-13 16:00_

---

_Comment by @MichaReiser on 2025-02-13 16:00_

It should be fine. I added the `do-not-merge` label to it

---

_Label `do-not-merge` removed by @MichaReiser on 2025-03-11 08:36_

---

_Comment by @MichaReiser on 2025-03-11 12:11_

@ntBre feel free to merge this after rebasing onto the ruff-0.10 branch

---

_Merged by @ntBre on 2025-03-11 18:10_

---

_Closed by @ntBre on 2025-03-11 18:10_

---

_Branch deleted on 2025-03-11 18:10_

---

```yaml
number: 14512
title: "Extend `invalid-envvar-default (PLW1508)` to detect `os.environ.get`"
type: pull_request
state: merged
author: harupy
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-PLW1508
created_at: 2024-11-21T11:24:55Z
updated_at: 2024-11-22T23:39:23Z
url: https://github.com/astral-sh/ruff/pull/14512
synced_at: 2026-01-10T20:50:57Z
```

# Extend `invalid-envvar-default (PLW1508)` to detect `os.environ.get`

---

_Pull request opened by @harupy on 2024-11-21 11:24_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fix `invalid-envvar-default (PLW1508)` to flag `os.environ.get`. 

```python
import os


os.getenv("a", 1)   # PLW1508
os.environ.get("a", 1)  # no PLW1508
```

## Test Plan

<!-- How was it tested? -->

New test case

---

_Comment by @github-actions[bot] on 2024-11-21 11:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+40 -0 violations, +0 -0 fixes in 9 projects; 45 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4f8cb6e328a1f99ef0550eff3b5d2ec94cb3260f/airflow/api_fastapi/core_api/app.py#L55'>airflow/api_fastapi/core_api/app.py:55:43:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/apache/airflow/blob/4f8cb6e328a1f99ef0550eff3b5d2ec94cb3260f/providers/src/airflow/providers/google/marketing_platform/example_dags/example_display_video.py#L49'>providers/src/airflow/providers/google/marketing_platform/example_dags/example_display_video.py:49:53:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/apache/airflow/blob/4f8cb6e328a1f99ef0550eff3b5d2ec94cb3260f/providers/src/airflow/providers/google/marketing_platform/example_dags/example_display_video.py#L56'>providers/src/airflow/providers/google/marketing_platform/example_dags/example_display_video.py:56:51:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/apache/airflow/blob/4f8cb6e328a1f99ef0550eff3b5d2ec94cb3260f/providers/tests/system/amazon/aws/example_bedrock.py#L58'>providers/tests/system/amazon/aws/example_bedrock.py:58:70:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/apache/airflow/blob/4f8cb6e328a1f99ef0550eff3b5d2ec94cb3260f/providers/tests/system/amazon/aws/example_bedrock.py#L63'>providers/tests/system/amazon/aws/example_bedrock.py:63:86:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/apache/airflow/blob/4f8cb6e328a1f99ef0550eff3b5d2ec94cb3260f/providers/tests/system/google/cloud/storage_transfer/example_cloud_storage_transfer_service_aws.py#L77'>providers/tests/system/google/cloud/storage_transfer/example_cloud_storage_transfer_service_aws.py:77:91:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/apache/airflow/blob/4f8cb6e328a1f99ef0550eff3b5d2ec94cb3260f/scripts/in_container/update_quarantined_test_status.py#L208'>scripts/in_container/update_quarantined_test_status.py:208:47:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/apache/airflow/blob/4f8cb6e328a1f99ef0550eff3b5d2ec94cb3260f/scripts/in_container/update_quarantined_test_status.py#L209'>scripts/in_container/update_quarantined_test_status.py:209:47:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/f8adaf66c198c4497b4134f106abdb5538051c43/superset/config.py#L1579'>superset/config.py:1579:77:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/apache/superset/blob/f8adaf66c198c4497b4134f106abdb5538051c43/superset/migrations/shared/utils.py#L35'>superset/migrations/shared/utils.py:35:55:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/apache/superset/blob/f8adaf66c198c4497b4134f106abdb5538051c43/superset/migrations/versions/2020-09-28_17-57_b56500de1855_add_uuid_column_to_import_mixin.py#L76'>superset/migrations/versions/2020-09-28_17-57_b56500de1855_add_uuid_column_to_import_mixin.py:76:55:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/apache/superset/blob/f8adaf66c198c4497b4134f106abdb5538051c43/superset/migrations/versions/2020-10-21_21-09_96e99fb176a0_add_import_mixing_to_saved_query.py#L54'>superset/migrations/versions/2020-10-21_21-09_96e99fb176a0_add_import_mixing_to_saved_query.py:54:55:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/57c2fc072100a6ebd81e509bc9c6f13270352b9f/samcli/local/docker/container.py#L41'>samcli/local/docker/container.py:41:93:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/aws/aws-sam-cli/blob/57c2fc072100a6ebd81e509bc9c6f13270352b9f/samcli/local/lambdafn/remote_files.py#L33'>samcli/local/lambdafn/remote_files.py:33:93:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/aws/aws-sam-cli/blob/57c2fc072100a6ebd81e509bc9c6f13270352b9f/tests/integration/buildcmd/test_build_terraform_applications.py#L177'>tests/integration/buildcmd/test_build_terraform_applications.py:177:69:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/aws/aws-sam-cli/blob/57c2fc072100a6ebd81e509bc9c6f13270352b9f/tests/integration/package/package_integ_base.py#L51'>tests/integration/package/package_integ_base.py:51:77:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/aws/aws-sam-cli/blob/57c2fc072100a6ebd81e509bc9c6f13270352b9f/tests/integration/package/package_integ_base.py#L55'>tests/integration/package/package_integ_base.py:55:78:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/aws/aws-sam-cli/blob/57c2fc072100a6ebd81e509bc9c6f13270352b9f/tests/integration/publish/publish_app_integ_base.py#L22'>tests/integration/publish/publish_app_integ_base.py:22:75:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/aws/aws-sam-cli/blob/57c2fc072100a6ebd81e509bc9c6f13270352b9f/tests/regression/package/regression_package_base.py#L24'>tests/regression/package/regression_package_base.py:24:75:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/aws/aws-sam-cli/blob/57c2fc072100a6ebd81e509bc9c6f13270352b9f/tests/testing_utils.py#L24'>tests/testing_utils.py:24:50:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/aws/aws-sam-cli/blob/57c2fc072100a6ebd81e509bc9c6f13270352b9f/tests/testing_utils.py#L26'>tests/testing_utils.py:26:50:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/aws/aws-sam-cli/blob/57c2fc072100a6ebd81e509bc9c6f13270352b9f/tests/testing_utils.py#L31'>tests/testing_utils.py:31:54:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/aws/aws-sam-cli/blob/57c2fc072100a6ebd81e509bc9c6f13270352b9f/tests/testing_utils.py#L31'>tests/testing_utils.py:31:94:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/aws/aws-sam-cli/blob/57c2fc072100a6ebd81e509bc9c6f13270352b9f/tests/testing_utils.py#L32'>tests/testing_utils.py:32:45:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/settings.py#L338'>src/bokeh/settings.py:338:53:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/fronzbot/blinkpy/blob/e2c747b5ad295424b08ff4fb03204129155666fc/blinkapp/blinkapp.py#L12'>blinkapp/blinkapp.py:12:48:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/15df63378b8a314e8cc06e49e6b99c302cc2ec79/ibis/backends/clickhouse/tests/conftest.py#L22'>ibis/backends/clickhouse/tests/conftest.py:22:67:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/ibis-project/ibis/blob/15df63378b8a314e8cc06e49e6b99c302cc2ec79/ibis/backends/clickhouse/tests/test_client.py#L221'>ibis/backends/clickhouse/tests/test_client.py:221:62:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/ibis-project/ibis/blob/15df63378b8a314e8cc06e49e6b99c302cc2ec79/ibis/backends/clickhouse/tests/test_client.py#L376'>ibis/backends/clickhouse/tests/test_client.py:376:60:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/ibis-project/ibis/blob/15df63378b8a314e8cc06e49e6b99c302cc2ec79/ibis/backends/exasol/tests/conftest.py#L22'>ibis/backends/exasol/tests/conftest.py:22:59:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/ibis-project/ibis/blob/15df63378b8a314e8cc06e49e6b99c302cc2ec79/ibis/backends/mssql/tests/conftest.py#L18'>ibis/backends/mssql/tests/conftest.py:18:57:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/ibis-project/ibis/blob/15df63378b8a314e8cc06e49e6b99c302cc2ec79/ibis/backends/mysql/tests/conftest.py#L19'>ibis/backends/mysql/tests/conftest.py:19:57:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/ibis-project/ibis/blob/15df63378b8a314e8cc06e49e6b99c302cc2ec79/ibis/backends/oracle/tests/conftest.py#L23'>ibis/backends/oracle/tests/conftest.py:23:59:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/ibis-project/ibis/blob/15df63378b8a314e8cc06e49e6b99c302cc2ec79/ibis/backends/postgres/tests/conftest.py#L37'>ibis/backends/postgres/tests/conftest.py:37:78:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/ibis-project/ibis/blob/15df63378b8a314e8cc06e49e6b99c302cc2ec79/ibis/backends/risingwave/tests/conftest.py#L22'>ibis/backends/risingwave/tests/conftest.py:22:80:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/ibis-project/ibis/blob/15df63378b8a314e8cc06e49e6b99c302cc2ec79/ibis/backends/trino/tests/conftest.py#L31'>ibis/backends/trino/tests/conftest.py:31:73:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
+ <a href='https://github.com/ibis-project/ibis/blob/15df63378b8a314e8cc06e49e6b99c302cc2ec79/ibis/expr/tests/test_visualize.py#L19'>ibis/expr/tests/test_visualize.py:19:39:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/8925fab0b53694d52ebf59aeb77a162978fbe2db/src/latch_cli/snakemake/single_task_snakemake.py#L38'>src/latch_cli/snakemake/single_task_snakemake.py:38:63:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/e62fcb15a70dfb6f4c408cf801f83b216578335b/setup.py#L367'>setup.py:367:54:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/abbe76dcea2b4a5fe0fb848741a01e554ee4b5bd/rotkehlchen/tests/fixtures/google.py#L66'>rotkehlchen/tests/fixtures/google.py:66:69:</a> PLW1508 Invalid type for environment variable default; expected `str` or `None`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW1508 | 40 | 40 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @harupy on 2024-11-21 11:40_

Should I update the doc to mention both `os.environ.get` and `os.getenv`?

---

_Comment by @MichaReiser on 2024-11-21 12:27_

Is this a change to align the rule with the upstream rule? 

I think we should gate this behind preview, considering that it extends the scope of the rule (even though it is in the rule's intent) 

---

_Label `rule` added by @MichaReiser on 2024-11-21 12:27_

---

_Label `preview` added by @MichaReiser on 2024-11-21 12:27_

---

_Comment by @harupy on 2024-11-21 12:32_

> Is this a change to align the rule with the upstream rule?

No, pylint only checks `os.getenv`:

https://github.com/pylint-dev/pylint/blob/68cb5b320653ad64c68ff48a4bb4ba449a01d3a6/pylint/checkers/stdlib.py#L33

> I think we should gate this behind preview, considering that it extends the scope of the rule (even though it is in the rule's intent)

Sounds good. How can I gate it behind preview?

---

_Comment by @harupy on 2024-11-21 12:59_

I think I've figured it out.

---

_Comment by @harupy on 2024-11-21 13:08_

@MichaReiser updated the code.

---

_@harupy reviewed on 2024-11-21 13:17_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/pylint/rules/invalid_envvar_default.rs`:62 on 2024-11-21 13:17_

Is there a simpler way to write this?

---

_@harupy reviewed on 2024-11-22 13:28_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLW1508_invalid_envvar_default.py.snap`:3 on 2024-11-22 13:28_

Not sure why this was removed.

---

_@MichaReiser reviewed on 2024-11-22 14:33_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLW1508_invalid_envvar_default.py.snap`:3 on 2024-11-22 14:33_

Can you try updating your local `cargo insta` version running `cargo install cargo-insta --force`?

---

_Renamed from "Fix `invalid-envvar-default (PLW1508)` to flag `os.environ.get`" to "Dedect `os.environ.get` in `invalid-envvar-default (PLW1508)`" by @MichaReiser on 2024-11-22 14:34_

---

_Renamed from "Dedect `os.environ.get` in `invalid-envvar-default (PLW1508)`" to "Extend `invalid-envvar-default (PLW1508)` to detect `os.environ.get`" by @MichaReiser on 2024-11-22 14:34_

---

_@MichaReiser reviewed on 2024-11-22 14:35_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/invalid_envvar_default.rs`:62 on 2024-11-22 14:35_

I think it's fine. The way it's written now makes it very easy to promote the change out of preview by simply deleting the if and the else branch.

---

_@harupy reviewed on 2024-11-22 14:40_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLW1508_invalid_envvar_default.py.snap`:2 on 2024-11-22 14:40_

```suggestion
source: crates/ruff_linter/src/rules/pylint/mod.rs
snapshot_kind: text
```

---

_@harupy reviewed on 2024-11-22 14:41_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__preview__PLW1508_invalid_envvar_default.py.snap`:2 on 2024-11-22 14:41_

```suggestion
source: crates/ruff_linter/src/rules/pylint/mod.rs
snapshot_kind: text
```

---

_Comment by @MichaReiser on 2024-11-22 14:55_

The extension seem useful in my view and it makes sense that `os.getenv` and `os.environ.get` are treated the same. Considering that `os.getenv` just delegates to `Environ.get` ([source](https://github.com/python/cpython/blob/f83ca6962af973fff6a3124f4bd3d45fea4dd5b8/Lib/os.py#L816-L820))

I wonder if there's a reason for pylint not supporting `os.environ.get`. It was mentioned in the [original issue requesting the rule](https://github.com/pylint-dev/pylint/issues/1669) but then never implemented. 

I think it would be great to open an upstream issue first before we deviate unnecessarily. 

---

_Comment by @harupy on 2024-11-22 15:22_

Sounds good. Let me file a ticket.

---

_Comment by @harupy on 2024-11-22 15:32_

Filed https://github.com/pylint-dev/pylint/issues/10092.

---

_Comment by @MichaReiser on 2024-11-22 16:41_

Thank you @harupy Let's give them a few days. I'm all in for merging if we don't hear back from them in a few days. In case I forget to check in, feel free to ping me! 

---

_Comment by @MichaReiser on 2024-11-22 19:13_

Pylint upstream is okay with supporting this, although they generally recommend using a type checker, which makes sense.

---

_Merged by @MichaReiser on 2024-11-22 19:13_

---

_Closed by @MichaReiser on 2024-11-22 19:13_

---

_Branch deleted on 2024-11-22 23:39_

---

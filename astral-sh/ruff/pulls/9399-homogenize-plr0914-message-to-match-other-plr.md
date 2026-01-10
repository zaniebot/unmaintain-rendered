```yaml
number: 9399
title: homogenize PLR0914 message to match other PLR 09XX rules and pylint message
type: pull_request
state: merged
author: mikaelarguedas
labels:
  - bug
assignees: []
merged: true
base: main
head: homogenize_PLR_err_msg_round2
created_at: 2024-01-05T09:40:14Z
updated_at: 2024-01-05T12:33:06Z
url: https://github.com/astral-sh/ruff/pull/9399
synced_at: 2026-01-10T22:57:09Z
```

# homogenize PLR0914 message to match other PLR 09XX rules and pylint message

---

_Pull request opened by @mikaelarguedas on 2024-01-05 09:40_

Similar to https://github.com/astral-sh/ruff/pull/9308 for PLR0914

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
cargo test / cargo insta review


---

_Comment by @github-actions[bot] on 2024-01-05 09:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+290 -290 violations, +0 -0 fixes in 10 projects; 33 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+37 -37 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/142c7376c6d6566938ced345e97a1e2442e2fd32/airflow/cli/commands/kubernetes_command.py#L77'>airflow/cli/commands/kubernetes_command.py:77:5:</a> PLR0914 Too many local variables (17/15)
- <a href='https://github.com/apache/airflow/blob/142c7376c6d6566938ced345e97a1e2442e2fd32/airflow/cli/commands/kubernetes_command.py#L77'>airflow/cli/commands/kubernetes_command.py:77:5:</a> PLR0914 Too many local variables: (17/15)
+ <a href='https://github.com/apache/airflow/blob/142c7376c6d6566938ced345e97a1e2442e2fd32/airflow/dag_processing/processor.py#L420'>airflow/dag_processing/processor.py:420:9:</a> PLR0914 Too many local variables (21/15)
- <a href='https://github.com/apache/airflow/blob/142c7376c6d6566938ced345e97a1e2442e2fd32/airflow/dag_processing/processor.py#L420'>airflow/dag_processing/processor.py:420:9:</a> PLR0914 Too many local variables: (21/15)
+ <a href='https://github.com/apache/airflow/blob/142c7376c6d6566938ced345e97a1e2442e2fd32/airflow/io/path.py#L96'>airflow/io/path.py:96:9:</a> PLR0914 Too many local variables (16/15)
- <a href='https://github.com/apache/airflow/blob/142c7376c6d6566938ced345e97a1e2442e2fd32/airflow/io/path.py#L96'>airflow/io/path.py:96:9:</a> PLR0914 Too many local variables: (16/15)
+ <a href='https://github.com/apache/airflow/blob/142c7376c6d6566938ced345e97a1e2442e2fd32/airflow/jobs/backfill_job_runner.py#L876'>airflow/jobs/backfill_job_runner.py:876:9:</a> PLR0914 Too many local variables (16/15)
- <a href='https://github.com/apache/airflow/blob/142c7376c6d6566938ced345e97a1e2442e2fd32/airflow/jobs/backfill_job_runner.py#L876'>airflow/jobs/backfill_job_runner.py:876:9:</a> PLR0914 Too many local variables: (16/15)
+ <a href='https://github.com/apache/airflow/blob/142c7376c6d6566938ced345e97a1e2442e2fd32/airflow/jobs/scheduler_job_runner.py#L294'>airflow/jobs/scheduler_job_runner.py:294:9:</a> PLR0914 Too many local variables (35/15)
- <a href='https://github.com/apache/airflow/blob/142c7376c6d6566938ced345e97a1e2442e2fd32/airflow/jobs/scheduler_job_runner.py#L294'>airflow/jobs/scheduler_job_runner.py:294:9:</a> PLR0914 Too many local variables: (35/15)
... 64 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+20 -20 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/ff7fd48f461b747541ce871bb6071911c61dd79c/samcli/commands/deploy/guided_context.py#L108'>samcli/commands/deploy/guided_context.py:108:9:</a> PLR0914 Too many local variables (20/15)
- <a href='https://github.com/aws/aws-sam-cli/blob/ff7fd48f461b747541ce871bb6071911c61dd79c/samcli/commands/deploy/guided_context.py#L108'>samcli/commands/deploy/guided_context.py:108:9:</a> PLR0914 Too many local variables: (20/15)
+ <a href='https://github.com/aws/aws-sam-cli/blob/ff7fd48f461b747541ce871bb6071911c61dd79c/samcli/commands/init/interactive_init_flow.py#L213'>samcli/commands/init/interactive_init_flow.py:213:5:</a> PLR0914 Too many local variables (29/15)
- <a href='https://github.com/aws/aws-sam-cli/blob/ff7fd48f461b747541ce871bb6071911c61dd79c/samcli/commands/init/interactive_init_flow.py#L213'>samcli/commands/init/interactive_init_flow.py:213:5:</a> PLR0914 Too many local variables: (29/15)
+ <a href='https://github.com/aws/aws-sam-cli/blob/ff7fd48f461b747541ce871bb6071911c61dd79c/samcli/commands/pipeline/bootstrap/cli.py#L240'>samcli/commands/pipeline/bootstrap/cli.py:240:5:</a> PLR0914 Too many local variables (23/15)
- <a href='https://github.com/aws/aws-sam-cli/blob/ff7fd48f461b747541ce871bb6071911c61dd79c/samcli/commands/pipeline/bootstrap/cli.py#L240'>samcli/commands/pipeline/bootstrap/cli.py:240:5:</a> PLR0914 Too many local variables: (23/15)
+ <a href='https://github.com/aws/aws-sam-cli/blob/ff7fd48f461b747541ce871bb6071911c61dd79c/samcli/hook_packages/terraform/hooks/prepare/translate.py#L153'>samcli/hook_packages/terraform/hooks/prepare/translate.py:153:5:</a> PLR0914 Too many local variables (31/15)
- <a href='https://github.com/aws/aws-sam-cli/blob/ff7fd48f461b747541ce871bb6071911c61dd79c/samcli/hook_packages/terraform/hooks/prepare/translate.py#L153'>samcli/hook_packages/terraform/hooks/prepare/translate.py:153:5:</a> PLR0914 Too many local variables: (31/15)
+ <a href='https://github.com/aws/aws-sam-cli/blob/ff7fd48f461b747541ce871bb6071911c61dd79c/samcli/lib/iac/cfn/cfn_iac.py#L83'>samcli/lib/iac/cfn/cfn_iac.py:83:9:</a> PLR0914 Too many local variables (21/15)
- <a href='https://github.com/aws/aws-sam-cli/blob/ff7fd48f461b747541ce871bb6071911c61dd79c/samcli/lib/iac/cfn/cfn_iac.py#L83'>samcli/lib/iac/cfn/cfn_iac.py:83:9:</a> PLR0914 Too many local variables: (21/15)
... 30 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+12 -12 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/advanced/extensions/parallel_plot/parallel_plot.py#L14'>examples/advanced/extensions/parallel_plot/parallel_plot.py:14:5:</a> PLR0914 Too many local variables (24/15)
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/advanced/extensions/parallel_plot/parallel_plot.py#L14'>examples/advanced/extensions/parallel_plot/parallel_plot.py:14:5:</a> PLR0914 Too many local variables: (24/15)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/calendars.py#L26'>examples/models/calendars.py:26:5:</a> PLR0914 Too many local variables (21/15)
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/examples/models/calendars.py#L26'>examples/models/calendars.py:26:5:</a> PLR0914 Too many local variables: (21/15)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/embed/bundle.py#L259'>src/bokeh/embed/bundle.py:259:5:</a> PLR0914 Too many local variables (25/15)
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/embed/bundle.py#L259'>src/bokeh/embed/bundle.py:259:5:</a> PLR0914 Too many local variables: (25/15)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/layouts.py#L193'>src/bokeh/layouts.py:193:5:</a> PLR0914 Too many local variables (22/15)
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/layouts.py#L193'>src/bokeh/layouts.py:193:5:</a> PLR0914 Too many local variables: (22/15)
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/models/util/structure.py#L226'>src/bokeh/models/util/structure.py:226:9:</a> PLR0914 Too many local variables (18/15)
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/models/util/structure.py#L226'>src/bokeh/models/util/structure.py:226:9:</a> PLR0914 Too many local variables: (18/15)
... 14 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+7 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/b3ddfd078d5124c93514539d252c96e816831ccf/securedrop/sdconfig.py#L101'>securedrop/sdconfig.py:101:5:</a> PLR0914 Too many local variables (20/15)
- <a href='https://github.com/freedomofpress/securedrop/blob/b3ddfd078d5124c93514539d252c96e816831ccf/securedrop/sdconfig.py#L101'>securedrop/sdconfig.py:101:5:</a> PLR0914 Too many local variables: (20/15)
+ <a href='https://github.com/freedomofpress/securedrop/blob/b3ddfd078d5124c93514539d252c96e816831ccf/securedrop/tests/functional/conftest.py#L95'>securedrop/tests/functional/conftest.py:95:5:</a> PLR0914 Too many local variables (18/15)
- <a href='https://github.com/freedomofpress/securedrop/blob/b3ddfd078d5124c93514539d252c96e816831ccf/securedrop/tests/functional/conftest.py#L95'>securedrop/tests/functional/conftest.py:95:5:</a> PLR0914 Too many local variables: (18/15)
+ <a href='https://github.com/freedomofpress/securedrop/blob/b3ddfd078d5124c93514539d252c96e816831ccf/securedrop/tests/test_integration.py#L120'>securedrop/tests/test_integration.py:120:5:</a> PLR0914 Too many local variables (18/15)
- <a href='https://github.com/freedomofpress/securedrop/blob/b3ddfd078d5124c93514539d252c96e816831ccf/securedrop/tests/test_integration.py#L120'>securedrop/tests/test_integration.py:120:5:</a> PLR0914 Too many local variables: (18/15)
+ <a href='https://github.com/freedomofpress/securedrop/blob/b3ddfd078d5124c93514539d252c96e816831ccf/securedrop/tests/test_integration.py#L220'>securedrop/tests/test_integration.py:220:5:</a> PLR0914 Too many local variables (16/15)
- <a href='https://github.com/freedomofpress/securedrop/blob/b3ddfd078d5124c93514539d252c96e816831ccf/securedrop/tests/test_integration.py#L220'>securedrop/tests/test_integration.py:220:5:</a> PLR0914 Too many local variables: (16/15)
+ <a href='https://github.com/freedomofpress/securedrop/blob/b3ddfd078d5124c93514539d252c96e816831ccf/securedrop/tests/test_journalist.py#L3347'>securedrop/tests/test_journalist.py:3347:5:</a> PLR0914 Too many local variables (17/15)
- <a href='https://github.com/freedomofpress/securedrop/blob/b3ddfd078d5124c93514539d252c96e816831ccf/securedrop/tests/test_journalist.py#L3347'>securedrop/tests/test_journalist.py:3347:5:</a> PLR0914 Too many local variables: (17/15)
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/fronzbot/blinkpy/blob/14c3a4f39af061bcf6af8cb6a319c0a786da678e/blinkpy/sync_module.py#L285'>blinkpy/sync_module.py:285:15:</a> PLR0914 Too many local variables (18/15)
- <a href='https://github.com/fronzbot/blinkpy/blob/14c3a4f39af061bcf6af8cb6a319c0a786da678e/blinkpy/sync_module.py#L285'>blinkpy/sync_module.py:285:15:</a> PLR0914 Too many local variables: (18/15)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+5 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/d8360ab69217d89fc911d3f10674951716943867/ibis/backends/bigquery/udf/core.py#L524'>ibis/backends/bigquery/udf/core.py:524:9:</a> PLR0914 Too many local variables (17/15)
- <a href='https://github.com/ibis-project/ibis/blob/d8360ab69217d89fc911d3f10674951716943867/ibis/backends/bigquery/udf/core.py#L524'>ibis/backends/bigquery/udf/core.py:524:9:</a> PLR0914 Too many local variables: (17/15)
+ <a href='https://github.com/ibis-project/ibis/blob/d8360ab69217d89fc911d3f10674951716943867/ibis/backends/dask/aggcontext.py#L90'>ibis/backends/dask/aggcontext.py:90:9:</a> PLR0914 Too many local variables (19/15)
- <a href='https://github.com/ibis-project/ibis/blob/d8360ab69217d89fc911d3f10674951716943867/ibis/backends/dask/aggcontext.py#L90'>ibis/backends/dask/aggcontext.py:90:9:</a> PLR0914 Too many local variables: (19/15)
+ <a href='https://github.com/ibis-project/ibis/blob/d8360ab69217d89fc911d3f10674951716943867/ibis/backends/pandas/aggcontext.py#L582'>ibis/backends/pandas/aggcontext.py:582:9:</a> PLR0914 Too many local variables (18/15)
- <a href='https://github.com/ibis-project/ibis/blob/d8360ab69217d89fc911d3f10674951716943867/ibis/backends/pandas/aggcontext.py#L582'>ibis/backends/pandas/aggcontext.py:582:9:</a> PLR0914 Too many local variables: (18/15)
+ <a href='https://github.com/ibis-project/ibis/blob/d8360ab69217d89fc911d3f10674951716943867/ibis/backends/pandas/execution/window.py#L258'>ibis/backends/pandas/execution/window.py:258:5:</a> PLR0914 Too many local variables (16/15)
- <a href='https://github.com/ibis-project/ibis/blob/d8360ab69217d89fc911d3f10674951716943867/ibis/backends/pandas/execution/window.py#L258'>ibis/backends/pandas/execution/window.py:258:5:</a> PLR0914 Too many local variables: (16/15)
+ <a href='https://github.com/ibis-project/ibis/blob/d8360ab69217d89fc911d3f10674951716943867/ibis/expr/datatypes/parse.py#L54'>ibis/expr/datatypes/parse.py:54:5:</a> PLR0914 Too many local variables (20/15)
- <a href='https://github.com/ibis-project/ibis/blob/d8360ab69217d89fc911d3f10674951716943867/ibis/expr/datatypes/parse.py#L54'>ibis/expr/datatypes/parse.py:54:5:</a> PLR0914 Too many local variables: (20/15)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+91 -91 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/b2bca5e96545c8b9fb50dbc45ffd71eb71bd2306/pandas/_version.py#L250'>pandas/_version.py:250:5:</a> PLR0914 Too many local variables (17/15)
- <a href='https://github.com/pandas-dev/pandas/blob/b2bca5e96545c8b9fb50dbc45ffd71eb71bd2306/pandas/_version.py#L250'>pandas/_version.py:250:5:</a> PLR0914 Too many local variables: (17/15)
+ <a href='https://github.com/pandas-dev/pandas/blob/b2bca5e96545c8b9fb50dbc45ffd71eb71bd2306/pandas/core/_numba/kernels/sum_.py#L65'>pandas/core/_numba/kernels/sum_.py:65:5:</a> PLR0914 Too many local variables (16/15)
- <a href='https://github.com/pandas-dev/pandas/blob/b2bca5e96545c8b9fb50dbc45ffd71eb71bd2306/pandas/core/_numba/kernels/sum_.py#L65'>pandas/core/_numba/kernels/sum_.py:65:5:</a> PLR0914 Too many local variables: (16/15)
+ <a href='https://github.com/pandas-dev/pandas/blob/b2bca5e96545c8b9fb50dbc45ffd71eb71bd2306/pandas/core/_numba/kernels/var_.py#L171'>pandas/core/_numba/kernels/var_.py:171:5:</a> PLR0914 Too many local variables (16/15)
- <a href='https://github.com/pandas-dev/pandas/blob/b2bca5e96545c8b9fb50dbc45ffd71eb71bd2306/pandas/core/_numba/kernels/var_.py#L171'>pandas/core/_numba/kernels/var_.py:171:5:</a> PLR0914 Too many local variables: (16/15)
+ <a href='https://github.com/pandas-dev/pandas/blob/b2bca5e96545c8b9fb50dbc45ffd71eb71bd2306/pandas/core/_numba/kernels/var_.py#L74'>pandas/core/_numba/kernels/var_.py:74:5:</a> PLR0914 Too many local variables (16/15)
- <a href='https://github.com/pandas-dev/pandas/blob/b2bca5e96545c8b9fb50dbc45ffd71eb71bd2306/pandas/core/_numba/kernels/var_.py#L74'>pandas/core/_numba/kernels/var_.py:74:5:</a> PLR0914 Too many local variables: (16/15)
+ <a href='https://github.com/pandas-dev/pandas/blob/b2bca5e96545c8b9fb50dbc45ffd71eb71bd2306/pandas/core/arraylike.py#L253'>pandas/core/arraylike.py:253:5:</a> PLR0914 Too many local variables (16/15)
- <a href='https://github.com/pandas-dev/pandas/blob/b2bca5e96545c8b9fb50dbc45ffd71eb71bd2306/pandas/core/arraylike.py#L253'>pandas/core/arraylike.py:253:5:</a> PLR0914 Too many local variables: (16/15)
+ <a href='https://github.com/pandas-dev/pandas/blob/b2bca5e96545c8b9fb50dbc45ffd71eb71bd2306/pandas/core/arrays/datetimes.py#L411'>pandas/core/arrays/datetimes.py:411:9:</a> PLR0914 Too many local variables (16/15)
- <a href='https://github.com/pandas-dev/pandas/blob/b2bca5e96545c8b9fb50dbc45ffd71eb71bd2306/pandas/core/arrays/datetimes.py#L411'>pandas/core/arrays/datetimes.py:411:9:</a> PLR0914 Too many local variables: (16/15)
+ <a href='https://github.com/pandas-dev/pandas/blob/b2bca5e96545c8b9fb50dbc45ffd71eb71bd2306/pandas/core/computation/align.py#L87'>pandas/core/computation/align.py:87:5:</a> PLR0914 Too many local variables (20/15)
- <a href='https://github.com/pandas-dev/pandas/blob/b2bca5e96545c8b9fb50dbc45ffd71eb71bd2306/pandas/core/computation/align.py#L87'>pandas/core/computation/align.py:87:5:</a> PLR0914 Too many local variables: (20/15)
+ <a href='https://github.com/pandas-dev/pandas/blob/b2bca5e96545c8b9fb50dbc45ffd71eb71bd2306/pandas/core/frame.py#L11373'>pandas/core/frame.py:11373:9:</a> PLR0914 Too many local variables (17/15)
... 167 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/pip/blob/e88d39ae49ab11a8b80609a018e5a36ea4ccad89/src/pip/_internal/cli/autocompletion.py#L15'>src/pip/_internal/cli/autocompletion.py:15:5:</a> PLR0914 Too many local variables (19/15)
- <a href='https://github.com/pypa/pip/blob/e88d39ae49ab11a8b80609a018e5a36ea4ccad89/src/pip/_internal/cli/autocompletion.py#L15'>src/pip/_internal/cli/autocompletion.py:15:5:</a> PLR0914 Too many local variables: (19/15)
+ <a href='https://github.com/pypa/pip/blob/e88d39ae49ab11a8b80609a018e5a36ea4ccad89/src/pip/_internal/commands/install.py#L266'>src/pip/_internal/commands/install.py:266:9:</a> PLR0914 Too many local variables (35/15)
- <a href='https://github.com/pypa/pip/blob/e88d39ae49ab11a8b80609a018e5a36ea4ccad89/src/pip/_internal/commands/install.py#L266'>src/pip/_internal/commands/install.py:266:9:</a> PLR0914 Too many local variables: (35/15)
+ <a href='https://github.com/pypa/pip/blob/e88d39ae49ab11a8b80609a018e5a36ea4ccad89/src/pip/_internal/operations/install/wheel.py#L426'>src/pip/_internal/operations/install/wheel.py:426:5:</a> PLR0914 Too many local variables (38/15)
- <a href='https://github.com/pypa/pip/blob/e88d39ae49ab11a8b80609a018e5a36ea4ccad89/src/pip/_internal/operations/install/wheel.py#L426'>src/pip/_internal/operations/install/wheel.py:426:5:</a> PLR0914 Too many local variables: (38/15)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+3 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/ec415785d438e860a52ed3c8151c19255508a318/src/scikit_build_core/build/sdist.py#L91'>src/scikit_build_core/build/sdist.py:91:5:</a> PLR0914 Too many local variables (16/15)
- <a href='https://github.com/scikit-build/scikit-build-core/blob/ec415785d438e860a52ed3c8151c19255508a318/src/scikit_build_core/build/sdist.py#L91'>src/scikit_build_core/build/sdist.py:91:5:</a> PLR0914 Too many local variables: (16/15)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/ec415785d438e860a52ed3c8151c19255508a318/src/scikit_build_core/build/wheel.py#L113'>src/scikit_build_core/build/wheel.py:113:5:</a> PLR0914 Too many local variables (38/15)
- <a href='https://github.com/scikit-build/scikit-build-core/blob/ec415785d438e860a52ed3c8151c19255508a318/src/scikit_build_core/build/wheel.py#L113'>src/scikit_build_core/build/wheel.py:113:5:</a> PLR0914 Too many local variables: (38/15)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/ec415785d438e860a52ed3c8151c19255508a318/src/scikit_build_core/setuptools/build_cmake.py#L87'>src/scikit_build_core/setuptools/build_cmake.py:87:9:</a> PLR0914 Too many local variables (17/15)
- <a href='https://github.com/scikit-build/scikit-build-core/blob/ec415785d438e860a52ed3c8151c19255508a318/src/scikit_build_core/setuptools/build_cmake.py#L87'>src/scikit_build_core/setuptools/build_cmake.py:87:9:</a> PLR0914 Too many local variables: (17/15)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+111 -111 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/9fde83c1612dab99dee14dde7a2861f375552bd9/analytics/management/commands/populate_analytics_db.py#L73'>analytics/management/commands/populate_analytics_db.py:73:9:</a> PLR0914 Too many local variables (26/15)
- <a href='https://github.com/zulip/zulip/blob/9fde83c1612dab99dee14dde7a2861f375552bd9/analytics/management/commands/populate_analytics_db.py#L73'>analytics/management/commands/populate_analytics_db.py:73:9:</a> PLR0914 Too many local variables: (26/15)
+ <a href='https://github.com/zulip/zulip/blob/9fde83c1612dab99dee14dde7a2861f375552bd9/analytics/views/installation_activity.py#L94'>analytics/views/installation_activity.py:94:5:</a> PLR0914 Too many local variables (16/15)
- <a href='https://github.com/zulip/zulip/blob/9fde83c1612dab99dee14dde7a2861f375552bd9/analytics/views/installation_activity.py#L94'>analytics/views/installation_activity.py:94:5:</a> PLR0914 Too many local variables: (16/15)
+ <a href='https://github.com/zulip/zulip/blob/9fde83c1612dab99dee14dde7a2861f375552bd9/analytics/views/stats.py#L250'>analytics/views/stats.py:250:5:</a> PLR0914 Too many local variables (17/15)
- <a href='https://github.com/zulip/zulip/blob/9fde83c1612dab99dee14dde7a2861f375552bd9/analytics/views/stats.py#L250'>analytics/views/stats.py:250:5:</a> PLR0914 Too many local variables: (17/15)
+ <a href='https://github.com/zulip/zulip/blob/9fde83c1612dab99dee14dde7a2861f375552bd9/analytics/views/support.py#L156'>analytics/views/support.py:156:5:</a> PLR0914 Too many local variables (27/15)
- <a href='https://github.com/zulip/zulip/blob/9fde83c1612dab99dee14dde7a2861f375552bd9/analytics/views/support.py#L156'>analytics/views/support.py:156:5:</a> PLR0914 Too many local variables: (27/15)
+ <a href='https://github.com/zulip/zulip/blob/9fde83c1612dab99dee14dde7a2861f375552bd9/analytics/views/support.py#L406'>analytics/views/support.py:406:5:</a> PLR0914 Too many local variables (18/15)
- <a href='https://github.com/zulip/zulip/blob/9fde83c1612dab99dee14dde7a2861f375552bd9/analytics/views/support.py#L406'>analytics/views/support.py:406:5:</a> PLR0914 Too many local variables: (18/15)
+ <a href='https://github.com/zulip/zulip/blob/9fde83c1612dab99dee14dde7a2861f375552bd9/corporate/lib/stripe.py#L1233'>corporate/lib/stripe.py:1233:9:</a> PLR0914 Too many local variables (21/15)
- <a href='https://github.com/zulip/zulip/blob/9fde83c1612dab99dee14dde7a2861f375552bd9/corporate/lib/stripe.py#L1233'>corporate/lib/stripe.py:1233:9:</a> PLR0914 Too many local variables: (21/15)
+ <a href='https://github.com/zulip/zulip/blob/9fde83c1612dab99dee14dde7a2861f375552bd9/corporate/lib/stripe.py#L1422'>corporate/lib/stripe.py:1422:9:</a> PLR0914 Too many local variables (16/15)
- <a href='https://github.com/zulip/zulip/blob/9fde83c1612dab99dee14dde7a2861f375552bd9/corporate/lib/stripe.py#L1422'>corporate/lib/stripe.py:1422:9:</a> PLR0914 Too many local variables: (16/15)
+ <a href='https://github.com/zulip/zulip/blob/9fde83c1612dab99dee14dde7a2861f375552bd9/corporate/lib/stripe.py#L1781'>corporate/lib/stripe.py:1781:9:</a> PLR0914 Too many local variables (28/15)
- <a href='https://github.com/zulip/zulip/blob/9fde83c1612dab99dee14dde7a2861f375552bd9/corporate/lib/stripe.py#L1781'>corporate/lib/stripe.py:1781:9:</a> PLR0914 Too many local variables: (28/15)
+ <a href='https://github.com/zulip/zulip/blob/9fde83c1612dab99dee14dde7a2861f375552bd9/corporate/lib/stripe.py#L1962'>corporate/lib/stripe.py:1962:9:</a> PLR0914 Too many local variables (23/15)
- <a href='https://github.com/zulip/zulip/blob/9fde83c1612dab99dee14dde7a2861f375552bd9/corporate/lib/stripe.py#L1962'>corporate/lib/stripe.py:1962:9:</a> PLR0914 Too many local variables: (23/15)
+ <a href='https://github.com/zulip/zulip/blob/9fde83c1612dab99dee14dde7a2861f375552bd9/corporate/tests/test_stripe.py#L1090'>corporate/tests/test_stripe.py:1090:9:</a> PLR0914 Too many local variables (19/15)
... 203 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0914 | 580 | 290 | 290 | 0 | 0 |

</p>
</details>




---

_@charliermarsh approved on 2024-01-05 12:25_

Thanks again!

---

_Label `bug` added by @charliermarsh on 2024-01-05 12:25_

---

_Merged by @charliermarsh on 2024-01-05 12:25_

---

_Closed by @charliermarsh on 2024-01-05 12:25_

---

_Branch deleted on 2024-01-05 12:33_

---

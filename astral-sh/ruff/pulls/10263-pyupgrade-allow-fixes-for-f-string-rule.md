```yaml
number: 10263
title: "[`pyupgrade`] Allow fixes for f-string rule regardless of line length (`UP032`)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - fixes
assignees: []
merged: true
base: main
head: charlie/up
created_at: 2024-03-07T03:33:44Z
updated_at: 2024-03-07T14:14:42Z
url: https://github.com/astral-sh/ruff/pull/10263
synced_at: 2026-01-10T22:47:01Z
```

# [`pyupgrade`] Allow fixes for f-string rule regardless of line length (`UP032`)

---

_Pull request opened by @charliermarsh on 2024-03-07 03:33_

## Summary

This is a follow-up to https://github.com/astral-sh/ruff/pull/10238 to offer fixes for the f-string rule regardless of the line length of the resulting fix. To quote Alex in the linked PR:

> Yes, from the user's perspective I'd rather have a fix that may lead to line length issues than have to fix them myself :-) Cleaning up line lengths is easier than changing from `"".format()` to `f""`

I agree with this position, which is that if we're going to offer a diagnostic, we should really be offering the user the ability to fix it -- otherwise, we're just inconveniencing them.


---

_Label `fixes` added by @charliermarsh on 2024-03-07 03:33_

---

_Comment by @github-actions[bot] on 2024-03-07 03:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +452 -0 fixes in 12 projects; 31 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/354e633ddd7da5fb8310ef6bd1c5b9e5e8763848/tests/conftest.py#L492'>tests/conftest.py:492:13:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/apache/airflow/blob/354e633ddd7da5fb8310ef6bd1c5b9e5e8763848/tests/conftest.py#L492'>tests/conftest.py:492:13:</a> UP032 [*] Use f-string instead of `format` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+0 -0 violations, +134 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py#L865'>Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py:865:17:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py#L865'>Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py:865:17:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py#L562'>Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py:562:13:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py#L562'>Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py:562:13:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py#L737'>Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py:737:22:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py#L737'>Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py:737:22:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py#L809'>Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py:809:13:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py#L809'>Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py:809:13:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py#L887'>Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py:887:26:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py#L887'>Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py:887:26:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/AttivoBotsink/Integrations/AttivoBotsink/AttivoBotsink.py#L752'>Packs/AttivoBotsink/Integrations/AttivoBotsink/AttivoBotsink.py:752:25:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/AttivoBotsink/Integrations/AttivoBotsink/AttivoBotsink.py#L752'>Packs/AttivoBotsink/Integrations/AttivoBotsink/AttivoBotsink.py:752:25:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/Base/Scripts/DBotPreprocessTextData/DBotPreprocessTextData.py#L429'>Packs/Base/Scripts/DBotPreprocessTextData/DBotPreprocessTextData.py:429:26:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/Base/Scripts/DBotPreprocessTextData/DBotPreprocessTextData.py#L429'>Packs/Base/Scripts/DBotPreprocessTextData/DBotPreprocessTextData.py:429:26:</a> UP032 [*] Use f-string instead of `format` call
... 120 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/docker/docker-py">docker/docker-py</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/docker/docker-py/blob/bd164f928ab82e798e30db455903578d06ba2070/tests/unit/api_exec_test.py#L14'>tests/unit/api_exec_test.py:14:51:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/docker/docker-py/blob/bd164f928ab82e798e30db455903578d06ba2070/tests/unit/api_exec_test.py#L14'>tests/unit/api_exec_test.py:14:51:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/docker/docker-py/blob/bd164f928ab82e798e30db455903578d06ba2070/tests/unit/errors_test.py#L128'>tests/unit/errors_test.py:128:15:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/docker/docker-py/blob/bd164f928ab82e798e30db455903578d06ba2070/tests/unit/errors_test.py#L128'>tests/unit/errors_test.py:128:15:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/docker/docker-py/blob/bd164f928ab82e798e30db455903578d06ba2070/tests/unit/errors_test.py#L142'>tests/unit/errors_test.py:142:15:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/docker/docker-py/blob/bd164f928ab82e798e30db455903578d06ba2070/tests/unit/errors_test.py#L142'>tests/unit/errors_test.py:142:15:</a> UP032 [*] Use f-string instead of `format` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+0 -0 violations, +52 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/install_files/ansible-base/roles/restore/files/compare_torrc.py#L56'>install_files/ansible-base/roles/restore/files/compare_torrc.py:56:9:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/install_files/ansible-base/roles/restore/files/compare_torrc.py#L56'>install_files/ansible-base/roles/restore/files/compare_torrc.py:56:9:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/molecule/testinfra/app-code/test_securedrop_rqrequeue.py#L20'>molecule/testinfra/app-code/test_securedrop_rqrequeue.py:20:13:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/molecule/testinfra/app-code/test_securedrop_rqrequeue.py#L20'>molecule/testinfra/app-code/test_securedrop_rqrequeue.py:20:13:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/molecule/testinfra/app-code/test_securedrop_rqrequeue.py#L24'>molecule/testinfra/app-code/test_securedrop_rqrequeue.py:24:13:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/molecule/testinfra/app-code/test_securedrop_rqrequeue.py#L24'>molecule/testinfra/app-code/test_securedrop_rqrequeue.py:24:13:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/molecule/testinfra/app-code/test_securedrop_rqworker.py#L22'>molecule/testinfra/app-code/test_securedrop_rqworker.py:22:13:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/molecule/testinfra/app-code/test_securedrop_rqworker.py#L22'>molecule/testinfra/app-code/test_securedrop_rqworker.py:22:13:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/molecule/testinfra/app-code/test_securedrop_shredder_configuration.py#L19'>molecule/testinfra/app-code/test_securedrop_shredder_configuration.py:19:13:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/molecule/testinfra/app-code/test_securedrop_shredder_configuration.py#L19'>molecule/testinfra/app-code/test_securedrop_shredder_configuration.py:19:13:</a> UP032 [*] Use f-string instead of `format` call
... 42 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+0 -0 violations, +14 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/aab0c139a3fa787aadcd23be213659dd50a5ab5e/ibis/backends/bigquery/udf/core.py#L390'>ibis/backends/bigquery/udf/core.py:390:16:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/ibis-project/ibis/blob/aab0c139a3fa787aadcd23be213659dd50a5ab5e/ibis/backends/bigquery/udf/core.py#L390'>ibis/backends/bigquery/udf/core.py:390:16:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/ibis-project/ibis/blob/aab0c139a3fa787aadcd23be213659dd50a5ab5e/ibis/backends/bigquery/udf/core.py#L440'>ibis/backends/bigquery/udf/core.py:440:16:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/ibis-project/ibis/blob/aab0c139a3fa787aadcd23be213659dd50a5ab5e/ibis/backends/bigquery/udf/core.py#L440'>ibis/backends/bigquery/udf/core.py:440:16:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/ibis-project/ibis/blob/aab0c139a3fa787aadcd23be213659dd50a5ab5e/ibis/backends/dask/__init__.py#L83'>ibis/backends/dask/__init__.py:83:17:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/ibis-project/ibis/blob/aab0c139a3fa787aadcd23be213659dd50a5ab5e/ibis/backends/dask/__init__.py#L83'>ibis/backends/dask/__init__.py:83:17:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/ibis-project/ibis/blob/aab0c139a3fa787aadcd23be213659dd50a5ab5e/ibis/backends/impala/ddl.py#L470'>ibis/backends/impala/ddl.py:470:16:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/ibis-project/ibis/blob/aab0c139a3fa787aadcd23be213659dd50a5ab5e/ibis/backends/impala/ddl.py#L470'>ibis/backends/impala/ddl.py:470:16:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/ibis-project/ibis/blob/aab0c139a3fa787aadcd23be213659dd50a5ab5e/ibis/backends/pandas/__init__.py#L320'>ibis/backends/pandas/__init__.py:320:17:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/ibis-project/ibis/blob/aab0c139a3fa787aadcd23be213659dd50a5ab5e/ibis/backends/pandas/__init__.py#L320'>ibis/backends/pandas/__init__.py:320:17:</a> UP032 [*] Use f-string instead of `format` call
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/examples/example_bulkinsert_json.py#L256'>examples/example_bulkinsert_json.py:256:11:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/examples/example_bulkinsert_json.py#L256'>examples/example_bulkinsert_json.py:256:11:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/examples/example_bulkinsert_numpy.py#L258'>examples/example_bulkinsert_numpy.py:258:11:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/examples/example_bulkinsert_numpy.py#L258'>examples/example_bulkinsert_numpy.py:258:11:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/pymilvus/bulk_writer/buffer.py#L112'>pymilvus/bulk_writer/buffer.py:112:21:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/pymilvus/bulk_writer/buffer.py#L112'>pymilvus/bulk_writer/buffer.py:112:21:</a> UP032 [*] Use f-string instead of `format` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+0 -0 violations, +74 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/mlflow/mlflow/blob/4f2565c8982ff5d2587f661a87a8904ac738f257/examples/multistep_workflow/main.py#L43'>examples/multistep_workflow/main.py:43:17:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/mlflow/mlflow/blob/4f2565c8982ff5d2587f661a87a8904ac738f257/examples/multistep_workflow/main.py#L43'>examples/multistep_workflow/main.py:43:17:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/mlflow/mlflow/blob/4f2565c8982ff5d2587f661a87a8904ac738f257/examples/pytorch/mnist_tensorboard_artifact.py#L153'>examples/pytorch/mnist_tensorboard_artifact.py:153:17:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/mlflow/mlflow/blob/4f2565c8982ff5d2587f661a87a8904ac738f257/examples/pytorch/mnist_tensorboard_artifact.py#L153'>examples/pytorch/mnist_tensorboard_artifact.py:153:17:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/mlflow/mlflow/blob/4f2565c8982ff5d2587f661a87a8904ac738f257/examples/pytorch/mnist_tensorboard_artifact.py#L184'>examples/pytorch/mnist_tensorboard_artifact.py:184:9:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/mlflow/mlflow/blob/4f2565c8982ff5d2587f661a87a8904ac738f257/examples/pytorch/mnist_tensorboard_artifact.py#L184'>examples/pytorch/mnist_tensorboard_artifact.py:184:9:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/mlflow/mlflow/blob/4f2565c8982ff5d2587f661a87a8904ac738f257/examples/pytorch/torchscript/MNIST/mnist_torchscript.py#L50'>examples/pytorch/torchscript/MNIST/mnist_torchscript.py:50:17:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/mlflow/mlflow/blob/4f2565c8982ff5d2587f661a87a8904ac738f257/examples/pytorch/torchscript/MNIST/mnist_torchscript.py#L50'>examples/pytorch/torchscript/MNIST/mnist_torchscript.py:50:17:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/mlflow/mlflow/blob/4f2565c8982ff5d2587f661a87a8904ac738f257/examples/pytorch/torchscript/MNIST/mnist_torchscript.py#L77'>examples/pytorch/torchscript/MNIST/mnist_torchscript.py:77:9:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/mlflow/mlflow/blob/4f2565c8982ff5d2587f661a87a8904ac738f257/examples/pytorch/torchscript/MNIST/mnist_torchscript.py#L77'>examples/pytorch/torchscript/MNIST/mnist_torchscript.py:77:9:</a> UP032 [*] Use f-string instead of `format` call
... 64 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+0 -0 violations, +34 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/src/pip/__pip-runner__.py#L21'>src/pip/__pip-runner__.py:21:9:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/src/pip/__pip-runner__.py#L21'>src/pip/__pip-runner__.py:21:9:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/src/pip/_internal/commands/search.py#L78'>src/pip/_internal/commands/search.py:78:23:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/src/pip/_internal/commands/search.py#L78'>src/pip/_internal/commands/search.py:78:23:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/src/pip/_internal/models/candidate.py#L23'>src/pip/_internal/models/candidate.py:23:16:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/src/pip/_internal/models/candidate.py#L23'>src/pip/_internal/models/candidate.py:23:16:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/src/pip/_internal/operations/install/wheel.py#L507'>src/pip/_internal/operations/install/wheel.py:507:27:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/src/pip/_internal/operations/install/wheel.py#L507'>src/pip/_internal/operations/install/wheel.py:507:27:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/src/pip/_internal/operations/install/wheel.py#L517'>src/pip/_internal/operations/install/wheel.py:517:27:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/src/pip/_internal/operations/install/wheel.py#L517'>src/pip/_internal/operations/install/wheel.py:517:27:</a> UP032 [*] Use f-string instead of `format` call
... 24 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -0 violations, +12 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/1856d56edcd78d28f485f4fff0867d7b966641ee/rotkehlchen/chain/ethereum/abi.py#L77'>rotkehlchen/chain/ethereum/abi.py:77:36:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/rotki/rotki/blob/1856d56edcd78d28f485f4fff0867d7b966641ee/rotkehlchen/chain/ethereum/abi.py#L77'>rotkehlchen/chain/ethereum/abi.py:77:36:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/rotki/rotki/blob/1856d56edcd78d28f485f4fff0867d7b966641ee/rotkehlchen/exchanges/binance.py#L398'>rotkehlchen/exchanges/binance.py:398:25:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/rotki/rotki/blob/1856d56edcd78d28f485f4fff0867d7b966641ee/rotkehlchen/exchanges/binance.py#L398'>rotkehlchen/exchanges/binance.py:398:25:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/rotki/rotki/blob/1856d56edcd78d28f485f4fff0867d7b966641ee/rotkehlchen/exchanges/bitcoinde.py#L236'>rotkehlchen/exchanges/bitcoinde.py:236:17:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/rotki/rotki/blob/1856d56edcd78d28f485f4fff0867d7b966641ee/rotkehlchen/exchanges/bitcoinde.py#L236'>rotkehlchen/exchanges/bitcoinde.py:236:17:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/rotki/rotki/blob/1856d56edcd78d28f485f4fff0867d7b966641ee/rotkehlchen/exchanges/bitmex.py#L207'>rotkehlchen/exchanges/bitmex.py:207:17:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/rotki/rotki/blob/1856d56edcd78d28f485f4fff0867d7b966641ee/rotkehlchen/exchanges/bitmex.py#L207'>rotkehlchen/exchanges/bitmex.py:207:17:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/rotki/rotki/blob/1856d56edcd78d28f485f4fff0867d7b966641ee/rotkehlchen/exchanges/iconomi.py#L187'>rotkehlchen/exchanges/iconomi.py:187:17:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/rotki/rotki/blob/1856d56edcd78d28f485f4fff0867d7b966641ee/rotkehlchen/exchanges/iconomi.py#L187'>rotkehlchen/exchanges/iconomi.py:187:17:</a> UP032 [*] Use f-string instead of `format` call
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/scikit-build/scikit-build/blob/c97af94e3a5d4ba2228cd5425e1290e5bb4ee30a/tests/test_issue342_cmake_osx_args_in_setup.py#L193'>tests/test_issue342_cmake_osx_args_in_setup.py:193:9:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/scikit-build/scikit-build/blob/c97af94e3a5d4ba2228cd5425e1290e5bb4ee30a/tests/test_issue342_cmake_osx_args_in_setup.py#L193'>tests/test_issue342_cmake_osx_args_in_setup.py:193:9:</a> UP032 [*] Use f-string instead of `format` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +38 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/597704fa5ff47791a4f73d575a6ce9c7bf648335/corporate/lib/stripe.py#L2205'>corporate/lib/stripe.py:2205:28:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/zulip/zulip/blob/597704fa5ff47791a4f73d575a6ce9c7bf648335/corporate/lib/stripe.py#L2205'>corporate/lib/stripe.py:2205:28:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/zulip/zulip/blob/597704fa5ff47791a4f73d575a6ce9c7bf648335/corporate/lib/stripe.py#L2207'>corporate/lib/stripe.py:2207:28:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/zulip/zulip/blob/597704fa5ff47791a4f73d575a6ce9c7bf648335/corporate/lib/stripe.py#L2207'>corporate/lib/stripe.py:2207:28:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/zulip/zulip/blob/597704fa5ff47791a4f73d575a6ce9c7bf648335/scripts/lib/zulip_tools.py#L565'>scripts/lib/zulip_tools.py:565:15:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/zulip/zulip/blob/597704fa5ff47791a4f73d575a6ce9c7bf648335/scripts/lib/zulip_tools.py#L565'>scripts/lib/zulip_tools.py:565:15:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/zulip/zulip/blob/597704fa5ff47791a4f73d575a6ce9c7bf648335/tools/lib/provision.py#L48'>tools/lib/provision.py:48:9:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/zulip/zulip/blob/597704fa5ff47791a4f73d575a6ce9c7bf648335/tools/lib/provision.py#L48'>tools/lib/provision.py:48:9:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/zulip/zulip/blob/597704fa5ff47791a4f73d575a6ce9c7bf648335/zerver/actions/create_realm.py#L322'>zerver/actions/create_realm.py:322:19:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/zulip/zulip/blob/597704fa5ff47791a4f73d575a6ce9c7bf648335/zerver/actions/create_realm.py#L322'>zerver/actions/create_realm.py:322:19:</a> UP032 [*] Use f-string instead of `format` call
... 28 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP032 | 452 | 0 | 0 | 452 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +452 -0 fixes in 12 projects; 31 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/354e633ddd7da5fb8310ef6bd1c5b9e5e8763848/tests/conftest.py#L492'>tests/conftest.py:492:13:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/apache/airflow/blob/354e633ddd7da5fb8310ef6bd1c5b9e5e8763848/tests/conftest.py#L492'>tests/conftest.py:492:13:</a> UP032 [*] Use f-string instead of `format` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+0 -0 violations, +134 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py#L865'>Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py:865:17:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py#L865'>Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query.py:865:17:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py#L562'>Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py:562:13:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py#L562'>Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py:562:13:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py#L737'>Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py:737:22:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py#L737'>Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py:737:22:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py#L809'>Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py:809:13:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py#L809'>Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py:809:13:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py#L887'>Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py:887:26:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py#L887'>Packs/ArcSightESM/Integrations/ArcSightESMv2/ArcSightESMv2.py:887:26:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/AttivoBotsink/Integrations/AttivoBotsink/AttivoBotsink.py#L752'>Packs/AttivoBotsink/Integrations/AttivoBotsink/AttivoBotsink.py:752:25:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/AttivoBotsink/Integrations/AttivoBotsink/AttivoBotsink.py#L752'>Packs/AttivoBotsink/Integrations/AttivoBotsink/AttivoBotsink.py:752:25:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/Base/Scripts/DBotPreprocessTextData/DBotPreprocessTextData.py#L429'>Packs/Base/Scripts/DBotPreprocessTextData/DBotPreprocessTextData.py:429:26:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/demisto/content/blob/4f4b97a7156596019526a19db9ae5a84b4485baa/Packs/Base/Scripts/DBotPreprocessTextData/DBotPreprocessTextData.py#L429'>Packs/Base/Scripts/DBotPreprocessTextData/DBotPreprocessTextData.py:429:26:</a> UP032 [*] Use f-string instead of `format` call
... 120 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/docker/docker-py">docker/docker-py</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/docker/docker-py/blob/bd164f928ab82e798e30db455903578d06ba2070/tests/unit/api_exec_test.py#L14'>tests/unit/api_exec_test.py:14:51:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/docker/docker-py/blob/bd164f928ab82e798e30db455903578d06ba2070/tests/unit/api_exec_test.py#L14'>tests/unit/api_exec_test.py:14:51:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/docker/docker-py/blob/bd164f928ab82e798e30db455903578d06ba2070/tests/unit/errors_test.py#L128'>tests/unit/errors_test.py:128:15:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/docker/docker-py/blob/bd164f928ab82e798e30db455903578d06ba2070/tests/unit/errors_test.py#L128'>tests/unit/errors_test.py:128:15:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/docker/docker-py/blob/bd164f928ab82e798e30db455903578d06ba2070/tests/unit/errors_test.py#L142'>tests/unit/errors_test.py:142:15:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/docker/docker-py/blob/bd164f928ab82e798e30db455903578d06ba2070/tests/unit/errors_test.py#L142'>tests/unit/errors_test.py:142:15:</a> UP032 [*] Use f-string instead of `format` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+0 -0 violations, +52 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/install_files/ansible-base/roles/restore/files/compare_torrc.py#L56'>install_files/ansible-base/roles/restore/files/compare_torrc.py:56:9:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/install_files/ansible-base/roles/restore/files/compare_torrc.py#L56'>install_files/ansible-base/roles/restore/files/compare_torrc.py:56:9:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/molecule/testinfra/app-code/test_securedrop_rqrequeue.py#L20'>molecule/testinfra/app-code/test_securedrop_rqrequeue.py:20:13:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/molecule/testinfra/app-code/test_securedrop_rqrequeue.py#L20'>molecule/testinfra/app-code/test_securedrop_rqrequeue.py:20:13:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/molecule/testinfra/app-code/test_securedrop_rqrequeue.py#L24'>molecule/testinfra/app-code/test_securedrop_rqrequeue.py:24:13:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/molecule/testinfra/app-code/test_securedrop_rqrequeue.py#L24'>molecule/testinfra/app-code/test_securedrop_rqrequeue.py:24:13:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/molecule/testinfra/app-code/test_securedrop_rqworker.py#L22'>molecule/testinfra/app-code/test_securedrop_rqworker.py:22:13:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/molecule/testinfra/app-code/test_securedrop_rqworker.py#L22'>molecule/testinfra/app-code/test_securedrop_rqworker.py:22:13:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/molecule/testinfra/app-code/test_securedrop_shredder_configuration.py#L19'>molecule/testinfra/app-code/test_securedrop_shredder_configuration.py:19:13:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/molecule/testinfra/app-code/test_securedrop_shredder_configuration.py#L19'>molecule/testinfra/app-code/test_securedrop_shredder_configuration.py:19:13:</a> UP032 [*] Use f-string instead of `format` call
... 42 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+0 -0 violations, +14 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/aab0c139a3fa787aadcd23be213659dd50a5ab5e/ibis/backends/bigquery/udf/core.py#L390'>ibis/backends/bigquery/udf/core.py:390:16:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/ibis-project/ibis/blob/aab0c139a3fa787aadcd23be213659dd50a5ab5e/ibis/backends/bigquery/udf/core.py#L390'>ibis/backends/bigquery/udf/core.py:390:16:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/ibis-project/ibis/blob/aab0c139a3fa787aadcd23be213659dd50a5ab5e/ibis/backends/bigquery/udf/core.py#L440'>ibis/backends/bigquery/udf/core.py:440:16:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/ibis-project/ibis/blob/aab0c139a3fa787aadcd23be213659dd50a5ab5e/ibis/backends/bigquery/udf/core.py#L440'>ibis/backends/bigquery/udf/core.py:440:16:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/ibis-project/ibis/blob/aab0c139a3fa787aadcd23be213659dd50a5ab5e/ibis/backends/dask/__init__.py#L83'>ibis/backends/dask/__init__.py:83:17:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/ibis-project/ibis/blob/aab0c139a3fa787aadcd23be213659dd50a5ab5e/ibis/backends/dask/__init__.py#L83'>ibis/backends/dask/__init__.py:83:17:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/ibis-project/ibis/blob/aab0c139a3fa787aadcd23be213659dd50a5ab5e/ibis/backends/impala/ddl.py#L470'>ibis/backends/impala/ddl.py:470:16:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/ibis-project/ibis/blob/aab0c139a3fa787aadcd23be213659dd50a5ab5e/ibis/backends/impala/ddl.py#L470'>ibis/backends/impala/ddl.py:470:16:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/ibis-project/ibis/blob/aab0c139a3fa787aadcd23be213659dd50a5ab5e/ibis/backends/pandas/__init__.py#L320'>ibis/backends/pandas/__init__.py:320:17:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/ibis-project/ibis/blob/aab0c139a3fa787aadcd23be213659dd50a5ab5e/ibis/backends/pandas/__init__.py#L320'>ibis/backends/pandas/__init__.py:320:17:</a> UP032 [*] Use f-string instead of `format` call
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/examples/example_bulkinsert_json.py#L256'>examples/example_bulkinsert_json.py:256:11:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/examples/example_bulkinsert_json.py#L256'>examples/example_bulkinsert_json.py:256:11:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/examples/example_bulkinsert_numpy.py#L258'>examples/example_bulkinsert_numpy.py:258:11:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/examples/example_bulkinsert_numpy.py#L258'>examples/example_bulkinsert_numpy.py:258:11:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/pymilvus/bulk_writer/buffer.py#L112'>pymilvus/bulk_writer/buffer.py:112:21:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/milvus-io/pymilvus/blob/5b5e3618408c1e0a6125e34ba14210e1ca351b72/pymilvus/bulk_writer/buffer.py#L112'>pymilvus/bulk_writer/buffer.py:112:21:</a> UP032 [*] Use f-string instead of `format` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+0 -0 violations, +74 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/mlflow/mlflow/blob/4f2565c8982ff5d2587f661a87a8904ac738f257/examples/multistep_workflow/main.py#L43'>examples/multistep_workflow/main.py:43:17:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/mlflow/mlflow/blob/4f2565c8982ff5d2587f661a87a8904ac738f257/examples/multistep_workflow/main.py#L43'>examples/multistep_workflow/main.py:43:17:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/mlflow/mlflow/blob/4f2565c8982ff5d2587f661a87a8904ac738f257/examples/pytorch/mnist_tensorboard_artifact.py#L153'>examples/pytorch/mnist_tensorboard_artifact.py:153:17:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/mlflow/mlflow/blob/4f2565c8982ff5d2587f661a87a8904ac738f257/examples/pytorch/mnist_tensorboard_artifact.py#L153'>examples/pytorch/mnist_tensorboard_artifact.py:153:17:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/mlflow/mlflow/blob/4f2565c8982ff5d2587f661a87a8904ac738f257/examples/pytorch/mnist_tensorboard_artifact.py#L184'>examples/pytorch/mnist_tensorboard_artifact.py:184:9:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/mlflow/mlflow/blob/4f2565c8982ff5d2587f661a87a8904ac738f257/examples/pytorch/mnist_tensorboard_artifact.py#L184'>examples/pytorch/mnist_tensorboard_artifact.py:184:9:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/mlflow/mlflow/blob/4f2565c8982ff5d2587f661a87a8904ac738f257/examples/pytorch/torchscript/MNIST/mnist_torchscript.py#L50'>examples/pytorch/torchscript/MNIST/mnist_torchscript.py:50:17:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/mlflow/mlflow/blob/4f2565c8982ff5d2587f661a87a8904ac738f257/examples/pytorch/torchscript/MNIST/mnist_torchscript.py#L50'>examples/pytorch/torchscript/MNIST/mnist_torchscript.py:50:17:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/mlflow/mlflow/blob/4f2565c8982ff5d2587f661a87a8904ac738f257/examples/pytorch/torchscript/MNIST/mnist_torchscript.py#L77'>examples/pytorch/torchscript/MNIST/mnist_torchscript.py:77:9:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/mlflow/mlflow/blob/4f2565c8982ff5d2587f661a87a8904ac738f257/examples/pytorch/torchscript/MNIST/mnist_torchscript.py#L77'>examples/pytorch/torchscript/MNIST/mnist_torchscript.py:77:9:</a> UP032 [*] Use f-string instead of `format` call
... 64 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+0 -0 violations, +34 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/src/pip/__pip-runner__.py#L21'>src/pip/__pip-runner__.py:21:9:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/src/pip/__pip-runner__.py#L21'>src/pip/__pip-runner__.py:21:9:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/src/pip/_internal/commands/search.py#L78'>src/pip/_internal/commands/search.py:78:23:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/src/pip/_internal/commands/search.py#L78'>src/pip/_internal/commands/search.py:78:23:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/src/pip/_internal/models/candidate.py#L23'>src/pip/_internal/models/candidate.py:23:16:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/src/pip/_internal/models/candidate.py#L23'>src/pip/_internal/models/candidate.py:23:16:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/src/pip/_internal/operations/install/wheel.py#L507'>src/pip/_internal/operations/install/wheel.py:507:27:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/src/pip/_internal/operations/install/wheel.py#L507'>src/pip/_internal/operations/install/wheel.py:507:27:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/src/pip/_internal/operations/install/wheel.py#L517'>src/pip/_internal/operations/install/wheel.py:517:27:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/pypa/pip/blob/db99b5be855c261361df5a44806eadcff96dc039/src/pip/_internal/operations/install/wheel.py#L517'>src/pip/_internal/operations/install/wheel.py:517:27:</a> UP032 [*] Use f-string instead of `format` call
... 24 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -0 violations, +12 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/1856d56edcd78d28f485f4fff0867d7b966641ee/rotkehlchen/chain/ethereum/abi.py#L77'>rotkehlchen/chain/ethereum/abi.py:77:36:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/rotki/rotki/blob/1856d56edcd78d28f485f4fff0867d7b966641ee/rotkehlchen/chain/ethereum/abi.py#L77'>rotkehlchen/chain/ethereum/abi.py:77:36:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/rotki/rotki/blob/1856d56edcd78d28f485f4fff0867d7b966641ee/rotkehlchen/exchanges/binance.py#L398'>rotkehlchen/exchanges/binance.py:398:25:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/rotki/rotki/blob/1856d56edcd78d28f485f4fff0867d7b966641ee/rotkehlchen/exchanges/binance.py#L398'>rotkehlchen/exchanges/binance.py:398:25:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/rotki/rotki/blob/1856d56edcd78d28f485f4fff0867d7b966641ee/rotkehlchen/exchanges/bitcoinde.py#L236'>rotkehlchen/exchanges/bitcoinde.py:236:17:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/rotki/rotki/blob/1856d56edcd78d28f485f4fff0867d7b966641ee/rotkehlchen/exchanges/bitcoinde.py#L236'>rotkehlchen/exchanges/bitcoinde.py:236:17:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/rotki/rotki/blob/1856d56edcd78d28f485f4fff0867d7b966641ee/rotkehlchen/exchanges/bitmex.py#L207'>rotkehlchen/exchanges/bitmex.py:207:17:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/rotki/rotki/blob/1856d56edcd78d28f485f4fff0867d7b966641ee/rotkehlchen/exchanges/bitmex.py#L207'>rotkehlchen/exchanges/bitmex.py:207:17:</a> UP032 [*] Use f-string instead of `format` call
- <a href='https://github.com/rotki/rotki/blob/1856d56edcd78d28f485f4fff0867d7b966641ee/rotkehlchen/exchanges/iconomi.py#L187'>rotkehlchen/exchanges/iconomi.py:187:17:</a> UP032 Use f-string instead of `format` call
+ <a href='https://github.com/rotki/rotki/blob/1856d56edcd78d28f485f4fff0867d7b966641ee/rotkehlchen/exchanges/iconomi.py#L187'>rotkehlchen/exchanges/iconomi.py:187:17:</a> UP032 [*] Use f-string instead of `format` call
... 2 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP032 | 452 | 0 | 0 | 452 | 0 |

</p>
</details>




---

_@MichaReiser approved on 2024-03-07 07:48_

I understand the motivation for changing this and am sorry for the frustration that it has caused. 

What I worry is that this now is a selective view. People agreeing with fixing code that increases the line length aren't represented. For example, [this comment](https://github.com/astral-sh/ruff/issues/8106#issuecomment-1874360713) which seems a fair concern. 

To me, it seems that we should decide and change this holistically rather than on a rule-by-rule basis, especially because we should avoid switching between the two paradigms. 

I'm okay moving forward with this because I share the sentiment that we shouldn't gate our fixes based on line-length. But I worry that we leave a group of people underrepresented. 

---

_Comment by @charliermarsh on 2024-03-07 13:59_

That's a fair position! But from that perspective, we likely shouldn't have changed this rule in the first place, right? Since we didn't establish a clear policy or poll users on both ends of the spectrum?

In my view, adding the diagnostic but not the fix just creates more work for users. It's more work to address the violation themselves than to fix-up the fix.

---

_Merged by @charliermarsh on 2024-03-07 13:59_

---

_Closed by @charliermarsh on 2024-03-07 13:59_

---

_Branch deleted on 2024-03-07 13:59_

---

_Comment by @alex on 2024-03-07 14:14_

Thank you!

---

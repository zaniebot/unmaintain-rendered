```yaml
number: 8838
title: Allow space-before-colon after end-of-slice
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/E203
created_at: 2023-11-25T18:10:32Z
updated_at: 2023-11-25T18:22:25Z
url: https://github.com/astral-sh/ruff/pull/8838
synced_at: 2026-01-10T23:40:55Z
```

# Allow space-before-colon after end-of-slice

---

_Pull request opened by @charliermarsh on 2023-11-25 18:10_

Closes https://github.com/astral-sh/ruff/issues/8752.

---

_Label `bug` added by @charliermarsh on 2023-11-25 18:10_

---

_Merged by @charliermarsh on 2023-11-25 18:16_

---

_Closed by @charliermarsh on 2023-11-25 18:16_

---

_Branch deleted on 2023-11-25 18:16_

---

_Comment by @github-actions[bot] on 2023-11-25 18:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -137 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/DisnakeDev/disnake/blob/cc5db415a8d1988b482acc4e8808489333f727b0/disnake/ext/commands/flag_converter.py#L593'>disnake/ext/commands/flag_converter.py:593:51:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/DisnakeDev/disnake/blob/cc5db415a8d1988b482acc4e8808489333f727b0/disnake/ext/commands/view.py#L70'>disnake/ext/commands/view.py:70:40:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -7 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/core/featurizers/precomputation.py#L259'>rasa/core/featurizers/precomputation.py:259:71:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/shared/core/trackers.py#L472'>rasa/shared/core/trackers.py:472:65:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/shared/core/training_data/visualization.py#L229'>rasa/shared/core/training_data/visualization.py:229:52:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/shared/nlu/training_data/message.py#L472'>rasa/shared/nlu/training_data/message.py:472:61:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/tests/test_server.py#L2211'>tests/test_server.py:2211:82:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/tests/test_server.py#L2242'>tests/test_server.py:2242:82:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/tests/test_server.py#L2246'>tests/test_server.py:2246:84:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -23 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/177da9016bbedcfa49c08256fdaf2fb537b97d6c/airflow/auth/managers/fab/fab_auth_manager.py#L326'>airflow/auth/managers/fab/fab_auth_manager.py:326:84:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/apache/airflow/blob/177da9016bbedcfa49c08256fdaf2fb537b97d6c/airflow/models/taskmixin.py#L167'>airflow/models/taskmixin.py:167:52:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/apache/airflow/blob/177da9016bbedcfa49c08256fdaf2fb537b97d6c/airflow/providers/amazon/aws/hooks/sagemaker.py#L123'>airflow/providers/amazon/aws/hooks/sagemaker.py:123:81:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/apache/airflow/blob/177da9016bbedcfa49c08256fdaf2fb537b97d6c/airflow/providers/cncf/kubernetes/operators/resource.py#L91'>airflow/providers/cncf/kubernetes/operators/resource.py:91:56:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/apache/airflow/blob/177da9016bbedcfa49c08256fdaf2fb537b97d6c/airflow/providers/google/cloud/transfers/azure_fileshare_to_gcs.py#L148'>airflow/providers/google/cloud/transfers/azure_fileshare_to_gcs.py:148:75:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/apache/airflow/blob/177da9016bbedcfa49c08256fdaf2fb537b97d6c/airflow/providers_manager.py#L170'>airflow/providers_manager.py:170:56:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/apache/airflow/blob/177da9016bbedcfa49c08256fdaf2fb537b97d6c/airflow/providers_manager.py#L658'>airflow/providers_manager.py:658:86:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/apache/airflow/blob/177da9016bbedcfa49c08256fdaf2fb537b97d6c/dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py#L557'>dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py:557:55:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/apache/airflow/blob/177da9016bbedcfa49c08256fdaf2fb537b97d6c/dev/breeze/src/airflow_breeze/utils/packages.py#L297'>dev/breeze/src/airflow_breeze/utils/packages.py:297:61:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/apache/airflow/blob/177da9016bbedcfa49c08256fdaf2fb537b97d6c/dev/breeze/src/airflow_breeze/utils/path_utils.py#L96'>dev/breeze/src/airflow_breeze/utils/path_utils.py:96:36:</a> E203 [*] Whitespace before ':'
... 13 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+0 -11 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/05bc6bcb3cab05d4859ef21d80d58284dfc4c2fc/samcli/hook_packages/terraform/hooks/prepare/resource_linking.py#L1018'>samcli/hook_packages/terraform/hooks/prepare/resource_linking.py:1018:78:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/aws/aws-sam-cli/blob/05bc6bcb3cab05d4859ef21d80d58284dfc4c2fc/samcli/hook_packages/terraform/hooks/prepare/resource_linking.py#L1034'>samcli/hook_packages/terraform/hooks/prepare/resource_linking.py:1034:61:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/aws/aws-sam-cli/blob/05bc6bcb3cab05d4859ef21d80d58284dfc4c2fc/samcli/hook_packages/terraform/hooks/prepare/resource_linking.py#L1926'>samcli/hook_packages/terraform/hooks/prepare/resource_linking.py:1926:42:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/aws/aws-sam-cli/blob/05bc6bcb3cab05d4859ef21d80d58284dfc4c2fc/samcli/hook_packages/terraform/hooks/prepare/resource_linking.py#L856'>samcli/hook_packages/terraform/hooks/prepare/resource_linking.py:856:97:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/aws/aws-sam-cli/blob/05bc6bcb3cab05d4859ef21d80d58284dfc4c2fc/samcli/hook_packages/terraform/hooks/prepare/resource_linking.py#L875'>samcli/hook_packages/terraform/hooks/prepare/resource_linking.py:875:65:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/aws/aws-sam-cli/blob/05bc6bcb3cab05d4859ef21d80d58284dfc4c2fc/samcli/hook_packages/terraform/hooks/prepare/resource_linking.py#L932'>samcli/hook_packages/terraform/hooks/prepare/resource_linking.py:932:82:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/aws/aws-sam-cli/blob/05bc6bcb3cab05d4859ef21d80d58284dfc4c2fc/samcli/hook_packages/terraform/hooks/prepare/resource_linking.py#L939'>samcli/hook_packages/terraform/hooks/prepare/resource_linking.py:939:65:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/aws/aws-sam-cli/blob/05bc6bcb3cab05d4859ef21d80d58284dfc4c2fc/samcli/hook_packages/terraform/hooks/prepare/translate.py#L395'>samcli/hook_packages/terraform/hooks/prepare/translate.py:395:87:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/aws/aws-sam-cli/blob/05bc6bcb3cab05d4859ef21d80d58284dfc4c2fc/samcli/lib/schemas/schemas_api_caller.py#L187'>samcli/lib/schemas/schemas_api_caller.py:187:104:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/aws/aws-sam-cli/blob/05bc6bcb3cab05d4859ef21d80d58284dfc4c2fc/samcli/lib/schemas/schemas_directory_hierarchy_builder.py#L15'>samcli/lib/schemas/schemas_directory_hierarchy_builder.py:15:47:</a> E203 [*] Whitespace before ':'
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/freedomofpress/securedrop/blob/4d9b74b7e494b6a8e2d06677bc7c20d558b5f078/securedrop/pretty_bad_protocol/_util.py#L227'>securedrop/pretty_bad_protocol/_util.py:227:35:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/freedomofpress/securedrop/blob/4d9b74b7e494b6a8e2d06677bc7c20d558b5f078/securedrop/tests/test_journalist.py#L3046'>securedrop/tests/test_journalist.py:3046:49:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/a465beeaf13f0bae6db47adbe56082daddf34319/ibis/backends/dask/tests/execution/test_strings.py#L24'>ibis/backends/dask/tests/execution/test_strings.py:24:39:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/ibis-project/ibis/blob/a465beeaf13f0bae6db47adbe56082daddf34319/ibis/backends/pandas/aggcontext.py#L472'>ibis/backends/pandas/aggcontext.py:472:57:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/ibis-project/ibis/blob/a465beeaf13f0bae6db47adbe56082daddf34319/ibis/backends/pandas/tests/execution/test_strings.py#L24'>ibis/backends/pandas/tests/execution/test_strings.py:24:39:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/ibis-project/ibis/blob/a465beeaf13f0bae6db47adbe56082daddf34319/ibis/backends/tests/test_string.py#L704'>ibis/backends/tests/test_string.py:704:71:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/jrnl-org/jrnl">jrnl-org/jrnl</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/jrnl-org/jrnl/blob/48a31e8154b7201ace29ec71461ffc82fa01920b/jrnl/journals/Entry.py#L231'>jrnl/journals/Entry.py:231:53:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/jrnl-org/jrnl/blob/48a31e8154b7201ace29ec71461ffc82fa01920b/jrnl/journals/Journal.py#L367'>jrnl/journals/Journal.py:367:44:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/jrnl-org/jrnl/blob/48a31e8154b7201ace29ec71461ffc82fa01920b/jrnl/journals/Journal.py#L449'>jrnl/journals/Journal.py:449:67:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/jrnl-org/jrnl/blob/48a31e8154b7201ace29ec71461ffc82fa01920b/jrnl/plugins/fancy_exporter.py#L87'>jrnl/plugins/fancy_exporter.py:87:60:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/jrnl-org/jrnl/blob/48a31e8154b7201ace29ec71461ffc82fa01920b/tests/lib/helpers.py#L38'>tests/lib/helpers.py:38:71:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/jrnl-org/jrnl/blob/48a31e8154b7201ace29ec71461ffc82fa01920b/tests/lib/helpers.py#L40'>tests/lib/helpers.py:40:59:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/lnbits/lnbits/blob/3a134672e95b75b0e354e8109f0587029d12489b/lnbits/core/migrations.py#L112'>lnbits/core/migrations.py:112:50:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/lnbits/lnbits/blob/3a134672e95b75b0e354e8109f0587029d12489b/lnbits/middleware.py#L205'>lnbits/middleware.py:205:59:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/lnbits/lnbits/blob/3a134672e95b75b0e354e8109f0587029d12489b/lnbits/nodes/cln.py#L291'>lnbits/nodes/cln.py:291:45:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/lnbits/lnbits/blob/3a134672e95b75b0e354e8109f0587029d12489b/lnbits/nodes/cln.py#L307'>lnbits/nodes/cln.py:307:47:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/lnbits/lnbits/blob/3a134672e95b75b0e354e8109f0587029d12489b/lnbits/wallets/lnbits.py#L155'>lnbits/wallets/lnbits.py:155:68:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/lnbits/lnbits/blob/3a134672e95b75b0e354e8109f0587029d12489b/lnbits/wallets/lntips.py#L153'>lnbits/wallets/lntips.py:153:52:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/9075512bb91b97dea74c853dbf57d52b7bb3760d/pymilvus/orm/iterator.py#L137'>pymilvus/orm/iterator.py:137:63:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+0 -26 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/mlflow/mlflow/blob/d0062b2dbac7e4eb5013040c31ce1202dfb71865/examples/jwt_auth/jwt_auth.py#L28'>examples/jwt_auth/jwt_auth.py:28:41:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/mlflow/mlflow/blob/d0062b2dbac7e4eb5013040c31ce1202dfb71865/mlflow/models/evaluation/base.py#L442'>mlflow/models/evaluation/base.py:442:69:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/mlflow/mlflow/blob/d0062b2dbac7e4eb5013040c31ce1202dfb71865/mlflow/models/evaluation/default_evaluator.py#L1635'>mlflow/models/evaluation/default_evaluator.py:1635:92:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/mlflow/mlflow/blob/d0062b2dbac7e4eb5013040c31ce1202dfb71865/mlflow/projects/utils.py#L57'>mlflow/projects/utils.py:57:63:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/mlflow/mlflow/blob/d0062b2dbac7e4eb5013040c31ce1202dfb71865/mlflow/pyfunc/model.py#L293'>mlflow/pyfunc/model.py:293:58:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/mlflow/mlflow/blob/d0062b2dbac7e4eb5013040c31ce1202dfb71865/mlflow/recipes/utils/execution.py#L109'>mlflow/recipes/utils/execution.py:109:76:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/mlflow/mlflow/blob/d0062b2dbac7e4eb5013040c31ce1202dfb71865/mlflow/sagemaker/__init__.py#L1980'>mlflow/sagemaker/__init__.py:1980:67:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/mlflow/mlflow/blob/d0062b2dbac7e4eb5013040c31ce1202dfb71865/mlflow/store/artifact/azure_blob_artifact_repo.py#L167'>mlflow/store/artifact/azure_blob_artifact_repo.py:167:52:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/mlflow/mlflow/blob/d0062b2dbac7e4eb5013040c31ce1202dfb71865/mlflow/store/artifact/azure_data_lake_artifact_repo.py#L125'>mlflow/store/artifact/azure_data_lake_artifact_repo.py:125:76:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/mlflow/mlflow/blob/d0062b2dbac7e4eb5013040c31ce1202dfb71865/mlflow/store/artifact/gcs_artifact_repo.py#L126'>mlflow/store/artifact/gcs_artifact_repo.py:126:59:</a> E203 [*] Whitespace before ':'
... 16 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+0 -10 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pypa/pip/blob/2a0acb595c4b6394f1f3a4e7ef034ac2e3e8c17e/src/pip/_internal/operations/freeze.py#L89'>src/pip/_internal/operations/freeze.py:89:58:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/pypa/pip/blob/2a0acb595c4b6394f1f3a4e7ef034ac2e3e8c17e/src/pip/_internal/req/req_install.py#L733'>src/pip/_internal/req/req_install.py:733:40:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/pypa/pip/blob/2a0acb595c4b6394f1f3a4e7ef034ac2e3e8c17e/src/pip/_internal/utils/compatibility_tags.py#L37'>src/pip/_internal/utils/compatibility_tags.py:37:53:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/pypa/pip/blob/2a0acb595c4b6394f1f3a4e7ef034ac2e3e8c17e/src/pip/_internal/utils/encoding.py#L26'>src/pip/_internal/utils/encoding.py:26:33:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/pypa/pip/blob/2a0acb595c4b6394f1f3a4e7ef034ac2e3e8c17e/src/pip/_internal/utils/misc.py#L203'>src/pip/_internal/utils/misc.py:203:43:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/pypa/pip/blob/2a0acb595c4b6394f1f3a4e7ef034ac2e3e8c17e/src/pip/_internal/vcs/git.py#L127'>src/pip/_internal/vcs/git.py:127:42:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/pypa/pip/blob/2a0acb595c4b6394f1f3a4e7ef034ac2e3e8c17e/src/pip/_internal/vcs/subversion.py#L220'>src/pip/_internal/vcs/subversion.py:220:46:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/pypa/pip/blob/2a0acb595c4b6394f1f3a4e7ef034ac2e3e8c17e/tests/functional/test_check.py#L10'>tests/functional/test_check.py:10:62:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/pypa/pip/blob/2a0acb595c4b6394f1f3a4e7ef034ac2e3e8c17e/tests/lib/__init__.py#L529'>tests/lib/__init__.py:529:51:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/pypa/pip/blob/2a0acb595c4b6394f1f3a4e7ef034ac2e3e8c17e/tests/unit/test_req_uninstall.py#L260'>tests/unit/test_req_uninstall.py:260:44:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/reflex-dev/reflex/blob/d61b83fde725c9804a94d6a8accb14f6088ac7b0/reflex/__init__.py#L293'>reflex/__init__.py:293:56:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/tiangolo/fastapi">tiangolo/fastapi</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/tiangolo/fastapi/blob/ac93277d3b506f4076aee1af6ae5a86406f545c6/docs_src/generate_clients/tutorial004.py#L12'>docs_src/generate_clients/tutorial004.py:12:55:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/yandex/ch-backup">yandex/ch-backup</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/yandex/ch-backup/blob/26d5b989362ba5136480c0cdd6525c59049cc86e/ch_backup/util.py#L84'>ch_backup/util.py:84:31:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -36 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/2a65183991d84f8c4ceb016faff0ff340b665799/tools/documentation_crawler/documentation_crawler/spiders/common/spiders.py#L164'>tools/documentation_crawler/documentation_crawler/spiders/common/spiders.py:164:91:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/zulip/zulip/blob/2a65183991d84f8c4ceb016faff0ff340b665799/tools/documentation_crawler/documentation_crawler/spiders/common/spiders.py#L175'>tools/documentation_crawler/documentation_crawler/spiders/common/spiders.py:175:96:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/zulip/zulip/blob/2a65183991d84f8c4ceb016faff0ff340b665799/tools/lib/template_parser.py#L233'>tools/lib/template_parser.py:233:39:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/zulip/zulip/blob/2a65183991d84f8c4ceb016faff0ff340b665799/zerver/lib/cache.py#L231'>zerver/lib/cache.py:231:32:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/zulip/zulip/blob/2a65183991d84f8c4ceb016faff0ff340b665799/zerver/lib/markdown/__init__.py#L1274'>zerver/lib/markdown/__init__.py:1274:64:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/zulip/zulip/blob/2a65183991d84f8c4ceb016faff0ff340b665799/zerver/lib/markdown/__init__.py#L1714'>zerver/lib/markdown/__init__.py:1714:76:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/zulip/zulip/blob/2a65183991d84f8c4ceb016faff0ff340b665799/zerver/lib/markdown/__init__.py#L285'>zerver/lib/markdown/__init__.py:285:46:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/zulip/zulip/blob/2a65183991d84f8c4ceb016faff0ff340b665799/zerver/lib/markdown/__init__.py#L825'>zerver/lib/markdown/__init__.py:825:45:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/zulip/zulip/blob/2a65183991d84f8c4ceb016faff0ff340b665799/zerver/lib/markdown/api_arguments_table_generator.py#L106'>zerver/lib/markdown/api_arguments_table_generator.py:106:59:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/zulip/zulip/blob/2a65183991d84f8c4ceb016faff0ff340b665799/zerver/lib/markdown/api_return_values_table_generator.py#L62'>zerver/lib/markdown/api_return_values_table_generator.py:62:59:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/zulip/zulip/blob/2a65183991d84f8c4ceb016faff0ff340b665799/zerver/lib/markdown/help_emoticon_translations_table.py#L60'>zerver/lib/markdown/help_emoticon_translations_table.py:60:59:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/zulip/zulip/blob/2a65183991d84f8c4ceb016faff0ff340b665799/zerver/lib/markdown/help_relative_links.py#L206'>zerver/lib/markdown/help_relative_links.py:206:63:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/zulip/zulip/blob/2a65183991d84f8c4ceb016faff0ff340b665799/zerver/lib/markdown/help_settings_links.py#L169'>zerver/lib/markdown/help_settings_links.py:169:63:</a> E203 [*] Whitespace before ':'
... 23 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E203 | 137 | 0 | 137 | 0 | 0 |

</p>
</details>




---

```yaml
number: 8939
title: Consider alternate split points for line-too-long exemptions
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/split-points
created_at: 2023-12-01T03:32:27Z
updated_at: 2024-03-28T17:31:55Z
url: https://github.com/astral-sh/ruff/pull/8939
synced_at: 2026-01-10T22:47:01Z
```

# Consider alternate split points for line-too-long exemptions

---

_Pull request opened by @charliermarsh on 2023-12-01 03:32_

Looking for ecosystem results here to make a call.

See: https://github.com/astral-sh/ruff/issues/8383.

---

_Comment by @github-actions[bot] on 2023-12-01 03:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1674 -1 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/shared/nlu/training_data/entities_parser.py#L27'>rasa/shared/nlu/training_data/entities_parser.py:27:145:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_cli.py#L46'>tests/cli/test_cli.py:46:89:</a> E501 Line too long (96 > 88)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+39 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/example_dags/example_params_ui_tutorial.py#L98'>airflow/example_dags/example_params_ui_tutorial.py:98:111:</a> E501 Line too long (117 > 110)
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/providers/databricks/sensors/databricks_partition.py#L203'>airflow/providers/databricks/sensors/databricks_partition.py:203:111:</a> E501 Line too long (118 > 110)
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/providers/databricks/sensors/databricks_partition.py#L207'>airflow/providers/databricks/sensors/databricks_partition.py:207:111:</a> E501 Line too long (118 > 110)
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/providers/google/cloud/hooks/vertex_ai/auto_ml.py#L1394'>airflow/providers/google/cloud/hooks/vertex_ai/auto_ml.py:1394:111:</a> E501 Line too long (133 > 110)
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/providers/google/cloud/hooks/vertex_ai/auto_ml.py#L1396'>airflow/providers/google/cloud/hooks/vertex_ai/auto_ml.py:1396:111:</a> E501 Line too long (117 > 110)
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py#L2014'>airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py:2014:111:</a> E501 Line too long (123 > 110)
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py#L2094'>airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py:2094:111:</a> E501 Line too long (133 > 110)
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py#L2096'>airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py:2096:111:</a> E501 Line too long (117 > 110)
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py#L2155'>airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py:2155:111:</a> E501 Line too long (133 > 110)
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py#L2157'>airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py:2157:111:</a> E501 Line too long (117 > 110)
... 29 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+28 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/870c547f210b9dbd93abf75e7a0a1ec9b292c50b/samcli/hook_packages/terraform/hooks/prepare/resource_linking.py#L1967'>samcli/hook_packages/terraform/hooks/prepare/resource_linking.py:1967:121:</a> E501 Line too long (129 > 120)
+ <a href='https://github.com/aws/aws-sam-cli/blob/870c547f210b9dbd93abf75e7a0a1ec9b292c50b/tests/integration/init/schemas/schemas_test_data_setup.py#L151'>tests/integration/init/schemas/schemas_test_data_setup.py:151:121:</a> E501 Line too long (144 > 120)
+ <a href='https://github.com/aws/aws-sam-cli/blob/870c547f210b9dbd93abf75e7a0a1ec9b292c50b/tests/integration/init/schemas/schemas_test_data_setup.py#L155'>tests/integration/init/schemas/schemas_test_data_setup.py:155:121:</a> E501 Line too long (133 > 120)
+ <a href='https://github.com/aws/aws-sam-cli/blob/870c547f210b9dbd93abf75e7a0a1ec9b292c50b/tests/integration/init/schemas/schemas_test_data_setup.py#L156'>tests/integration/init/schemas/schemas_test_data_setup.py:156:121:</a> E501 Line too long (153 > 120)
+ <a href='https://github.com/aws/aws-sam-cli/blob/870c547f210b9dbd93abf75e7a0a1ec9b292c50b/tests/integration/init/schemas/schemas_test_data_setup.py#L157'>tests/integration/init/schemas/schemas_test_data_setup.py:157:121:</a> E501 Line too long (151 > 120)
+ <a href='https://github.com/aws/aws-sam-cli/blob/870c547f210b9dbd93abf75e7a0a1ec9b292c50b/tests/integration/init/schemas/schemas_test_data_setup.py#L158'>tests/integration/init/schemas/schemas_test_data_setup.py:158:121:</a> E501 Line too long (154 > 120)
+ <a href='https://github.com/aws/aws-sam-cli/blob/870c547f210b9dbd93abf75e7a0a1ec9b292c50b/tests/unit/hook_packages/terraform/hooks/prepare/test_resource_linking.py#L1992'>tests/unit/hook_packages/terraform/hooks/prepare/test_resource_linking.py:1992:121:</a> E501 Line too long (124 > 120)
+ <a href='https://github.com/aws/aws-sam-cli/blob/870c547f210b9dbd93abf75e7a0a1ec9b292c50b/tests/unit/hook_packages/terraform/hooks/prepare/test_resource_linking.py#L2040'>tests/unit/hook_packages/terraform/hooks/prepare/test_resource_linking.py:2040:121:</a> E501 Line too long (123 > 120)
+ <a href='https://github.com/aws/aws-sam-cli/blob/870c547f210b9dbd93abf75e7a0a1ec9b292c50b/tests/unit/hook_packages/terraform/hooks/prepare/test_resource_linking.py#L2041'>tests/unit/hook_packages/terraform/hooks/prepare/test_resource_linking.py:2041:121:</a> E501 Line too long (123 > 120)
+ <a href='https://github.com/aws/aws-sam-cli/blob/870c547f210b9dbd93abf75e7a0a1ec9b292c50b/tests/unit/hook_packages/terraform/hooks/prepare/test_resource_linking.py#L2594'>tests/unit/hook_packages/terraform/hooks/prepare/test_resource_linking.py:2594:121:</a> E501 Line too long (122 > 120)
... 18 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/models/maps.py#L10'>examples/models/maps.py:10:166:</a> E501 Line too long (797 > 165)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/commaai/openpilot/blob/e687be939e7cf67c4a75b87320ab058941d4316f/system/sensord/pigeond.py#L167'>system/sensord/pigeond.py:167:161:</a> E501 Line too long (207 > 160)
+ <a href='https://github.com/commaai/openpilot/blob/e687be939e7cf67c4a75b87320ab058941d4316f/system/sensord/pigeond.py#L172'>system/sensord/pigeond.py:172:161:</a> E501 Line too long (223 > 160)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/fronzbot/blinkpy/blob/32f3235c9f58294bb0ea8c6385220eff054eecb4/blinksync/blinksync.py#L94'>blinksync/blinksync.py:94:89:</a> E501 Line too long (114 > 88)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/b66396857b667ea8f191917d20f750764ea1219a/lnbits/nodes/lndrest.py#L155'>lnbits/nodes/lndrest.py:155:89:</a> E501 Line too long (92 > 88)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/f7c73a5f1aaf0724598e60c0cc5732604ec842a8/pandas/tests/util/test_hashing.py#L358'>pandas/tests/util/test_hashing.py:358:89:</a> E501 Line too long (523 > 88)
+ <a href='https://github.com/pandas-dev/pandas/blob/f7c73a5f1aaf0724598e60c0cc5732604ec842a8/pandas/tests/util/test_hashing.py#L359'>pandas/tests/util/test_hashing.py:359:89:</a> E501 Line too long (523 > 88)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/models/wheel.py#L17'>src/pip/_internal/models/wheel.py:17:89:</a> E501 Line too long (89 > 88)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1544 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/api/rest.py#L2467'>rotkehlchen/api/rest.py:2467:100:</a> E501 Line too long (102 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/api/rest.py#L2551'>rotkehlchen/api/rest.py:2551:100:</a> E501 Line too long (103 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/api/rest.py#L3198'>rotkehlchen/api/rest.py:3198:100:</a> E501 Line too long (101 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/api/rest.py#L3502'>rotkehlchen/api/rest.py:3502:100:</a> E501 Line too long (102 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/api/rest.py#L3519'>rotkehlchen/api/rest.py:3519:100:</a> E501 Line too long (102 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/api/rest.py#L4282'>rotkehlchen/api/rest.py:4282:100:</a> E501 Line too long (122 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/api/rest.py#L4298'>rotkehlchen/api/rest.py:4298:100:</a> E501 Line too long (121 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/api/v1/schemas.py#L1292'>rotkehlchen/api/v1/schemas.py:1292:100:</a> E501 Line too long (110 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/api/v1/schemas.py#L3017'>rotkehlchen/api/v1/schemas.py:3017:100:</a> E501 Line too long (101 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/arbitrum_one/modules/eas/decoder.py#L24'>rotkehlchen/chain/arbitrum_one/modules/eas/decoder.py:24:100:</a> E501 Line too long (108 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/arbitrum_one/node_inquirer.py#L55'>rotkehlchen/chain/arbitrum_one/node_inquirer.py:55:100:</a> E501 Line too long (119 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/arbitrum_one/node_inquirer.py#L56'>rotkehlchen/chain/arbitrum_one/node_inquirer.py:56:100:</a> E501 Line too long (114 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/base/modules/eas/decoder.py#L24'>rotkehlchen/chain/base/modules/eas/decoder.py:24:100:</a> E501 Line too long (108 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/base/node_inquirer.py#L55'>rotkehlchen/chain/base/node_inquirer.py:55:100:</a> E501 Line too long (119 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/base/node_inquirer.py#L56'>rotkehlchen/chain/base/node_inquirer.py:56:100:</a> E501 Line too long (114 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L154'>rotkehlchen/chain/ethereum/defi/price.py:154:100:</a> E501 Line too long (124 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L155'>rotkehlchen/chain/ethereum/defi/price.py:155:100:</a> E501 Line too long (124 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L162'>rotkehlchen/chain/ethereum/defi/price.py:162:100:</a> E501 Line too long (124 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L163'>rotkehlchen/chain/ethereum/defi/price.py:163:100:</a> E501 Line too long (124 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L171'>rotkehlchen/chain/ethereum/defi/price.py:171:100:</a> E501 Line too long (124 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L172'>rotkehlchen/chain/ethereum/defi/price.py:172:100:</a> E501 Line too long (124 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L179'>rotkehlchen/chain/ethereum/defi/price.py:179:100:</a> E501 Line too long (124 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L180'>rotkehlchen/chain/ethereum/defi/price.py:180:100:</a> E501 Line too long (124 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L192'>rotkehlchen/chain/ethereum/defi/price.py:192:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L200'>rotkehlchen/chain/ethereum/defi/price.py:200:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L219'>rotkehlchen/chain/ethereum/defi/price.py:219:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L227'>rotkehlchen/chain/ethereum/defi/price.py:227:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L235'>rotkehlchen/chain/ethereum/defi/price.py:235:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L243'>rotkehlchen/chain/ethereum/defi/price.py:243:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L251'>rotkehlchen/chain/ethereum/defi/price.py:251:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L259'>rotkehlchen/chain/ethereum/defi/price.py:259:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L267'>rotkehlchen/chain/ethereum/defi/price.py:267:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L275'>rotkehlchen/chain/ethereum/defi/price.py:275:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/convex/constants.py#L39'>rotkehlchen/chain/ethereum/modules/convex/constants.py:39:100:</a> E501 Line too long (120 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/convex/constants.py#L41'>rotkehlchen/chain/ethereum/modules/convex/constants.py:41:100:</a> E501 Line too long (112 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/convex/constants.py#L42'>rotkehlchen/chain/ethereum/modules/convex/constants.py:42:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/curve/decoder.py#L51'>rotkehlchen/chain/ethereum/modules/curve/decoder.py:51:100:</a> E501 Line too long (109 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/eas/decoder.py#L24'>rotkehlchen/chain/ethereum/modules/eas/decoder.py:24:100:</a> E501 Line too long (108 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/oneinch/v2/decoder.py#L30'>rotkehlchen/chain/ethereum/modules/oneinch/v2/decoder.py:30:100:</a> E501 Line too long (112 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/yearn/vaults.py#L182'>rotkehlchen/chain/ethereum/modules/yearn/vaults.py:182:100:</a> E501 Line too long (127 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/yearn/vaults.py#L188'>rotkehlchen/chain/ethereum/modules/yearn/vaults.py:188:100:</a> E501 Line too long (127 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/yearn/vaults.py#L194'>rotkehlchen/chain/ethereum/modules/yearn/vaults.py:194:100:</a> E501 Line too long (127 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/yearn/vaults.py#L200'>rotkehlchen/chain/ethereum/modules/yearn/vaults.py:200:100:</a> E501 Line too long (127 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/yearn/vaults.py#L206'>rotkehlchen/chain/ethereum/modules/yearn/vaults.py:206:100:</a> E501 Line too long (127 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/yearn/vaults.py#L212'>rotkehlchen/chain/ethereum/modules/yearn/vaults.py:212:100:</a> E501 Line too long (127 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/yearn/vaults.py#L218'>rotkehlchen/chain/ethereum/modules/yearn/vaults.py:218:100:</a> E501 Line too long (127 > 99)
... 1498 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+54 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/569c364392d8201c6236767925c66b2ca24f742e/corporate/lib/stripe.py#L2214'>corporate/lib/stripe.py:2214:101:</a> E501 Line too long (107 > 100)
+ <a href='https://github.com/zulip/zulip/blob/569c364392d8201c6236767925c66b2ca24f742e/corporate/lib/stripe.py#L825'>corporate/lib/stripe.py:825:101:</a> E501 Line too long (117 > 100)
+ <a href='https://github.com/zulip/zulip/blob/569c364392d8201c6236767925c66b2ca24f742e/corporate/lib/stripe.py#L849'>corporate/lib/stripe.py:849:101:</a> E501 Line too long (117 > 100)
+ <a href='https://github.com/zulip/zulip/blob/569c364392d8201c6236767925c66b2ca24f742e/corporate/migrations/0018_customer_cloud_xor_self_hosted.py#L10'>corporate/migrations/0018_customer_cloud_xor_self_hosted.py:10:101:</a> E501 Line too long (106 > 100)
+ <a href='https://github.com/zulip/zulip/blob/569c364392d8201c6236767925c66b2ca24f742e/corporate/views/remote_billing_page.py#L70'>corporate/views/remote_billing_page.py:70:101:</a> E501 Line too long (110 > 100)
+ <a href='https://github.com/zulip/zulip/blob/569c364392d8201c6236767925c66b2ca24f742e/zerver/actions/message_send.py#L1058'>zerver/actions/message_send.py:1058:101:</a> E501 Line too long (129 > 100)
+ <a href='https://github.com/zulip/zulip/blob/569c364392d8201c6236767925c66b2ca24f742e/zerver/actions/message_send.py#L1059'>zerver/actions/message_send.py:1059:101:</a> E501 Line too long (131 > 100)
+ <a href='https://github.com/zulip/zulip/blob/569c364392d8201c6236767925c66b2ca24f742e/zerver/actions/message_send.py#L494'>zerver/actions/message_send.py:494:101:</a> E501 Line too long (108 > 100)
+ <a href='https://github.com/zulip/zulip/blob/569c364392d8201c6236767925c66b2ca24f742e/zerver/actions/message_send.py#L495'>zerver/actions/message_send.py:495:101:</a> E501 Line too long (110 > 100)
+ <a href='https://github.com/zulip/zulip/blob/569c364392d8201c6236767925c66b2ca24f742e/zerver/actions/message_send.py#L705'>zerver/actions/message_send.py:705:101:</a> E501 Line too long (108 > 100)
... 44 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E501 | 1674 | 1674 | 0 | 0 | 0 |
| RUF100 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1674 -1 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/rasa/shared/nlu/training_data/entities_parser.py#L27'>rasa/shared/nlu/training_data/entities_parser.py:27:145:</a> RUF100 [*] Unused `noqa` directive (unused: `E501`)
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/tests/cli/test_cli.py#L46'>tests/cli/test_cli.py:46:89:</a> E501 Line too long (96 > 88)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+39 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/example_dags/example_params_ui_tutorial.py#L98'>airflow/example_dags/example_params_ui_tutorial.py:98:111:</a> E501 Line too long (117 > 110)
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/providers/databricks/sensors/databricks_partition.py#L203'>airflow/providers/databricks/sensors/databricks_partition.py:203:111:</a> E501 Line too long (118 > 110)
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/providers/databricks/sensors/databricks_partition.py#L207'>airflow/providers/databricks/sensors/databricks_partition.py:207:111:</a> E501 Line too long (118 > 110)
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/providers/google/cloud/hooks/vertex_ai/auto_ml.py#L1394'>airflow/providers/google/cloud/hooks/vertex_ai/auto_ml.py:1394:111:</a> E501 Line too long (133 > 110)
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/providers/google/cloud/hooks/vertex_ai/auto_ml.py#L1396'>airflow/providers/google/cloud/hooks/vertex_ai/auto_ml.py:1396:111:</a> E501 Line too long (117 > 110)
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py#L2014'>airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py:2014:111:</a> E501 Line too long (123 > 110)
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py#L2094'>airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py:2094:111:</a> E501 Line too long (133 > 110)
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py#L2096'>airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py:2096:111:</a> E501 Line too long (117 > 110)
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py#L2155'>airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py:2155:111:</a> E501 Line too long (133 > 110)
+ <a href='https://github.com/apache/airflow/blob/cf052dc64f00e851427a41a34ffe576fd39be51b/airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py#L2157'>airflow/providers/google/cloud/hooks/vertex_ai/custom_job.py:2157:111:</a> E501 Line too long (117 > 110)
... 29 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+28 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/870c547f210b9dbd93abf75e7a0a1ec9b292c50b/samcli/hook_packages/terraform/hooks/prepare/resource_linking.py#L1967'>samcli/hook_packages/terraform/hooks/prepare/resource_linking.py:1967:121:</a> E501 Line too long (129 > 120)
+ <a href='https://github.com/aws/aws-sam-cli/blob/870c547f210b9dbd93abf75e7a0a1ec9b292c50b/tests/integration/init/schemas/schemas_test_data_setup.py#L151'>tests/integration/init/schemas/schemas_test_data_setup.py:151:121:</a> E501 Line too long (144 > 120)
+ <a href='https://github.com/aws/aws-sam-cli/blob/870c547f210b9dbd93abf75e7a0a1ec9b292c50b/tests/integration/init/schemas/schemas_test_data_setup.py#L155'>tests/integration/init/schemas/schemas_test_data_setup.py:155:121:</a> E501 Line too long (133 > 120)
+ <a href='https://github.com/aws/aws-sam-cli/blob/870c547f210b9dbd93abf75e7a0a1ec9b292c50b/tests/integration/init/schemas/schemas_test_data_setup.py#L156'>tests/integration/init/schemas/schemas_test_data_setup.py:156:121:</a> E501 Line too long (153 > 120)
+ <a href='https://github.com/aws/aws-sam-cli/blob/870c547f210b9dbd93abf75e7a0a1ec9b292c50b/tests/integration/init/schemas/schemas_test_data_setup.py#L157'>tests/integration/init/schemas/schemas_test_data_setup.py:157:121:</a> E501 Line too long (151 > 120)
+ <a href='https://github.com/aws/aws-sam-cli/blob/870c547f210b9dbd93abf75e7a0a1ec9b292c50b/tests/integration/init/schemas/schemas_test_data_setup.py#L158'>tests/integration/init/schemas/schemas_test_data_setup.py:158:121:</a> E501 Line too long (154 > 120)
+ <a href='https://github.com/aws/aws-sam-cli/blob/870c547f210b9dbd93abf75e7a0a1ec9b292c50b/tests/unit/hook_packages/terraform/hooks/prepare/test_resource_linking.py#L1992'>tests/unit/hook_packages/terraform/hooks/prepare/test_resource_linking.py:1992:121:</a> E501 Line too long (124 > 120)
+ <a href='https://github.com/aws/aws-sam-cli/blob/870c547f210b9dbd93abf75e7a0a1ec9b292c50b/tests/unit/hook_packages/terraform/hooks/prepare/test_resource_linking.py#L2040'>tests/unit/hook_packages/terraform/hooks/prepare/test_resource_linking.py:2040:121:</a> E501 Line too long (123 > 120)
+ <a href='https://github.com/aws/aws-sam-cli/blob/870c547f210b9dbd93abf75e7a0a1ec9b292c50b/tests/unit/hook_packages/terraform/hooks/prepare/test_resource_linking.py#L2041'>tests/unit/hook_packages/terraform/hooks/prepare/test_resource_linking.py:2041:121:</a> E501 Line too long (123 > 120)
+ <a href='https://github.com/aws/aws-sam-cli/blob/870c547f210b9dbd93abf75e7a0a1ec9b292c50b/tests/unit/hook_packages/terraform/hooks/prepare/test_resource_linking.py#L2594'>tests/unit/hook_packages/terraform/hooks/prepare/test_resource_linking.py:2594:121:</a> E501 Line too long (122 > 120)
... 18 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/examples/models/maps.py#L10'>examples/models/maps.py:10:166:</a> E501 Line too long (797 > 165)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/commaai/openpilot/blob/e687be939e7cf67c4a75b87320ab058941d4316f/system/sensord/pigeond.py#L167'>system/sensord/pigeond.py:167:161:</a> E501 Line too long (207 > 160)
+ <a href='https://github.com/commaai/openpilot/blob/e687be939e7cf67c4a75b87320ab058941d4316f/system/sensord/pigeond.py#L172'>system/sensord/pigeond.py:172:161:</a> E501 Line too long (223 > 160)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/fronzbot/blinkpy/blob/32f3235c9f58294bb0ea8c6385220eff054eecb4/blinksync/blinksync.py#L94'>blinksync/blinksync.py:94:89:</a> E501 Line too long (114 > 88)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/b66396857b667ea8f191917d20f750764ea1219a/lnbits/nodes/lndrest.py#L155'>lnbits/nodes/lndrest.py:155:89:</a> E501 Line too long (92 > 88)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/f7c73a5f1aaf0724598e60c0cc5732604ec842a8/pandas/tests/util/test_hashing.py#L358'>pandas/tests/util/test_hashing.py:358:89:</a> E501 Line too long (523 > 88)
+ <a href='https://github.com/pandas-dev/pandas/blob/f7c73a5f1aaf0724598e60c0cc5732604ec842a8/pandas/tests/util/test_hashing.py#L359'>pandas/tests/util/test_hashing.py:359:89:</a> E501 Line too long (523 > 88)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/models/wheel.py#L17'>src/pip/_internal/models/wheel.py:17:89:</a> E501 Line too long (89 > 88)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1544 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/api/rest.py#L2467'>rotkehlchen/api/rest.py:2467:100:</a> E501 Line too long (102 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/api/rest.py#L2551'>rotkehlchen/api/rest.py:2551:100:</a> E501 Line too long (103 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/api/rest.py#L3198'>rotkehlchen/api/rest.py:3198:100:</a> E501 Line too long (101 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/api/rest.py#L3502'>rotkehlchen/api/rest.py:3502:100:</a> E501 Line too long (102 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/api/rest.py#L3519'>rotkehlchen/api/rest.py:3519:100:</a> E501 Line too long (102 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/api/rest.py#L4282'>rotkehlchen/api/rest.py:4282:100:</a> E501 Line too long (122 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/api/rest.py#L4298'>rotkehlchen/api/rest.py:4298:100:</a> E501 Line too long (121 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/api/v1/schemas.py#L1292'>rotkehlchen/api/v1/schemas.py:1292:100:</a> E501 Line too long (110 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/api/v1/schemas.py#L3017'>rotkehlchen/api/v1/schemas.py:3017:100:</a> E501 Line too long (101 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/arbitrum_one/modules/eas/decoder.py#L24'>rotkehlchen/chain/arbitrum_one/modules/eas/decoder.py:24:100:</a> E501 Line too long (108 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/arbitrum_one/node_inquirer.py#L55'>rotkehlchen/chain/arbitrum_one/node_inquirer.py:55:100:</a> E501 Line too long (119 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/arbitrum_one/node_inquirer.py#L56'>rotkehlchen/chain/arbitrum_one/node_inquirer.py:56:100:</a> E501 Line too long (114 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/base/modules/eas/decoder.py#L24'>rotkehlchen/chain/base/modules/eas/decoder.py:24:100:</a> E501 Line too long (108 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/base/node_inquirer.py#L55'>rotkehlchen/chain/base/node_inquirer.py:55:100:</a> E501 Line too long (119 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/base/node_inquirer.py#L56'>rotkehlchen/chain/base/node_inquirer.py:56:100:</a> E501 Line too long (114 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L154'>rotkehlchen/chain/ethereum/defi/price.py:154:100:</a> E501 Line too long (124 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L155'>rotkehlchen/chain/ethereum/defi/price.py:155:100:</a> E501 Line too long (124 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L162'>rotkehlchen/chain/ethereum/defi/price.py:162:100:</a> E501 Line too long (124 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L163'>rotkehlchen/chain/ethereum/defi/price.py:163:100:</a> E501 Line too long (124 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L171'>rotkehlchen/chain/ethereum/defi/price.py:171:100:</a> E501 Line too long (124 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L172'>rotkehlchen/chain/ethereum/defi/price.py:172:100:</a> E501 Line too long (124 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L179'>rotkehlchen/chain/ethereum/defi/price.py:179:100:</a> E501 Line too long (124 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L180'>rotkehlchen/chain/ethereum/defi/price.py:180:100:</a> E501 Line too long (124 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L192'>rotkehlchen/chain/ethereum/defi/price.py:192:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L200'>rotkehlchen/chain/ethereum/defi/price.py:200:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L219'>rotkehlchen/chain/ethereum/defi/price.py:219:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L227'>rotkehlchen/chain/ethereum/defi/price.py:227:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L235'>rotkehlchen/chain/ethereum/defi/price.py:235:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L243'>rotkehlchen/chain/ethereum/defi/price.py:243:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L251'>rotkehlchen/chain/ethereum/defi/price.py:251:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L259'>rotkehlchen/chain/ethereum/defi/price.py:259:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L267'>rotkehlchen/chain/ethereum/defi/price.py:267:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/defi/price.py#L275'>rotkehlchen/chain/ethereum/defi/price.py:275:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/convex/constants.py#L39'>rotkehlchen/chain/ethereum/modules/convex/constants.py:39:100:</a> E501 Line too long (120 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/convex/constants.py#L41'>rotkehlchen/chain/ethereum/modules/convex/constants.py:41:100:</a> E501 Line too long (112 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/convex/constants.py#L42'>rotkehlchen/chain/ethereum/modules/convex/constants.py:42:100:</a> E501 Line too long (118 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/curve/decoder.py#L51'>rotkehlchen/chain/ethereum/modules/curve/decoder.py:51:100:</a> E501 Line too long (109 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/eas/decoder.py#L24'>rotkehlchen/chain/ethereum/modules/eas/decoder.py:24:100:</a> E501 Line too long (108 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/oneinch/v2/decoder.py#L30'>rotkehlchen/chain/ethereum/modules/oneinch/v2/decoder.py:30:100:</a> E501 Line too long (112 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/yearn/vaults.py#L182'>rotkehlchen/chain/ethereum/modules/yearn/vaults.py:182:100:</a> E501 Line too long (127 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/yearn/vaults.py#L188'>rotkehlchen/chain/ethereum/modules/yearn/vaults.py:188:100:</a> E501 Line too long (127 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/yearn/vaults.py#L194'>rotkehlchen/chain/ethereum/modules/yearn/vaults.py:194:100:</a> E501 Line too long (127 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/yearn/vaults.py#L200'>rotkehlchen/chain/ethereum/modules/yearn/vaults.py:200:100:</a> E501 Line too long (127 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/yearn/vaults.py#L206'>rotkehlchen/chain/ethereum/modules/yearn/vaults.py:206:100:</a> E501 Line too long (127 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/yearn/vaults.py#L212'>rotkehlchen/chain/ethereum/modules/yearn/vaults.py:212:100:</a> E501 Line too long (127 > 99)
+ <a href='https://github.com/rotki/rotki/blob/9cedbedeea7b96b2be699f48febddfcc87b30f26/rotkehlchen/chain/ethereum/modules/yearn/vaults.py#L218'>rotkehlchen/chain/ethereum/modules/yearn/vaults.py:218:100:</a> E501 Line too long (127 > 99)
... 1498 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+54 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/569c364392d8201c6236767925c66b2ca24f742e/corporate/lib/stripe.py#L2214'>corporate/lib/stripe.py:2214:101:</a> E501 Line too long (107 > 100)
+ <a href='https://github.com/zulip/zulip/blob/569c364392d8201c6236767925c66b2ca24f742e/corporate/lib/stripe.py#L825'>corporate/lib/stripe.py:825:101:</a> E501 Line too long (117 > 100)
+ <a href='https://github.com/zulip/zulip/blob/569c364392d8201c6236767925c66b2ca24f742e/corporate/lib/stripe.py#L849'>corporate/lib/stripe.py:849:101:</a> E501 Line too long (117 > 100)
+ <a href='https://github.com/zulip/zulip/blob/569c364392d8201c6236767925c66b2ca24f742e/corporate/migrations/0018_customer_cloud_xor_self_hosted.py#L10'>corporate/migrations/0018_customer_cloud_xor_self_hosted.py:10:101:</a> E501 Line too long (106 > 100)
+ <a href='https://github.com/zulip/zulip/blob/569c364392d8201c6236767925c66b2ca24f742e/corporate/views/remote_billing_page.py#L70'>corporate/views/remote_billing_page.py:70:101:</a> E501 Line too long (110 > 100)
+ <a href='https://github.com/zulip/zulip/blob/569c364392d8201c6236767925c66b2ca24f742e/zerver/actions/message_send.py#L1058'>zerver/actions/message_send.py:1058:101:</a> E501 Line too long (129 > 100)
+ <a href='https://github.com/zulip/zulip/blob/569c364392d8201c6236767925c66b2ca24f742e/zerver/actions/message_send.py#L1059'>zerver/actions/message_send.py:1059:101:</a> E501 Line too long (131 > 100)
+ <a href='https://github.com/zulip/zulip/blob/569c364392d8201c6236767925c66b2ca24f742e/zerver/actions/message_send.py#L494'>zerver/actions/message_send.py:494:101:</a> E501 Line too long (108 > 100)
+ <a href='https://github.com/zulip/zulip/blob/569c364392d8201c6236767925c66b2ca24f742e/zerver/actions/message_send.py#L495'>zerver/actions/message_send.py:495:101:</a> E501 Line too long (110 > 100)
+ <a href='https://github.com/zulip/zulip/blob/569c364392d8201c6236767925c66b2ca24f742e/zerver/actions/message_send.py#L705'>zerver/actions/message_send.py:705:101:</a> E501 Line too long (108 > 100)
... 44 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E501 | 1674 | 1674 | 0 | 0 | 0 |
| RUF100 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2023-12-01 04:42_

I don't love what I'm seeing from the ecosystem checks. Some of these changes are "good"... I could use a second pair of eyes.

---

_Review comment by @T-256 on `crates/ruff_linter/src/rules/pycodestyle/overlong.rs`:73 on 2023-12-01 10:21_

Did you consider expanding these characters?

---

_@T-256 reviewed on 2023-12-01 10:21_

---

_Comment by @T-256 on 2023-12-01 10:23_

> I don't love what I'm seeing from the ecosystem checks

Overall good changes. which types you didn't like?

---

_Comment by @MichaReiser on 2023-12-05 05:12_

> I don't love what I'm seeing from the ecosystem checks. Some of these changes are "good"... I could use a second pair of eyes.

Could you share a few examples for the cases you dislike? 

---

_Comment by @charliermarsh on 2023-12-05 05:17_

- https://github.com/commaai/openpilot/blob/e687be939e7cf67c4a75b87320ab058941d4316f/system/sensord/pigeond.py#L172
- https://github.com/pandas-dev/pandas/blob/f7c73a5f1aaf0724598e60c0cc5732604ec842a8/pandas/tests/util/test_hashing.py#L359
- https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/src/pip/_internal/models/wheel.py#L17
- https://github.com/aws/aws-sam-cli/blob/870c547f210b9dbd93abf75e7a0a1ec9b292c50b/tests/integration/init/schemas/schemas_test_data_setup.py#L156

---

_Comment by @MichaReiser on 2023-12-05 05:51_

Yeah, this doesn't look ideal. I like the examples where there's some python code, e.g. a call that passes a long string as an argument. But splitting some of the strings would lead to worse formatting. 

Could we rely on the lexed tokens of the line to identify if it is a single string literal that can't be split or if there are any other Python tokens that are valid split points?

---

_Comment by @MichaReiser on 2024-03-28 17:31_

I'll close this PR because there hasn't been any activity in a while. Feel free to reopen if you plan to continue working on this.

---

_Closed by @MichaReiser on 2024-03-28 17:31_

---

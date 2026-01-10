```yaml
number: 10804
title: "Add comment test for `FURB110`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: charlie/fu
created_at: 2024-04-06T16:42:39Z
updated_at: 2024-04-06T16:55:34Z
url: https://github.com/astral-sh/ruff/pull/10804
synced_at: 2026-01-10T22:47:03Z
```

# Add comment test for `FURB110`

---

_Pull request opened by @charliermarsh on 2024-04-06 16:42_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2024-04-06 16:42_

---

_Label `testing` added by @charliermarsh on 2024-04-06 16:42_

---

_Merged by @charliermarsh on 2024-04-06 16:49_

---

_Closed by @charliermarsh on 2024-04-06 16:49_

---

_Branch deleted on 2024-04-06 16:49_

---

_Comment by @github-actions[bot] on 2024-04-06 16:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+91 -0 violations, +0 -0 fixes in 5 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+49 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/api_connexion/endpoints/pool_endpoint.py#L121'>airflow/api_connexion/endpoints/pool_endpoint.py:121:17:</a> FURB110 Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/cli/commands/dag_command.py#L430'>airflow/cli/commands/dag_command.py:430:12:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/configuration.py#L1446'>airflow/configuration.py:1446:13:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/amazon/aws/hooks/base_aws.py#L540'>airflow/providers/amazon/aws/hooks/base_aws.py:540:16:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/amazon/aws/hooks/base_aws.py#L885'>airflow/providers/amazon/aws/hooks/base_aws.py:885:20:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/amazon/aws/hooks/redshift_cluster.py#L120'>airflow/providers/amazon/aws/hooks/redshift_cluster.py:120:16:</a> FURB110 Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/amazon/aws/hooks/redshift_cluster.py#L150'>airflow/providers/amazon/aws/hooks/redshift_cluster.py:150:16:</a> FURB110 Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/amazon/aws/hooks/redshift_cluster.py#L178'>airflow/providers/amazon/aws/hooks/redshift_cluster.py:178:16:</a> FURB110 Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/apache/livy/hooks/livy.py#L580'>airflow/providers/apache/livy/hooks/livy.py:580:22:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/apache/livy/hooks/livy.py#L581'>airflow/providers/apache/livy/hooks/livy.py:581:20:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/apache/spark/hooks/spark_submit.py#L251'>airflow/providers/apache/spark/hooks/spark_submit.py:251:34:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/apache/spark/hooks/spark_submit.py#L252'>airflow/providers/apache/spark/hooks/spark_submit.py:252:40:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py#L371'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py:371:17:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/cncf/kubernetes/utils/pod_manager.py#L261'>airflow/providers/cncf/kubernetes/utils/pod_manager.py:261:16:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/dingding/hooks/dingding.py#L96'>airflow/providers/dingding/hooks/dingding.py:96:25:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/google/cloud/hooks/compute_ssh.py#L187'>airflow/providers/google/cloud/hooks/compute_ssh.py:187:25:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/google/cloud/hooks/compute_ssh.py#L189'>airflow/providers/google/cloud/hooks/compute_ssh.py:189:29:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/google/cloud/log/gcs_task_handler.py#L128'>airflow/providers/google/cloud/log/gcs_task_handler.py:128:21:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/google/cloud/transfers/gcs_to_bigquery.py#L498'>airflow/providers/google/cloud/transfers/gcs_to_bigquery.py:498:30:</a> FURB110 Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/google/cloud/transfers/sftp_to_gcs.py#L140'>airflow/providers/google/cloud/transfers/sftp_to_gcs.py:140:17:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/google/cloud/utils/credentials_provider.py#L304'>airflow/providers/google/cloud/utils/credentials_provider.py:304:24:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/hashicorp/hooks/vault.py#L216'>airflow/providers/hashicorp/hooks/vault.py:216:23:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/http/hooks/http.py#L114'>airflow/providers/http/hooks/http.py:114:26:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/http/hooks/http.py#L115'>airflow/providers/http/hooks/http.py:115:24:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/http/hooks/http.py#L349'>airflow/providers/http/hooks/http.py:349:26:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/apache/airflow/blob/4d65f216dec5705f82262d9c4a9f799e92b55944/airflow/providers/http/hooks/http.py#L350'>airflow/providers/http/hooks/http.py:350:24:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
... 23 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/resources.py#L617'>src/bokeh/resources.py:617:19:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/settings.py#L374'>src/bokeh/settings.py:374:25:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L474'>src/bokeh/util/compiler.py:474:12:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/examples.py#L169'>tests/support/util/examples.py:169:22:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/examples.py#L175'>tests/support/util/examples.py:175:26:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/examples.py#L178'>tests/support/util/examples.py:178:26:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+23 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/d4caff8d2df144bf7ffa4026efc6bfb062306c9d/rotkehlchen/accounting/history_base_entries.py#L122'>rotkehlchen/accounting/history_base_entries.py:122:19:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/rotki/rotki/blob/d4caff8d2df144bf7ffa4026efc6bfb062306c9d/rotkehlchen/accounting/history_base_entries.py#L173'>rotkehlchen/accounting/history_base_entries.py:173:19:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/rotki/rotki/blob/d4caff8d2df144bf7ffa4026efc6bfb062306c9d/rotkehlchen/accounting/history_base_entries.py#L186'>rotkehlchen/accounting/history_base_entries.py:186:22:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/rotki/rotki/blob/d4caff8d2df144bf7ffa4026efc6bfb062306c9d/rotkehlchen/api/v1/resources.py#L2396'>rotkehlchen/api/v1/resources.py:2396:24:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/rotki/rotki/blob/d4caff8d2df144bf7ffa4026efc6bfb062306c9d/rotkehlchen/api/v1/resources.py#L2750'>rotkehlchen/api/v1/resources.py:2750:20:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/rotki/rotki/blob/d4caff8d2df144bf7ffa4026efc6bfb062306c9d/rotkehlchen/api/v1/resources.py#L2809'>rotkehlchen/api/v1/resources.py:2809:41:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/rotki/rotki/blob/d4caff8d2df144bf7ffa4026efc6bfb062306c9d/rotkehlchen/api/v1/resources.py#L2810'>rotkehlchen/api/v1/resources.py:2810:47:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/rotki/rotki/blob/d4caff8d2df144bf7ffa4026efc6bfb062306c9d/rotkehlchen/chain/arbitrum_one/modules/gmx/balances.py#L155'>rotkehlchen/chain/arbitrum_one/modules/gmx/balances.py:155:29:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/rotki/rotki/blob/d4caff8d2df144bf7ffa4026efc6bfb062306c9d/rotkehlchen/chain/avalanche/manager.py#L95'>rotkehlchen/chain/avalanche/manager.py:95:30:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/rotki/rotki/blob/d4caff8d2df144bf7ffa4026efc6bfb062306c9d/rotkehlchen/chain/bitcoin/xpub.py#L53'>rotkehlchen/chain/bitcoin/xpub.py:53:39:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/rotki/rotki/blob/d4caff8d2df144bf7ffa4026efc6bfb062306c9d/rotkehlchen/chain/evm/accounting/interfaces.py#L113'>rotkehlchen/chain/evm/accounting/interfaces.py:113:23:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/rotki/rotki/blob/d4caff8d2df144bf7ffa4026efc6bfb062306c9d/rotkehlchen/chain/evm/contracts.py#L100'>rotkehlchen/chain/evm/contracts.py:100:18:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
... 11 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/lib/events.py#L697'>zerver/lib/events.py:697:34:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/webhooks/bitbucket3/view.py#L269'>zerver/webhooks/bitbucket3/view.py:269:18:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/zulip/zulip/blob/f4d109c289f614273b43b411cbd8d1fad128842e/zerver/webhooks/taiga/view.py#L345'>zerver/webhooks/taiga/view.py:345:22:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/367d9abbec32c3aba1d13901ac67b04923d21144/indico/modules/designer/controllers.py#L203'>indico/modules/designer/controllers.py:203:23:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/indico/indico/blob/367d9abbec32c3aba1d13901ac67b04923d21144/indico/modules/designer/models/templates.py#L148'>indico/modules/designer/models/templates.py:148:16:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/indico/indico/blob/367d9abbec32c3aba1d13901ac67b04923d21144/indico/modules/events/abstracts/controllers/abstract_list.py#L243'>indico/modules/events/abstracts/controllers/abstract_list.py:243:30:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/indico/indico/blob/367d9abbec32c3aba1d13901ac67b04923d21144/indico/modules/events/abstracts/util.py#L91'>indico/modules/events/abstracts/util.py:91:56:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/indico/indico/blob/367d9abbec32c3aba1d13901ac67b04923d21144/indico/modules/events/contributions/util.py#L107'>indico/modules/events/contributions/util.py:107:63:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/indico/indico/blob/367d9abbec32c3aba1d13901ac67b04923d21144/indico/modules/events/contributions/util.py#L110'>indico/modules/events/contributions/util.py:110:63:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/indico/indico/blob/367d9abbec32c3aba1d13901ac67b04923d21144/indico/modules/events/registration/models/forms.py#L404'>indico/modules/events/registration/models/forms.py:404:18:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/indico/indico/blob/367d9abbec32c3aba1d13901ac67b04923d21144/indico/modules/events/tracks/models/tracks.py#L118'>indico/modules/events/tracks/models/tracks.py:118:16:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/indico/indico/blob/367d9abbec32c3aba1d13901ac67b04923d21144/indico/modules/receipts/models/templates.py#L92'>indico/modules/receipts/models/templates.py:92:16:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
+ <a href='https://github.com/indico/indico/blob/367d9abbec32c3aba1d13901ac67b04923d21144/indico/web/forms/fields/principals.py#L201'>indico/web/forms/fields/principals.py:201:16:</a> FURB110 [*] Replace ternary `if` expression with `or` operator
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB110 | 91 | 91 | 0 | 0 | 0 |

</p>
</details>




---

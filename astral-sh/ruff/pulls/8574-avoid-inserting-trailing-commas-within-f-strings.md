```yaml
number: 8574
title: Avoid inserting trailing commas within f-strings
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/com
created_at: 2023-11-09T04:11:06Z
updated_at: 2023-11-09T04:51:04Z
url: https://github.com/astral-sh/ruff/pull/8574
synced_at: 2026-01-12T15:55:26Z
```

# Avoid inserting trailing commas within f-strings

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/8556.

---

_Label `bug` added by @charliermarsh on 2023-11-09 04:11_

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-11-09 04:11_

---

_@charliermarsh reviewed on 2023-11-09 04:13_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/flake8_commas/COM81.py`:45 on 2023-11-09 04:13_

My editor is stripping this.

---

_@dhruvmanila approved on 2023-11-09 04:22_

Thanks!

Although this does mean that we wouldn't support similar expressions inside the f-string. This could be a candidate to move the rule to AST checker. We can then use the lexer to check for missing commas for each possible expression.

---

_Comment by @charliermarsh on 2023-11-09 04:25_

👍 Yeah, I'm not sure we can feasibly support linting within f-strings for this rule as long as it's tokenized.

---

_Merged by @charliermarsh on 2023-11-09 04:25_

---

_Closed by @charliermarsh on 2023-11-09 04:25_

---

_Branch deleted on 2023-11-09 04:25_

---

_Comment by @github-actions[bot] on 2023-11-09 04:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+649 -1017 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+353 -847 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/api/common/trigger_dag.py#L70'>airflow/api/common/trigger_dag.py:70:78:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/api_connexion/endpoints/config_endpoint.py#L122'>airflow/api_connexion/endpoints/config_endpoint.py:122:103:</a> COM812 [*] Trailing comma missing
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/api_connexion/endpoints/config_endpoint.py#L122'>airflow/api_connexion/endpoints/config_endpoint.py:122:45:</a> COM812 [*] Trailing comma missing
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/api_connexion/endpoints/task_instance_endpoint.py#L387'>airflow/api_connexion/endpoints/task_instance_endpoint.py:387:24:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/api_connexion/endpoints/task_instance_endpoint.py#L387'>airflow/api_connexion/endpoints/task_instance_endpoint.py:387:95:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/api_connexion/endpoints/task_instance_endpoint.py#L535'>airflow/api_connexion/endpoints/task_instance_endpoint.py:535:102:</a> COM812 [*] Trailing comma missing
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/api_connexion/endpoints/task_instance_endpoint.py#L535'>airflow/api_connexion/endpoints/task_instance_endpoint.py:535:20:</a> COM812 [*] Trailing comma missing
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/api_connexion/parameters.py#L121'>airflow/api_connexion/parameters.py:121:20:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/api_connexion/parameters.py#L122'>airflow/api_connexion/parameters.py:122:57:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/api_internal/internal_api_call.py#L106'>airflow/api_internal/internal_api_call.py:106:103:</a> COM812 [*] Trailing comma missing
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/auth/managers/fab/api_endpoints/role_and_permission_endpoint.py#L83'>airflow/auth/managers/fab/api_endpoints/role_and_permission_endpoint.py:83:20:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/auth/managers/fab/api_endpoints/role_and_permission_endpoint.py#L84'>airflow/auth/managers/fab/api_endpoints/role_and_permission_endpoint.py:84:57:</a> COM812 [*] Trailing comma missing
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/auth/managers/fab/api_endpoints/user_endpoint.py#L78'>airflow/auth/managers/fab/api_endpoints/user_endpoint.py:78:20:</a> COM812 [*] Trailing comma missing
... 965 additional changes omitted for rule COM812
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/cli/cli_config.py#L69'>airflow/cli/cli_config.py:69:20:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/cli/commands/db_command.py#L214'>airflow/cli/commands/db_command.py:214:41:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/hooks/filesystem.py#L86'>airflow/hooks/filesystem.py:86:29:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/hooks/filesystem.py#L87'>airflow/hooks/filesystem.py:87:24:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/hooks/package_index.py#L92'>airflow/hooks/package_index.py:92:25:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/hooks/package_index.py#L94'>airflow/hooks/package_index.py:94:20:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/amazon/aws/hooks/base_aws.py#L866'>airflow/providers/amazon/aws/hooks/base_aws.py:866:25:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/amazon/aws/operators/emr.py#L1229'>airflow/providers/amazon/aws/operators/emr.py:1229:51:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/apache/beam/hooks/beam.py#L191'>airflow/providers/apache/beam/hooks/beam.py:191:31:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/apache/beam/hooks/beam.py#L507'>airflow/providers/apache/beam/hooks/beam.py:507:31:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/apache/hive/hooks/hive.py#L262'>airflow/providers/apache/hive/hooks/hive.py:262:53:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/apache/hive/hooks/hive.py#L265'>airflow/providers/apache/hive/hooks/hive.py:265:53:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/apache/spark/hooks/spark_submit.py#L276'>airflow/providers/apache/spark/hooks/spark_submit.py:276:40:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/apache/sqoop/hooks/sqoop.py#L125'>airflow/providers/apache/sqoop/hooks/sqoop.py:125:36:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/cncf/kubernetes/decorators/kubernetes.py#L75'>airflow/providers/cncf/kubernetes/decorators/kubernetes.py:75:35:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/cncf/kubernetes/pod_launcher_deprecated.py#L283'>airflow/providers/cncf/kubernetes/pod_launcher_deprecated.py:283:49:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/cncf/kubernetes/utils/delete_from.py#L133'>airflow/providers/cncf/kubernetes/utils/delete_from.py:133:23:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/cncf/kubernetes/utils/delete_from.py#L134'>airflow/providers/cncf/kubernetes/utils/delete_from.py:134:31:</a> COM819 [*] Trailing comma prohibited
... 130 additional changes omitted for rule COM819
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/slack/hooks/slack.py#L227'>airflow/providers/slack/hooks/slack.py:227:25:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/slack/hooks/slack.py#L231'>airflow/providers/slack/hooks/slack.py:231:24:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/tabular/hooks/tabular.py#L74'>airflow/providers/tabular/hooks/tabular.py:74:25:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/dev/breeze/src/airflow_breeze/commands/ci_image_commands.py#L640'>dev/breeze/src/airflow_breeze/commands/ci_image_commands.py:640:43:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L1055'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:1055:29:</a> COM818 Trailing comma on bare tuple prohibited
... 1164 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+43 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/examples/output/apis/autoload_static_flask.py#L27'>examples/output/apis/autoload_static_flask.py:27:37:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/examples/server/app/stocks/main.py#L75'>examples/server/app/stocks/main.py:75:73:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/scripts/sri.py#L54'>scripts/sri.py:54:6:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/scripts/sri.py#L72'>scripts/sri.py:72:34:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/src/bokeh/application/handlers/document_lifecycle.py#L66'>src/bokeh/application/handlers/document_lifecycle.py:66:73:</a> COM812 [*] Trailing comma missing
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/src/bokeh/core/serialization.py#L210'>src/bokeh/core/serialization.py:210:41:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/src/bokeh/core/serialization.py#L490'>src/bokeh/core/serialization.py:490:41:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/src/bokeh/document/models.py#L174'>src/bokeh/document/models.py:174:49:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/src/bokeh/embed/bundle.py#L309'>src/bokeh/embed/bundle.py:309:41:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/src/bokeh/embed/bundle.py#L321'>src/bokeh/embed/bundle.py:321:46:</a> COM819 [*] Trailing comma prohibited
... 33 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/0f74c2d0dda93e8e88a26205b8f6888001ed563b/pymilvus/client/__init__.py#L47'>pymilvus/client/__init__.py:47:65:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/milvus-io/pymilvus/blob/0f74c2d0dda93e8e88a26205b8f6888001ed563b/pymilvus/decorators.py#L81'>pymilvus/decorators.py:81:53:</a> COM819 [*] Trailing comma prohibited
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+91 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/a5cf0816c9c007acb889bfa7ee97b664d6f228e8/rotkehlchen/accounting/cost_basis/base.py#L159'>rotkehlchen/accounting/cost_basis/base.py:159:100:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/rotki/rotki/blob/a5cf0816c9c007acb889bfa7ee97b664d6f228e8/rotkehlchen/accounting/pot.py#L309'>rotkehlchen/accounting/pot.py:309:26:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/rotki/rotki/blob/a5cf0816c9c007acb889bfa7ee97b664d6f228e8/rotkehlchen/assets/asset.py#L581'>rotkehlchen/assets/asset.py:581:40:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/rotki/rotki/blob/a5cf0816c9c007acb889bfa7ee97b664d6f228e8/rotkehlchen/chain/aggregator.py#L141'>rotkehlchen/chain/aggregator.py:141:17:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/rotki/rotki/blob/a5cf0816c9c007acb889bfa7ee97b664d6f228e8/rotkehlchen/chain/aggregator.py#L776'>rotkehlchen/chain/aggregator.py:776:28:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/rotki/rotki/blob/a5cf0816c9c007acb889bfa7ee97b664d6f228e8/rotkehlchen/chain/bitcoin/utils.py#L189'>rotkehlchen/chain/bitcoin/utils.py:189:25:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/rotki/rotki/blob/a5cf0816c9c007acb889bfa7ee97b664d6f228e8/rotkehlchen/chain/bitcoin/utils.py#L192'>rotkehlchen/chain/bitcoin/utils.py:192:25:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/rotki/rotki/blob/a5cf0816c9c007acb889bfa7ee97b664d6f228e8/rotkehlchen/chain/ethereum/modules/eth2/eth2.py#L303'>rotkehlchen/chain/ethereum/modules/eth2/eth2.py:303:31:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/rotki/rotki/blob/a5cf0816c9c007acb889bfa7ee97b664d6f228e8/rotkehlchen/chain/ethereum/modules/eth2/eth2.py#L304'>rotkehlchen/chain/ethereum/modules/eth2/eth2.py:304:25:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/rotki/rotki/blob/a5cf0816c9c007acb889bfa7ee97b664d6f228e8/rotkehlchen/chain/ethereum/modules/hop/decoder.py#L45'>rotkehlchen/chain/ethereum/modules/hop/decoder.py:45:44:</a> COM819 [*] Trailing comma prohibited
... 20 additional changes omitted for rule COM819
... 81 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (+23 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/sphinx/domains/cpp.py#L6218'>sphinx/domains/cpp.py:6218:80:</a> COM812 [*] Trailing comma missing
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/sphinx/ext/apidoc.py#L83'>sphinx/ext/apidoc.py:83:35:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/sphinx/ext/doctest.py#L463'>sphinx/ext/doctest.py:463:27:</a> COM812 [*] Trailing comma missing
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/sphinx/roles.py#L104'>sphinx/roles.py:104:51:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/sphinx/testing/util.py#L49'>sphinx/testing/util.py:49:31:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/sphinx/testing/util.py#L51'>sphinx/testing/util.py:51:41:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/sphinx/testing/util.py#L63'>sphinx/testing/util.py:63:38:</a> COM818 Trailing comma on bare tuple prohibited
- <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/sphinx/writers/texinfo.py#L433'>sphinx/writers/texinfo.py:433:68:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/tests/test_build_latex.py#L1737'>tests/test_build_latex.py:1737:50:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/tests/test_ext_autodoc.py#L439'>tests/test_ext_autodoc.py:439:50:</a> COM818 Trailing comma on bare tuple prohibited
... 14 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+137 -169 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/dadf03536614be670a418b6565c305003fc0f89a/analytics/lib/fixtures.py#L60'>analytics/lib/fixtures.py:60:88:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/dadf03536614be670a418b6565c305003fc0f89a/analytics/management/commands/update_analytics_counts.py#L49'>analytics/management/commands/update_analytics_counts.py:49:35:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/dadf03536614be670a418b6565c305003fc0f89a/analytics/management/commands/update_analytics_counts.py#L94'>analytics/management/commands/update_analytics_counts.py:94:107:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/dadf03536614be670a418b6565c305003fc0f89a/analytics/tests/test_stats_views.py#L463'>analytics/tests/test_stats_views.py:463:306:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/dadf03536614be670a418b6565c305003fc0f89a/analytics/tests/test_stats_views.py#L478'>analytics/tests/test_stats_views.py:478:306:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/dadf03536614be670a418b6565c305003fc0f89a/analytics/tests/test_stats_views.py#L512'>analytics/tests/test_stats_views.py:512:291:</a> COM812 [*] Trailing comma missing
... 170 additional changes omitted for rule COM812
+ <a href='https://github.com/zulip/zulip/blob/dadf03536614be670a418b6565c305003fc0f89a/corporate/tests/test_stripe.py#L1988'>corporate/tests/test_stripe.py:1988:53:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/zulip/zulip/blob/dadf03536614be670a418b6565c305003fc0f89a/scripts/lib/sharding.py#L35'>scripts/lib/sharding.py:35:35:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/zulip/zulip/blob/dadf03536614be670a418b6565c305003fc0f89a/scripts/lib/sharding.py#L69'>scripts/lib/sharding.py:69:49:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/zulip/zulip/blob/dadf03536614be670a418b6565c305003fc0f89a/scripts/lib/zulip_tools.py#L131'>scripts/lib/zulip_tools.py:131:34:</a> COM819 [*] Trailing comma prohibited
... 296 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| COM812 | 1156 | 140 | 1016 | 0 | 0 |
| COM819 | 315 | 314 | 1 | 0 | 0 |
| COM818 | 195 | 195 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+629 -955 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+336 -786 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/api/common/trigger_dag.py#L70'>airflow/api/common/trigger_dag.py:70:78:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/api_connexion/endpoints/config_endpoint.py#L122'>airflow/api_connexion/endpoints/config_endpoint.py:122:103:</a> COM812 [*] Trailing comma missing
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/api_connexion/endpoints/config_endpoint.py#L122'>airflow/api_connexion/endpoints/config_endpoint.py:122:45:</a> COM812 [*] Trailing comma missing
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/api_connexion/endpoints/task_instance_endpoint.py#L387'>airflow/api_connexion/endpoints/task_instance_endpoint.py:387:24:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/api_connexion/endpoints/task_instance_endpoint.py#L387'>airflow/api_connexion/endpoints/task_instance_endpoint.py:387:95:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/api_connexion/endpoints/task_instance_endpoint.py#L535'>airflow/api_connexion/endpoints/task_instance_endpoint.py:535:102:</a> COM812 [*] Trailing comma missing
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/api_connexion/endpoints/task_instance_endpoint.py#L535'>airflow/api_connexion/endpoints/task_instance_endpoint.py:535:20:</a> COM812 [*] Trailing comma missing
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/api_connexion/parameters.py#L121'>airflow/api_connexion/parameters.py:121:20:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/api_connexion/parameters.py#L122'>airflow/api_connexion/parameters.py:122:57:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/api_internal/internal_api_call.py#L106'>airflow/api_internal/internal_api_call.py:106:103:</a> COM812 [*] Trailing comma missing
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/auth/managers/fab/api_endpoints/role_and_permission_endpoint.py#L83'>airflow/auth/managers/fab/api_endpoints/role_and_permission_endpoint.py:83:20:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/auth/managers/fab/api_endpoints/role_and_permission_endpoint.py#L84'>airflow/auth/managers/fab/api_endpoints/role_and_permission_endpoint.py:84:57:</a> COM812 [*] Trailing comma missing
... 899 additional changes omitted for rule COM812
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/cli/cli_config.py#L69'>airflow/cli/cli_config.py:69:20:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/cli/commands/db_command.py#L214'>airflow/cli/commands/db_command.py:214:41:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/hooks/filesystem.py#L86'>airflow/hooks/filesystem.py:86:29:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/hooks/filesystem.py#L87'>airflow/hooks/filesystem.py:87:24:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/hooks/package_index.py#L92'>airflow/hooks/package_index.py:92:25:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/hooks/package_index.py#L94'>airflow/hooks/package_index.py:94:20:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/amazon/aws/hooks/base_aws.py#L866'>airflow/providers/amazon/aws/hooks/base_aws.py:866:25:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/apache/beam/hooks/beam.py#L191'>airflow/providers/apache/beam/hooks/beam.py:191:31:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/apache/beam/hooks/beam.py#L507'>airflow/providers/apache/beam/hooks/beam.py:507:31:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/apache/hive/hooks/hive.py#L262'>airflow/providers/apache/hive/hooks/hive.py:262:53:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/apache/hive/hooks/hive.py#L265'>airflow/providers/apache/hive/hooks/hive.py:265:53:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/apache/spark/hooks/spark_submit.py#L276'>airflow/providers/apache/spark/hooks/spark_submit.py:276:40:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/apache/sqoop/hooks/sqoop.py#L125'>airflow/providers/apache/sqoop/hooks/sqoop.py:125:36:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/cncf/kubernetes/decorators/kubernetes.py#L75'>airflow/providers/cncf/kubernetes/decorators/kubernetes.py:75:35:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/cncf/kubernetes/pod_launcher_deprecated.py#L283'>airflow/providers/cncf/kubernetes/pod_launcher_deprecated.py:283:49:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/cncf/kubernetes/utils/delete_from.py#L133'>airflow/providers/cncf/kubernetes/utils/delete_from.py:133:23:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/cncf/kubernetes/utils/delete_from.py#L134'>airflow/providers/cncf/kubernetes/utils/delete_from.py:134:31:</a> COM819 [*] Trailing comma prohibited
... 123 additional changes omitted for rule COM819
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/slack/hooks/slack.py#L227'>airflow/providers/slack/hooks/slack.py:227:25:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/slack/hooks/slack.py#L231'>airflow/providers/slack/hooks/slack.py:231:24:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/airflow/providers/tabular/hooks/tabular.py#L74'>airflow/providers/tabular/hooks/tabular.py:74:25:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/dev/breeze/src/airflow_breeze/commands/ci_image_commands.py#L640'>dev/breeze/src/airflow_breeze/commands/ci_image_commands.py:640:43:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L1055'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:1055:29:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/apache/airflow/blob/9356233762a67c0a787fc3e16040455019af13bd/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L1331'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:1331:29:</a> COM818 Trailing comma on bare tuple prohibited
... 1087 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+41 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/examples/output/apis/autoload_static_flask.py#L27'>examples/output/apis/autoload_static_flask.py:27:37:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/examples/server/app/stocks/main.py#L75'>examples/server/app/stocks/main.py:75:73:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/scripts/sri.py#L54'>scripts/sri.py:54:6:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/scripts/sri.py#L72'>scripts/sri.py:72:34:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/src/bokeh/application/handlers/document_lifecycle.py#L66'>src/bokeh/application/handlers/document_lifecycle.py:66:73:</a> COM812 [*] Trailing comma missing
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/src/bokeh/document/models.py#L174'>src/bokeh/document/models.py:174:49:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/src/bokeh/embed/bundle.py#L309'>src/bokeh/embed/bundle.py:309:41:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/src/bokeh/embed/bundle.py#L321'>src/bokeh/embed/bundle.py:321:46:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/src/bokeh/io/webdriver.py#L83'>src/bokeh/io/webdriver.py:83:55:</a> COM819 [*] Trailing comma prohibited
... 13 additional changes omitted for rule COM819
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/src/bokeh/plotting/_renderer.py#L291'>src/bokeh/plotting/_renderer.py:291:120:</a> COM812 [*] Trailing comma missing
... 31 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/0f74c2d0dda93e8e88a26205b8f6888001ed563b/pymilvus/client/__init__.py#L47'>pymilvus/client/__init__.py:47:65:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/milvus-io/pymilvus/blob/0f74c2d0dda93e8e88a26205b8f6888001ed563b/pymilvus/decorators.py#L81'>pymilvus/decorators.py:81:53:</a> COM819 [*] Trailing comma prohibited
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+91 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/a5cf0816c9c007acb889bfa7ee97b664d6f228e8/rotkehlchen/accounting/cost_basis/base.py#L159'>rotkehlchen/accounting/cost_basis/base.py:159:100:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/rotki/rotki/blob/a5cf0816c9c007acb889bfa7ee97b664d6f228e8/rotkehlchen/accounting/pot.py#L309'>rotkehlchen/accounting/pot.py:309:26:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/rotki/rotki/blob/a5cf0816c9c007acb889bfa7ee97b664d6f228e8/rotkehlchen/assets/asset.py#L581'>rotkehlchen/assets/asset.py:581:40:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/rotki/rotki/blob/a5cf0816c9c007acb889bfa7ee97b664d6f228e8/rotkehlchen/chain/aggregator.py#L141'>rotkehlchen/chain/aggregator.py:141:17:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/rotki/rotki/blob/a5cf0816c9c007acb889bfa7ee97b664d6f228e8/rotkehlchen/chain/aggregator.py#L776'>rotkehlchen/chain/aggregator.py:776:28:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/rotki/rotki/blob/a5cf0816c9c007acb889bfa7ee97b664d6f228e8/rotkehlchen/chain/bitcoin/utils.py#L189'>rotkehlchen/chain/bitcoin/utils.py:189:25:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/rotki/rotki/blob/a5cf0816c9c007acb889bfa7ee97b664d6f228e8/rotkehlchen/chain/bitcoin/utils.py#L192'>rotkehlchen/chain/bitcoin/utils.py:192:25:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/rotki/rotki/blob/a5cf0816c9c007acb889bfa7ee97b664d6f228e8/rotkehlchen/chain/ethereum/modules/eth2/eth2.py#L303'>rotkehlchen/chain/ethereum/modules/eth2/eth2.py:303:31:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/rotki/rotki/blob/a5cf0816c9c007acb889bfa7ee97b664d6f228e8/rotkehlchen/chain/ethereum/modules/eth2/eth2.py#L304'>rotkehlchen/chain/ethereum/modules/eth2/eth2.py:304:25:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/rotki/rotki/blob/a5cf0816c9c007acb889bfa7ee97b664d6f228e8/rotkehlchen/chain/ethereum/modules/hop/decoder.py#L45'>rotkehlchen/chain/ethereum/modules/hop/decoder.py:45:44:</a> COM819 [*] Trailing comma prohibited
... 20 additional changes omitted for rule COM819
... 81 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (+23 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/sphinx/domains/cpp.py#L6218'>sphinx/domains/cpp.py:6218:80:</a> COM812 [*] Trailing comma missing
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/sphinx/ext/apidoc.py#L83'>sphinx/ext/apidoc.py:83:35:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/sphinx/ext/doctest.py#L463'>sphinx/ext/doctest.py:463:27:</a> COM812 [*] Trailing comma missing
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/sphinx/roles.py#L104'>sphinx/roles.py:104:51:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/sphinx/testing/util.py#L49'>sphinx/testing/util.py:49:31:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/sphinx/testing/util.py#L51'>sphinx/testing/util.py:51:41:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/sphinx/testing/util.py#L63'>sphinx/testing/util.py:63:38:</a> COM818 Trailing comma on bare tuple prohibited
- <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/sphinx/writers/texinfo.py#L433'>sphinx/writers/texinfo.py:433:68:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/tests/test_build_latex.py#L1737'>tests/test_build_latex.py:1737:50:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/tests/test_ext_autodoc.py#L439'>tests/test_ext_autodoc.py:439:50:</a> COM818 Trailing comma on bare tuple prohibited
... 14 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+136 -168 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/dadf03536614be670a418b6565c305003fc0f89a/analytics/lib/fixtures.py#L60'>analytics/lib/fixtures.py:60:88:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/dadf03536614be670a418b6565c305003fc0f89a/analytics/management/commands/update_analytics_counts.py#L49'>analytics/management/commands/update_analytics_counts.py:49:35:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/dadf03536614be670a418b6565c305003fc0f89a/analytics/management/commands/update_analytics_counts.py#L94'>analytics/management/commands/update_analytics_counts.py:94:107:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/dadf03536614be670a418b6565c305003fc0f89a/analytics/tests/test_stats_views.py#L463'>analytics/tests/test_stats_views.py:463:306:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/dadf03536614be670a418b6565c305003fc0f89a/analytics/tests/test_stats_views.py#L478'>analytics/tests/test_stats_views.py:478:306:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/dadf03536614be670a418b6565c305003fc0f89a/analytics/tests/test_stats_views.py#L512'>analytics/tests/test_stats_views.py:512:291:</a> COM812 [*] Trailing comma missing
... 169 additional changes omitted for rule COM812
+ <a href='https://github.com/zulip/zulip/blob/dadf03536614be670a418b6565c305003fc0f89a/corporate/tests/test_stripe.py#L1988'>corporate/tests/test_stripe.py:1988:53:</a> COM819 [*] Trailing comma prohibited
+ <a href='https://github.com/zulip/zulip/blob/dadf03536614be670a418b6565c305003fc0f89a/scripts/lib/sharding.py#L35'>scripts/lib/sharding.py:35:35:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/zulip/zulip/blob/dadf03536614be670a418b6565c305003fc0f89a/scripts/lib/sharding.py#L69'>scripts/lib/sharding.py:69:49:</a> COM818 Trailing comma on bare tuple prohibited
+ <a href='https://github.com/zulip/zulip/blob/dadf03536614be670a418b6565c305003fc0f89a/scripts/lib/zulip_tools.py#L131'>scripts/lib/zulip_tools.py:131:34:</a> COM819 [*] Trailing comma prohibited
... 294 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| COM812 | 1088 | 134 | 954 | 0 | 0 |
| COM819 | 306 | 305 | 1 | 0 | 0 |
| COM818 | 190 | 190 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2023-11-09 04:51_

Ahh this is buggy. Need to fix.

---

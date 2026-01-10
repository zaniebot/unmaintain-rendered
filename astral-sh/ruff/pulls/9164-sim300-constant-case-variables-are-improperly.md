```yaml
number: 9164
title: "SIM300: CONSTANT_CASE variables are improperly flagged for yoda violation"
type: pull_request
state: merged
author: asafamr-mm
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: SIM300-CONSTANT-CASE-false-positives
created_at: 2023-12-16T19:22:54Z
updated_at: 2023-12-20T20:33:30Z
url: https://github.com/astral-sh/ruff/pull/9164
synced_at: 2026-01-10T23:07:18Z
```

# SIM300: CONSTANT_CASE variables are improperly flagged for yoda violation

---

_Pull request opened by @asafamr-mm on 2023-12-16 19:22_

## Summary

fixes #6956 
details in issue

Following an advice in https://github.com/astral-sh/ruff/issues/6956#issuecomment-1817672585,
this change separates expressions to 3 levels of "constant likelihood":
*  literals, empty dict and tuples... (definitely constant, level 2)
*  CONSTANT_CASE vars (probably constant, 1)
* all other expressions (0)

a comparison is marked yoda if the level is strictly higher on its left hand side

following https://github.com/astral-sh/ruff/issues/6956#issuecomment-1697107822 marking compound expressions of literals (e.g. `60 * 60` ) as constants 
this change current behaviour on `SomeClass().settings.SOME_CONSTANT_VALUE > (60 * 60)` in the fixture from error to ok  



---

_Comment by @github-actions[bot] on 2023-12-16 19:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+15 -0 violations, +0 -0 fixes in 4 projects; 37 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/tests/plugins/test_plugins_manager.py#L146'>tests/plugins/test_plugins_manager.py:146:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/tests/providers/cncf/kubernetes/executors/test_kubernetes_executor.py#L156'>tests/providers/cncf/kubernetes/executors/test_kubernetes_executor.py:156:20:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/tests/providers/cncf/kubernetes/test_pod_generator.py#L579'>tests/providers/cncf/kubernetes/test_pod_generator.py:579:16:</a> SIM300 [*] Yoda conditions are discouraged, use `result.metadata.annotations["dag_id"] == "a" * 512` instead
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/tests/providers/cncf/kubernetes/test_pod_generator.py#L580'>tests/providers/cncf/kubernetes/test_pod_generator.py:580:16:</a> SIM300 [*] Yoda conditions are discouraged
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/574294623fa519094a67c585b2d7c19defd22a76/Packs/FeedCyjax/Integrations/FeedCyjax/FeedCyjax_test.py#L20'>Packs/FeedCyjax/Integrations/FeedCyjax/FeedCyjax_test.py:20:12:</a> SIM300 [*] Yoda conditions are discouraged, use `INDICATORS_LAST_FETCH_KEY == 'last_fetch'` instead
+ <a href='https://github.com/demisto/content/blob/574294623fa519094a67c585b2d7c19defd22a76/Packs/FeedCyjax/Integrations/FeedCyjax/FeedCyjax_test.py#L21'>Packs/FeedCyjax/Integrations/FeedCyjax/FeedCyjax_test.py:21:12:</a> SIM300 [*] Yoda conditions are discouraged, use `DATE_FORMAT == '%Y-%m-%dT%H:%M:%SZ'` instead
+ <a href='https://github.com/demisto/content/blob/574294623fa519094a67c585b2d7c19defd22a76/Packs/FeedCyjax/Integrations/FeedCyjax/FeedCyjax_test.py#L22'>Packs/FeedCyjax/Integrations/FeedCyjax/FeedCyjax_test.py:22:12:</a> SIM300 [*] Yoda conditions are discouraged, use `INDICATORS_LIMIT == 50` instead
+ <a href='https://github.com/demisto/content/blob/574294623fa519094a67c585b2d7c19defd22a76/Tests/scripts/wait_until_server_ready.py#L104'>Tests/scripts/wait_until_server_ready.py:104:28:</a> SIM300 [*] Yoda conditions are discouraged, use `SETUP_TIMEOUT != 60 * 60` instead
+ <a href='https://github.com/demisto/content/blob/574294623fa519094a67c585b2d7c19defd22a76/Tests/scripts/wait_until_server_ready.py#L96'>Tests/scripts/wait_until_server_ready.py:96:28:</a> SIM300 [*] Yoda conditions are discouraged, use `SETUP_TIMEOUT != 60 * 10` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/8ac08e2cb422b9bf9954e1f0001b1e414d738b7d/reflex/app_module_for_backend.py#L7'>reflex/app_module_for_backend.py:7:4:</a> SIM300 [*] Yoda conditions are discouraged, use `constants.CompileVars.APP != "app"` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/653901fc30e4ae2b44326ca5f7efea01f9cc80f9/zerver/views/realm_emoji.py#L47'>zerver/views/realm_emoji.py:47:8:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/zulip/zulip/blob/653901fc30e4ae2b44326ca5f7efea01f9cc80f9/zerver/views/realm_icon.py#L25'>zerver/views/realm_icon.py:25:8:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/zulip/zulip/blob/653901fc30e4ae2b44326ca5f7efea01f9cc80f9/zerver/views/realm_logo.py#L31'>zerver/views/realm_logo.py:31:8:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/zulip/zulip/blob/653901fc30e4ae2b44326ca5f7efea01f9cc80f9/zerver/views/upload.py#L318'>zerver/views/upload.py:318:8:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/zulip/zulip/blob/653901fc30e4ae2b44326ca5f7efea01f9cc80f9/zerver/views/user_settings.py#L413'>zerver/views/user_settings.py:413:8:</a> SIM300 [*] Yoda conditions are discouraged
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM300 | 15 | 15 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+474 -2 violations, +0 -0 fixes in 8 projects; 33 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+359 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/dev/breeze/tests/test_packages.py#L112'>dev/breeze/tests/test_packages.py:112:12:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/dev/breeze/tests/test_packages.py#L117'>dev/breeze/tests/test_packages.py:117:12:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/dev/breeze/tests/test_packages.py#L122'>dev/breeze/tests/test_packages.py:122:12:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_cleanup_pods.py#L182'>helm_tests/airflow_aux/test_cleanup_pods.py:182:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_cleanup_pods.py#L226'>helm_tests/airflow_aux/test_cleanup_pods.py:226:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_cleanup_pods.py#L229'>helm_tests/airflow_aux/test_cleanup_pods.py:229:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_cleanup_pods.py#L240'>helm_tests/airflow_aux/test_cleanup_pods.py:240:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_create_user_job.py#L164'>helm_tests/airflow_aux/test_create_user_job.py:164:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_create_user_job.py#L179'>helm_tests/airflow_aux/test_create_user_job.py:179:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_create_user_job.py#L193'>helm_tests/airflow_aux/test_create_user_job.py:193:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_create_user_job.py#L206'>helm_tests/airflow_aux/test_create_user_job.py:206:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_create_user_job.py#L209'>helm_tests/airflow_aux/test_create_user_job.py:209:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_create_user_job.py#L335'>helm_tests/airflow_aux/test_create_user_job.py:335:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_create_user_job.py#L336'>helm_tests/airflow_aux/test_create_user_job.py:336:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_create_user_job.py#L356'>helm_tests/airflow_aux/test_create_user_job.py:356:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_logs_persistent_volume_claim.py#L66'>helm_tests/airflow_aux/test_logs_persistent_volume_claim.py:66:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_migrate_database_job.py#L155'>helm_tests/airflow_aux/test_migrate_database_job.py:155:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_migrate_database_job.py#L179'>helm_tests/airflow_aux/test_migrate_database_job.py:179:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_migrate_database_job.py#L217'>helm_tests/airflow_aux/test_migrate_database_job.py:217:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_migrate_database_job.py#L231'>helm_tests/airflow_aux/test_migrate_database_job.py:231:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_migrate_database_job.py#L244'>helm_tests/airflow_aux/test_migrate_database_job.py:244:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_migrate_database_job.py#L247'>helm_tests/airflow_aux/test_migrate_database_job.py:247:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_migrate_database_job.py#L317'>helm_tests/airflow_aux/test_migrate_database_job.py:317:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_migrate_database_job.py#L318'>helm_tests/airflow_aux/test_migrate_database_job.py:318:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_pod_launcher_role.py#L51'>helm_tests/airflow_aux/test_pod_launcher_role.py:51:20:</a> SIM300 [*] Yoda conditions are discouraged, use `docs == []` instead
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_pod_template_file.py#L537'>helm_tests/airflow_aux/test_pod_template_file.py:537:16:</a> SIM300 [*] Yoda conditions are discouraged, use `jmespath.search("spec.affinity", docs[0]) == {}` instead
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_pod_template_file.py#L656'>helm_tests/airflow_aux/test_pod_template_file.py:656:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_pod_template_file.py#L674'>helm_tests/airflow_aux/test_pod_template_file.py:674:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_pod_template_file.py#L686'>helm_tests/airflow_aux/test_pod_template_file.py:686:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_pod_template_file.py#L734'>helm_tests/airflow_aux/test_pod_template_file.py:734:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_pod_template_file.py#L751'>helm_tests/airflow_aux/test_pod_template_file.py:751:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_aux/test_pod_template_file.py#L91'>helm_tests/airflow_aux/test_pod_template_file.py:91:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_core/test_dag_processor.py#L108'>helm_tests/airflow_core/test_dag_processor.py:108:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_core/test_dag_processor.py#L126'>helm_tests/airflow_core/test_dag_processor.py:126:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_core/test_dag_processor.py#L336'>helm_tests/airflow_core/test_dag_processor.py:336:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_core/test_dag_processor.py#L371'>helm_tests/airflow_core/test_dag_processor.py:371:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/apache/airflow/blob/23b87534e636e407f3f68107d442f55b771216f3/helm_tests/airflow_core/test_dag_processor.py#L420'>helm_tests/airflow_core/test_dag_processor.py:420:16:</a> SIM300 [*] Yoda conditions are discouraged
... 322 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+15 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/unit/bokeh/application/handlers/test_server_request_handler.py#L121'>tests/unit/bokeh/application/handlers/test_server_request_handler.py:121:16:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/unit/bokeh/document/test_document.py#L717'>tests/unit/bokeh/document/test_document.py:717:16:</a> SIM300 [*] Yoda conditions are discouraged, use `foos == [42,43]` instead
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/unit/bokeh/io/test_doc.py#L54'>tests/unit/bokeh/io/test_doc.py:54:12:</a> SIM300 [*] Yoda conditions are discouraged, use `[] == bid._PATCHED_CURDOCS` instead
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/unit/bokeh/models/test_sources.py#L100'>tests/unit/bokeh/models/test_sources.py:100:16:</a> SIM300 [*] Yoda conditions are discouraged, use `list(ds.data['index']) == [0, 1]` instead
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/unit/bokeh/models/test_sources.py#L112'>tests/unit/bokeh/models/test_sources.py:112:16:</a> SIM300 [*] Yoda conditions are discouraged, use `list(ds.data['index']) == [0, 1]` instead
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/unit/bokeh/models/test_sources.py#L124'>tests/unit/bokeh/models/test_sources.py:124:16:</a> SIM300 [*] Yoda conditions are discouraged, use `list(ds.data['level_0']) == [0, 1]` instead
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/unit/bokeh/models/test_sources.py#L138'>tests/unit/bokeh/models/test_sources.py:138:16:</a> SIM300 [*] Yoda conditions are discouraged, use `list(ds.data['level_0']) == [0, 1]` instead
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/unit/bokeh/models/test_sources.py#L151'>tests/unit/bokeh/models/test_sources.py:151:16:</a> SIM300 [*] Yoda conditions are discouraged, use `list(ds.data['index']) == [0, 1]` instead
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/unit/bokeh/models/test_sources.py#L166'>tests/unit/bokeh/models/test_sources.py:166:16:</a> SIM300 [*] Yoda conditions are discouraged, use `list(ds.data['index']) == [0, 1]` instead
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/tests/unit/bokeh/models/test_sources.py#L86'>tests/unit/bokeh/models/test_sources.py:86:16:</a> SIM300 [*] Yoda conditions are discouraged, use `list(ds.data['index']) == [0, 1]` instead
... 7 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+89 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/574294623fa519094a67c585b2d7c19defd22a76/Packs/Algosec/Scripts/AlgosecGetTicket/AlgosecGetTicket_test.py#L18'>Packs/Algosec/Scripts/AlgosecGetTicket/AlgosecGetTicket_test.py:18:12:</a> SIM300 [*] Yoda conditions are discouraged, use `content == {'some_info': 'info: test'}` instead
+ <a href='https://github.com/demisto/content/blob/574294623fa519094a67c585b2d7c19defd22a76/Packs/ApiModules/Scripts/CSVFeedApiModule/CSVFeedApiModule_test.py#L241'>Packs/ApiModules/Scripts/CSVFeedApiModule/CSVFeedApiModule_test.py:241:20:</a> SIM300 [*] Yoda conditions are discouraged, use `indicators[0]['fields']['tags'] == []` instead
+ <a href='https://github.com/demisto/content/blob/574294623fa519094a67c585b2d7c19defd22a76/Packs/ApiModules/Scripts/HTTPFeedApiModule/HTTPFeedApiModule_test.py#L169'>Packs/ApiModules/Scripts/HTTPFeedApiModule/HTTPFeedApiModule_test.py:169:12:</a> SIM300 [*] Yoda conditions are discouraged, use `client.get_feed_config() == {}` instead
+ <a href='https://github.com/demisto/content/blob/574294623fa519094a67c585b2d7c19defd22a76/Packs/AzureNetworkSecurityGroups/Integrations/AzureNetworkSecurityGroups/AzureNetworkSecurityGroups_test.py#L73'>Packs/AzureNetworkSecurityGroups/Integrations/AzureNetworkSecurityGroups/AzureNetworkSecurityGroups_test.py:73:12:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/demisto/content/blob/574294623fa519094a67c585b2d7c19defd22a76/Packs/Base/Scripts/GetIncidentsByQuery/GetIncidentsByQuery_test.py#L152'>Packs/Base/Scripts/GetIncidentsByQuery/GetIncidentsByQuery_test.py:152:12:</a> SIM300 [*] Yoda conditions are discouraged, use `entry['Contents'][0]['context'] == {}` instead
+ <a href='https://github.com/demisto/content/blob/574294623fa519094a67c585b2d7c19defd22a76/Packs/CarbonBlackProtect/Scripts/CBPCatalogFindHash/CBPCatalogFindHash_test.py#L14'>Packs/CarbonBlackProtect/Scripts/CBPCatalogFindHash/CBPCatalogFindHash_test.py:14:12:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/demisto/content/blob/574294623fa519094a67c585b2d7c19defd22a76/Packs/CheckPointSandBlast/Integrations/CheckPointSandBlast/CheckPointSandBlast_test.py#L122'>Packs/CheckPointSandBlast/Integrations/CheckPointSandBlast/CheckPointSandBlast_test.py:122:12:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/demisto/content/blob/574294623fa519094a67c585b2d7c19defd22a76/Packs/CiscoASA/Integrations/CiscoASA/CiscoASA_test.py#L129'>Packs/CiscoASA/Integrations/CiscoASA/CiscoASA_test.py:129:12:</a> SIM300 [*] Yoda conditions are discouraged, use `command_results.outputs == []` instead
+ <a href='https://github.com/demisto/content/blob/574294623fa519094a67c585b2d7c19defd22a76/Packs/CommonScripts/Scripts/DomainReputation/DomainReputation_test.py#L34'>Packs/CommonScripts/Scripts/DomainReputation/DomainReputation_test.py:34:12:</a> SIM300 [*] Yoda conditions are discouraged, use `results_mock.call_args[0][0] == []` instead
+ <a href='https://github.com/demisto/content/blob/574294623fa519094a67c585b2d7c19defd22a76/Packs/CommonScripts/Scripts/ExtractIndicatorsFromTextFile/ExtractIndicatorsFromTextFile_test.py#L27'>Packs/CommonScripts/Scripts/ExtractIndicatorsFromTextFile/ExtractIndicatorsFromTextFile_test.py:27:12:</a> SIM300 [*] Yoda conditions are discouraged
... 79 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/model-bakers/model_bakery">model-bakers/model_bakery</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/model-bakers/model_bakery/blob/c972cbb24f8c1fabfd285112588a72bd282d9014/tests/test_baker.py#L800'>tests/test_baker.py:800:16:</a> SIM300 [*] Yoda conditions are discouraged, use `instance.fk == ["foo"]` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/8ac08e2cb422b9bf9954e1f0001b1e414d738b7d/reflex/app_module_for_backend.py#L7'>reflex/app_module_for_backend.py:7:4:</a> SIM300 [*] Yoda conditions are discouraged, use `constants.CompileVars.APP != "app"` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/1823dbd1db67a19516f4752de6bcdca423db690c/rotkehlchen/tests/db/test_db_upgrades.py#L580'>rotkehlchen/tests/db/test_db_upgrades.py:580:12:</a> SIM300 [*] Yoda conditions are discouraged, use `[x[0] for x in manual_balance_ids] == [1, 2, 3]` instead
+ <a href='https://github.com/rotki/rotki/blob/1823dbd1db67a19516f4752de6bcdca423db690c/rotkehlchen/tests/unit/test_eth2.py#L760'>rotkehlchen/tests/unit/test_eth2.py:760:16:</a> SIM300 [*] Yoda conditions are discouraged, use `hidden_ids == [2]` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/sphinx-doc/sphinx/blob/35965903177c6ed9a6afb62ccd33243a746a3fc0/sphinx/ext/napoleon/docstring.py#L1236'>sphinx/ext/napoleon/docstring.py:1236:17:</a> SIM300 [*] Yoda conditions are discouraged, use `[line1, line2] == ['', '']` instead
+ <a href='https://github.com/sphinx-doc/sphinx/blob/35965903177c6ed9a6afb62ccd33243a746a3fc0/sphinx/util/__init__.py#L205'>sphinx/util/__init__.py:205:16:</a> SIM300 [*] Yoda conditions are discouraged, use `begend == ['', '']` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/653901fc30e4ae2b44326ca5f7efea01f9cc80f9/zerver/views/realm_emoji.py#L47'>zerver/views/realm_emoji.py:47:8:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/zulip/zulip/blob/653901fc30e4ae2b44326ca5f7efea01f9cc80f9/zerver/views/realm_icon.py#L25'>zerver/views/realm_icon.py:25:8:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/zulip/zulip/blob/653901fc30e4ae2b44326ca5f7efea01f9cc80f9/zerver/views/realm_logo.py#L31'>zerver/views/realm_logo.py:31:8:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/zulip/zulip/blob/653901fc30e4ae2b44326ca5f7efea01f9cc80f9/zerver/views/upload.py#L318'>zerver/views/upload.py:318:8:</a> SIM300 [*] Yoda conditions are discouraged
+ <a href='https://github.com/zulip/zulip/blob/653901fc30e4ae2b44326ca5f7efea01f9cc80f9/zerver/views/user_settings.py#L413'>zerver/views/user_settings.py:413:8:</a> SIM300 [*] Yoda conditions are discouraged
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM300 | 476 | 474 | 2 | 0 | 0 |

</p>
</details>




---

_Label `bug` added by @charliermarsh on 2023-12-20 17:36_

---

_Label `rule` added by @charliermarsh on 2023-12-20 17:36_

---

_Merged by @charliermarsh on 2023-12-20 20:26_

---

_Closed by @charliermarsh on 2023-12-20 20:26_

---

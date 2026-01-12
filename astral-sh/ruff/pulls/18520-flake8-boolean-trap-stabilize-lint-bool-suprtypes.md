```yaml
number: 18520
title: "[`flake8-boolean-trap`] Stabilize lint `bool` suprtypes in `boolean-type-hint-positional-argument` (`FBT001`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
  - fixes
assignees: []
merged: true
base: brent/release-0.12.0
head: dylan/stabilize-boolean-trap-optional
created_at: 2025-06-06T23:13:09Z
updated_at: 2025-06-08T18:08:00Z
url: https://github.com/astral-sh/ruff/pull/18520
synced_at: 2026-01-12T15:56:20Z
```

# [`flake8-boolean-trap`] Stabilize lint `bool` suprtypes in `boolean-type-hint-positional-argument` (`FBT001`)

---

_@dylwil3_

Feel free to complain about the rephrasing in the docs!

---

_Label `rule` added by @dylwil3 on 2025-06-06 23:13_

---

_Label `fixes` added by @dylwil3 on 2025-06-06 23:13_

---

_Comment by @github-actions[bot] on 2025-06-06 23:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+226 -0 violations, +0 -0 fixes in 7 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+123 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/airflow-core/src/airflow/api_fastapi/common/parameters.py#L690'>airflow-core/src/airflow/api_fastapi/common/parameters.py:690:23:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/airflow-core/src/airflow/api_fastapi/core_api/routes/public/job.py#L102'>airflow-core/src/airflow/api_fastapi/core_api/routes/public/job.py:102:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/airflow-core/src/airflow/models/baseoperator.py#L523'>airflow-core/src/airflow/models/baseoperator.py:523:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/airflow-core/src/airflow/models/dagbag.py#L124'>airflow-core/src/airflow/models/dagbag.py:124:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/airflow-core/src/airflow/models/dagbag.py#L125'>airflow-core/src/airflow/models/dagbag.py:125:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/airflow-core/src/airflow/models/taskinstance.py#L1712'>airflow-core/src/airflow/models/taskinstance.py:1712:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/airflow-core/src/airflow/models/taskinstance.py#L1821'>airflow-core/src/airflow/models/taskinstance.py:1821:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/airflow-core/src/airflow/timetables/trigger.py#L55'>airflow-core/src/airflow/timetables/trigger.py:55:32:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/airflow-core/src/airflow/timetables/trigger.py#L61'>airflow-core/src/airflow/timetables/trigger.py:61:34:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/airflow-core/src/airflow/utils/helpers.py#L62'>airflow-core/src/airflow/utils/helpers.py:62:30:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/airflow-core/src/airflow/utils/helpers.py#L79'>airflow-core/src/airflow/utils/helpers.py:79:54:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/dev/breeze/src/airflow_breeze/params/shell_params.py#L109'>dev/breeze/src/airflow_breeze/params/shell_params.py:109:50:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/dev/breeze/src/airflow_breeze/utils/coertions.py#L23'>dev/breeze/src/airflow_breeze/utils/coertions.py:23:23:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/dev/breeze/src/airflow_breeze/utils/shared_options.py#L38'>dev/breeze/src/airflow_breeze/utils/shared_options.py:38:17:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/dev/breeze/src/airflow_breeze/utils/shared_options.py#L52'>dev/breeze/src/airflow_breeze/utils/shared_options.py:52:17:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/devel-common/src/tests_common/pytest_plugin.py#L2133'>devel-common/src/tests_common/pytest_plugin.py:2133:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/providers/amazon/src/airflow/providers/amazon/aws/hooks/base_aws.py#L492'>providers/amazon/src/airflow/providers/amazon/aws/hooks/base_aws.py:492:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/providers/amazon/src/airflow/providers/amazon/aws/hooks/dms.py#L72'>providers/amazon/src/airflow/providers/amazon/aws/hooks/dms.py:72:72:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/providers/amazon/src/airflow/providers/amazon/aws/hooks/logs.py#L67'>providers/amazon/src/airflow/providers/amazon/aws/hooks/logs.py:67:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/providers/amazon/src/airflow/providers/amazon/aws/triggers/glue.py#L106'>providers/amazon/src/airflow/providers/amazon/aws/triggers/glue.py:106:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/providers/amazon/src/airflow/providers/amazon/aws/triggers/redshift_data.py#L53'>providers/amazon/src/airflow/providers/amazon/aws/triggers/redshift_data.py:53:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/providers/amazon/src/airflow/providers/amazon/aws/triggers/s3.py#L161'>providers/amazon/src/airflow/providers/amazon/aws/triggers/s3.py:161:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/providers/amazon/src/airflow/providers/amazon/aws/triggers/s3.py#L57'>providers/amazon/src/airflow/providers/amazon/aws/triggers/s3.py:57:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/providers/amazon/src/airflow/providers/amazon/aws/triggers/sqs.py#L83'>providers/amazon/src/airflow/providers/amazon/aws/triggers/sqs.py:83:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/providers/amazon/src/airflow/providers/amazon/aws/utils/mixins.py#L57'>providers/amazon/src/airflow/providers/amazon/aws/utils/mixins.py:57:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/providers/apache/spark/src/airflow/providers/apache/spark/decorators/pyspark.py#L134'>providers/apache/spark/src/airflow/providers/apache/spark/decorators/pyspark.py:134:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/airflow/blob/43cd627100da2a8dbad0bf55af308a299976d1c8/providers/apprise/src/airflow/providers/apprise/hooks/apprise.py#L80'>providers/apprise/src/airflow/providers/apprise/hooks/apprise.py:80:9:</a> FBT001 Boolean-typed positional argument in function definition
... 96 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+42 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/5541dad32b4ed87ac843c53add71ee4ef1de92d2/RELEASING/changelog.py#L73'>RELEASING/changelog.py:73:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/superset/blob/5541dad32b4ed87ac843c53add71ee4ef1de92d2/superset/async_events/async_query_manager.py#L220'>superset/async_events/async_query_manager.py:220:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/superset/blob/5541dad32b4ed87ac843c53add71ee4ef1de92d2/superset/commands/dataset/update.py#L61'>superset/commands/dataset/update.py:61:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/superset/blob/5541dad32b4ed87ac843c53add71ee4ef1de92d2/superset/common/query_actions.py#L99'>superset/common/query_actions.py:99:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/superset/blob/5541dad32b4ed87ac843c53add71ee4ef1de92d2/superset/common/query_context.py#L121'>superset/common/query_context.py:121:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/superset/blob/5541dad32b4ed87ac843c53add71ee4ef1de92d2/superset/common/query_context.py#L98'>superset/common/query_context.py:98:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/superset/blob/5541dad32b4ed87ac843c53add71ee4ef1de92d2/superset/common/query_context_processor.py#L131'>superset/common/query_context_processor.py:131:39:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/superset/blob/5541dad32b4ed87ac843c53add71ee4ef1de92d2/superset/common/query_context_processor.py#L746'>superset/common/query_context_processor.py:746:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/superset/blob/5541dad32b4ed87ac843c53add71ee4ef1de92d2/superset/common/query_object.py#L178'>superset/common/query_object.py:178:34:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/apache/superset/blob/5541dad32b4ed87ac843c53add71ee4ef1de92d2/superset/common/query_object.py#L208'>superset/common/query_object.py:208:9:</a> FBT001 Boolean-typed positional argument in function definition
... 32 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/util.py#L186'>src/bokeh/embed/util.py:186:65:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L609'>src/bokeh/server/tornado.py:609:25:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/settings.py#L172'>src/bokeh/settings.py:172:18:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/token.py#L207'>src/bokeh/util/token.py:207:32:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_settings.py#L131'>tests/unit/bokeh/test_settings.py:131:33:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_settings.py#L135'>tests/unit/bokeh/test_settings.py:135:39:</a> FBT001 Boolean-typed positional argument in function definition
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+14 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/langchain_core/language_models/base.py#L133'>libs/core/langchain_core/language_models/base.py:133:26:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/langchain_core/language_models/llms.py#L132'>libs/core/langchain_core/language_models/llms.py:132:20:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/langchain_core/language_models/llms.py#L158'>libs/core/langchain_core/language_models/llms.py:158:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/langchain_core/language_models/llms.py#L194'>libs/core/langchain_core/language_models/llms.py:194:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/langchain_core/language_models/llms.py#L227'>libs/core/langchain_core/language_models/llms.py:227:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/langchain_core/language_models/llms.py#L260'>libs/core/langchain_core/language_models/llms.py:260:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/langchain_core/tools/base.py#L667'>libs/core/langchain_core/tools/base.py:667:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/langchain_core/tools/base.py#L779'>libs/core/langchain_core/tools/base.py:779:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/tests/unit_tests/language_models/chat_models/test_base.py#L332'>libs/core/tests/unit_tests/language_models/chat_models/test_base.py:332:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/langchain-ai/langchain/blob/ece9e31a7a33c1e42dbb85fb1f7e8e960a51287e/libs/core/tests/unit_tests/language_models/chat_models/test_base.py#L356'>libs/core/tests/unit_tests/language_models/chat_models/test_base.py:356:5:</a> FBT001 Boolean-typed positional argument in function definition
... 4 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/0a29cc1c89c6c79e6f9bb1124d1bbc16bb070cda/src/scikit_build_core/settings/skbuild_model.py#L46'>src/scikit_build_core/settings/skbuild_model.py:46:22:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/0a29cc1c89c6c79e6f9bb1124d1bbc16bb070cda/src/scikit_build_core/settings/skbuild_overrides.py#L243'>src/scikit_build_core/settings/skbuild_overrides.py:243:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/0a29cc1c89c6c79e6f9bb1124d1bbc16bb070cda/src/scikit_build_core/settings/skbuild_overrides.py#L244'>src/scikit_build_core/settings/skbuild_overrides.py:244:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/0a29cc1c89c6c79e6f9bb1124d1bbc16bb070cda/src/scikit_build_core/settings/sources.py#L391'>src/scikit_build_core/settings/sources.py:391:14:</a> FBT001 Boolean-typed positional argument in function definition
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+33 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/5a090c47adb7bc5578af453bb7d99c99d51cbea9/analytics/lib/counts.py#L329'>analytics/lib/counts.py:329:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5a090c47adb7bc5578af453bb7d99c99d51cbea9/corporate/tests/test_stripe.py#L463'>corporate/tests/test_stripe.py:463:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5a090c47adb7bc5578af453bb7d99c99d51cbea9/scripts/lib/zulip_tools.py#L608'>scripts/lib/zulip_tools.py:608:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5a090c47adb7bc5578af453bb7d99c99d51cbea9/zerver/actions/create_user.py#L512'>zerver/actions/create_user.py:512:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5a090c47adb7bc5578af453bb7d99c99d51cbea9/zerver/actions/custom_profile_fields.py#L115'>zerver/actions/custom_profile_fields.py:115:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5a090c47adb7bc5578af453bb7d99c99d51cbea9/zerver/actions/custom_profile_fields.py#L116'>zerver/actions/custom_profile_fields.py:116:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5a090c47adb7bc5578af453bb7d99c99d51cbea9/zerver/actions/custom_profile_fields.py#L117'>zerver/actions/custom_profile_fields.py:117:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5a090c47adb7bc5578af453bb7d99c99d51cbea9/zerver/actions/navigation_views.py#L46'>zerver/actions/navigation_views.py:46:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5a090c47adb7bc5578af453bb7d99c99d51cbea9/zerver/actions/user_settings.py#L437'>zerver/actions/user_settings.py:437:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/zulip/zulip/blob/5a090c47adb7bc5578af453bb7d99c99d51cbea9/zerver/actions/user_status.py#L13'>zerver/actions/user_status.py:13:5:</a> FBT001 Boolean-typed positional argument in function definition
... 23 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/wntrblm/nox">wntrblm/nox</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/wntrblm/nox/blob/382fc8c36292056d315929dad59cb12e74bdfb92/nox/_decorators.py#L72'>nox/_decorators.py:72:9:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/wntrblm/nox/blob/382fc8c36292056d315929dad59cb12e74bdfb92/nox/_option_set.py#L166'>nox/_option_set.py:166:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/wntrblm/nox/blob/382fc8c36292056d315929dad59cb12e74bdfb92/nox/registry.py#L50'>nox/registry.py:50:5:</a> FBT001 Boolean-typed positional argument in function definition
+ <a href='https://github.com/wntrblm/nox/blob/382fc8c36292056d315929dad59cb12e74bdfb92/nox/registry.py#L66'>nox/registry.py:66:5:</a> FBT001 Boolean-typed positional argument in function definition
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FBT001 | 226 | 226 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-06 23:28_

---

_@ntBre approved on 2025-06-08 15:57_

---

_Merged by @dylwil3 on 2025-06-08 18:07_

---

_Closed by @dylwil3 on 2025-06-08 18:07_

---

_Branch deleted on 2025-06-08 18:08_

---

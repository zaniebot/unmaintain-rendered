```yaml
number: 9345
title: Short rule messages should not end with a period
type: pull_request
state: merged
author: DimitriPapadopoulos
labels:
  - cli
assignees: []
merged: true
base: main
head: full_stop
created_at: 2024-01-01T13:54:13Z
updated_at: 2024-01-02T08:22:22Z
url: https://github.com/astral-sh/ruff/pull/9345
synced_at: 2026-01-12T15:55:28Z
```

# Short rule messages should not end with a period

---

_@DimitriPapadopoulos_

## Summary

Remove the period from a couple short messages, for consistency with all other short messages.

All other short rule messages lack such a period, except for long messages made of multiple sentences.

## Test Plan

Tests modified accordingly.

Not sure if this would qualify as a breaking change because user-visible messages are modified.

---

_Comment by @github-actions[bot] on 2024-01-01 14:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+38 -38 violations, +0 -0 fixes in 3 projects; 38 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+16 -16 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/airflow/providers/amazon/aws/executors/ecs/ecs_executor.py#L228'>airflow/providers/amazon/aws/executors/ecs/ecs_executor.py:228:30:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/airflow/providers/amazon/aws/executors/ecs/ecs_executor.py#L228'>airflow/providers/amazon/aws/executors/ecs/ecs_executor.py:228:30:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/airflow/providers/amazon/aws/executors/ecs/utils.py#L142'>airflow/providers/amazon/aws/executors/ecs/utils.py:142:30:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/airflow/providers/amazon/aws/executors/ecs/utils.py#L142'>airflow/providers/amazon/aws/executors/ecs/utils.py:142:30:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/airflow/providers/amazon/aws/executors/ecs/utils.py#L148'>airflow/providers/amazon/aws/executors/ecs/utils.py:148:40:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/airflow/providers/amazon/aws/executors/ecs/utils.py#L148'>airflow/providers/amazon/aws/executors/ecs/utils.py:148:40:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/airflow/www/extensions/init_views.py#L246'>airflow/www/extensions/init_views.py:246:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/airflow/www/extensions/init_views.py#L246'>airflow/www/extensions/init_views.py:246:16:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/airflow/www/extensions/init_views.py#L257'>airflow/www/extensions/init_views.py:257:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/airflow/www/extensions/init_views.py#L257'>airflow/www/extensions/init_views.py:257:16:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/dev/breeze/src/airflow_breeze/commands/testing_commands.py#L681'>dev/breeze/src/airflow_breeze/commands/testing_commands.py:681:20:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/dev/breeze/src/airflow_breeze/commands/testing_commands.py#L681'>dev/breeze/src/airflow_breeze/commands/testing_commands.py:681:20:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1025'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1025:20:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1025'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1025:20:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L386'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:386:13:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L386'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:386:13:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L978'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:978:13:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L978'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:978:13:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L998'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:998:13:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L998'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:998:13:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/tests/api_connexion/endpoints/test_connection_endpoint.py#L628'>tests/api_connexion/endpoints/test_connection_endpoint.py:628:24:</a> C419 Unnecessary list comprehension
... 11 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+21 -21 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/CofenseTriage/Integrations/CofenseTriagev2/CofenseTriagev2.py#L595'>Packs/CofenseTriage/Integrations/CofenseTriagev2/CofenseTriagev2.py:595:39:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/CofenseTriage/Integrations/CofenseTriagev2/CofenseTriagev2.py#L595'>Packs/CofenseTriage/Integrations/CofenseTriagev2/CofenseTriagev2.py:595:39:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/CofenseTriage/Integrations/CofenseTriagev3/CofenseTriagev3.py#L261'>Packs/CofenseTriage/Integrations/CofenseTriagev3/CofenseTriagev3.py:261:24:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/CofenseTriage/Integrations/CofenseTriagev3/CofenseTriagev3.py#L261'>Packs/CofenseTriage/Integrations/CofenseTriagev3/CofenseTriagev3.py:261:24:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/ExtraHop/Integrations/ExtraHop_v2/ExtraHop_v2.py#L300'>Packs/ExtraHop/Integrations/ExtraHop_v2/ExtraHop_v2.py:300:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/ExtraHop/Integrations/ExtraHop_v2/ExtraHop_v2.py#L300'>Packs/ExtraHop/Integrations/ExtraHop_v2/ExtraHop_v2.py:300:16:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/FeedProofpoint/Integrations/FeedProofpoint/FeedProofpoint_test.py#L35'>Packs/FeedProofpoint/Integrations/FeedProofpoint/FeedProofpoint_test.py:35:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/FeedProofpoint/Integrations/FeedProofpoint/FeedProofpoint_test.py#L35'>Packs/FeedProofpoint/Integrations/FeedProofpoint/FeedProofpoint_test.py:35:16:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/FeedProofpoint/Integrations/FeedProofpoint/FeedProofpoint_test.py#L36'>Packs/FeedProofpoint/Integrations/FeedProofpoint/FeedProofpoint_test.py:36:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/FeedProofpoint/Integrations/FeedProofpoint/FeedProofpoint_test.py#L36'>Packs/FeedProofpoint/Integrations/FeedProofpoint/FeedProofpoint_test.py:36:16:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/FiltersAndTransformers/Scripts/ParseHTMLTables/ParseHTMLTables.py#L247'>Packs/FiltersAndTransformers/Scripts/ParseHTMLTables/ParseHTMLTables.py:247:62:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/FiltersAndTransformers/Scripts/ParseHTMLTables/ParseHTMLTables.py#L247'>Packs/FiltersAndTransformers/Scripts/ParseHTMLTables/ParseHTMLTables.py:247:62:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/MailSenderNew/Integrations/MailSenderNew/MailSenderNew_test.py#L126'>Packs/MailSenderNew/Integrations/MailSenderNew/MailSenderNew_test.py:126:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/MailSenderNew/Integrations/MailSenderNew/MailSenderNew_test.py#L126'>Packs/MailSenderNew/Integrations/MailSenderNew/MailSenderNew_test.py:126:16:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/MailSenderNew/Integrations/MailSenderNew/MailSenderNew_test.py#L127'>Packs/MailSenderNew/Integrations/MailSenderNew/MailSenderNew_test.py:127:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/MailSenderNew/Integrations/MailSenderNew/MailSenderNew_test.py#L127'>Packs/MailSenderNew/Integrations/MailSenderNew/MailSenderNew_test.py:127:16:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/McAfee_ESM-v10/Integrations/McAfee_ESM-v10/McAfee_ESM-v10.py#L422'>Packs/McAfee_ESM-v10/Integrations/McAfee_ESM-v10/McAfee_ESM-v10.py:422:68:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/McAfee_ESM-v10/Integrations/McAfee_ESM-v10/McAfee_ESM-v10.py#L422'>Packs/McAfee_ESM-v10/Integrations/McAfee_ESM-v10/McAfee_ESM-v10.py:422:68:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/McAfee_ESM-v10/Integrations/McAfee_ESM-v10/McAfee_ESM-v10.py#L436'>Packs/McAfee_ESM-v10/Integrations/McAfee_ESM-v10/McAfee_ESM-v10.py:436:17:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/McAfee_ESM-v10/Integrations/McAfee_ESM-v10/McAfee_ESM-v10.py#L436'>Packs/McAfee_ESM-v10/Integrations/McAfee_ESM-v10/McAfee_ESM-v10.py:436:17:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py#L122'>Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py:122:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py#L122'>Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py:122:16:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py#L227'>Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py:227:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py#L227'>Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py:227:16:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py#L410'>Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py:410:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py#L410'>Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py:410:16:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py#L47'>Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py:47:16:</a> C419 Unnecessary list comprehension
... 15 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/e9fdcb0a64e77eddfca52ffbcc794c18f7091159/latch_cli/snakemake/config/utils.py#L98'>latch_cli/snakemake/config/utils.py:98:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/latchbio/latch/blob/e9fdcb0a64e77eddfca52ffbcc794c18f7091159/latch_cli/snakemake/config/utils.py#L98'>latch_cli/snakemake/config/utils.py:98:16:</a> C419 Unnecessary list comprehension.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| C419 | 76 | 38 | 38 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+37 -37 violations, +0 -0 fixes in 3 projects; 38 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+15 -15 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/airflow/providers/amazon/aws/executors/ecs/ecs_executor.py#L228'>airflow/providers/amazon/aws/executors/ecs/ecs_executor.py:228:30:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/airflow/providers/amazon/aws/executors/ecs/ecs_executor.py#L228'>airflow/providers/amazon/aws/executors/ecs/ecs_executor.py:228:30:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/airflow/providers/amazon/aws/executors/ecs/utils.py#L142'>airflow/providers/amazon/aws/executors/ecs/utils.py:142:30:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/airflow/providers/amazon/aws/executors/ecs/utils.py#L142'>airflow/providers/amazon/aws/executors/ecs/utils.py:142:30:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/airflow/providers/amazon/aws/executors/ecs/utils.py#L148'>airflow/providers/amazon/aws/executors/ecs/utils.py:148:40:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/airflow/providers/amazon/aws/executors/ecs/utils.py#L148'>airflow/providers/amazon/aws/executors/ecs/utils.py:148:40:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/airflow/www/extensions/init_views.py#L246'>airflow/www/extensions/init_views.py:246:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/airflow/www/extensions/init_views.py#L246'>airflow/www/extensions/init_views.py:246:16:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/airflow/www/extensions/init_views.py#L257'>airflow/www/extensions/init_views.py:257:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/airflow/www/extensions/init_views.py#L257'>airflow/www/extensions/init_views.py:257:16:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/dev/breeze/src/airflow_breeze/commands/testing_commands.py#L681'>dev/breeze/src/airflow_breeze/commands/testing_commands.py:681:20:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/dev/breeze/src/airflow_breeze/commands/testing_commands.py#L681'>dev/breeze/src/airflow_breeze/commands/testing_commands.py:681:20:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1025'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1025:20:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L1025'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:1025:20:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L386'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:386:13:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L386'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:386:13:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L978'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:978:13:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L978'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:978:13:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L998'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:998:13:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/apache/airflow/blob/326ae6bdc258274ae8a0c653ca08ace9c3033c18/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L998'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:998:13:</a> C419 Unnecessary list comprehension.
... 10 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+21 -21 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/CofenseTriage/Integrations/CofenseTriagev2/CofenseTriagev2.py#L595'>Packs/CofenseTriage/Integrations/CofenseTriagev2/CofenseTriagev2.py:595:39:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/CofenseTriage/Integrations/CofenseTriagev2/CofenseTriagev2.py#L595'>Packs/CofenseTriage/Integrations/CofenseTriagev2/CofenseTriagev2.py:595:39:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/CofenseTriage/Integrations/CofenseTriagev3/CofenseTriagev3.py#L261'>Packs/CofenseTriage/Integrations/CofenseTriagev3/CofenseTriagev3.py:261:24:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/CofenseTriage/Integrations/CofenseTriagev3/CofenseTriagev3.py#L261'>Packs/CofenseTriage/Integrations/CofenseTriagev3/CofenseTriagev3.py:261:24:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/ExtraHop/Integrations/ExtraHop_v2/ExtraHop_v2.py#L300'>Packs/ExtraHop/Integrations/ExtraHop_v2/ExtraHop_v2.py:300:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/ExtraHop/Integrations/ExtraHop_v2/ExtraHop_v2.py#L300'>Packs/ExtraHop/Integrations/ExtraHop_v2/ExtraHop_v2.py:300:16:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/FeedProofpoint/Integrations/FeedProofpoint/FeedProofpoint_test.py#L35'>Packs/FeedProofpoint/Integrations/FeedProofpoint/FeedProofpoint_test.py:35:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/FeedProofpoint/Integrations/FeedProofpoint/FeedProofpoint_test.py#L35'>Packs/FeedProofpoint/Integrations/FeedProofpoint/FeedProofpoint_test.py:35:16:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/FeedProofpoint/Integrations/FeedProofpoint/FeedProofpoint_test.py#L36'>Packs/FeedProofpoint/Integrations/FeedProofpoint/FeedProofpoint_test.py:36:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/FeedProofpoint/Integrations/FeedProofpoint/FeedProofpoint_test.py#L36'>Packs/FeedProofpoint/Integrations/FeedProofpoint/FeedProofpoint_test.py:36:16:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/FiltersAndTransformers/Scripts/ParseHTMLTables/ParseHTMLTables.py#L247'>Packs/FiltersAndTransformers/Scripts/ParseHTMLTables/ParseHTMLTables.py:247:62:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/FiltersAndTransformers/Scripts/ParseHTMLTables/ParseHTMLTables.py#L247'>Packs/FiltersAndTransformers/Scripts/ParseHTMLTables/ParseHTMLTables.py:247:62:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/MailSenderNew/Integrations/MailSenderNew/MailSenderNew_test.py#L126'>Packs/MailSenderNew/Integrations/MailSenderNew/MailSenderNew_test.py:126:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/MailSenderNew/Integrations/MailSenderNew/MailSenderNew_test.py#L126'>Packs/MailSenderNew/Integrations/MailSenderNew/MailSenderNew_test.py:126:16:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/MailSenderNew/Integrations/MailSenderNew/MailSenderNew_test.py#L127'>Packs/MailSenderNew/Integrations/MailSenderNew/MailSenderNew_test.py:127:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/MailSenderNew/Integrations/MailSenderNew/MailSenderNew_test.py#L127'>Packs/MailSenderNew/Integrations/MailSenderNew/MailSenderNew_test.py:127:16:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/McAfee_ESM-v10/Integrations/McAfee_ESM-v10/McAfee_ESM-v10.py#L422'>Packs/McAfee_ESM-v10/Integrations/McAfee_ESM-v10/McAfee_ESM-v10.py:422:68:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/McAfee_ESM-v10/Integrations/McAfee_ESM-v10/McAfee_ESM-v10.py#L422'>Packs/McAfee_ESM-v10/Integrations/McAfee_ESM-v10/McAfee_ESM-v10.py:422:68:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/McAfee_ESM-v10/Integrations/McAfee_ESM-v10/McAfee_ESM-v10.py#L436'>Packs/McAfee_ESM-v10/Integrations/McAfee_ESM-v10/McAfee_ESM-v10.py:436:17:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/McAfee_ESM-v10/Integrations/McAfee_ESM-v10/McAfee_ESM-v10.py#L436'>Packs/McAfee_ESM-v10/Integrations/McAfee_ESM-v10/McAfee_ESM-v10.py:436:17:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py#L122'>Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py:122:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py#L122'>Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py:122:16:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py#L227'>Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py:227:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py#L227'>Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py:227:16:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py#L410'>Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py:410:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py#L410'>Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py:410:16:</a> C419 Unnecessary list comprehension.
+ <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py#L47'>Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py:47:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/demisto/content/blob/a0d4f9622c2d4ffda2d76b041ac34be24ad5b4af/Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py#L47'>Packs/NetscoutAED/Integrations/NetscoutAED/NetscoutAED_test.py:47:16:</a> C419 Unnecessary list comprehension.
... 14 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/e9fdcb0a64e77eddfca52ffbcc794c18f7091159/latch_cli/snakemake/config/utils.py#L98'>latch_cli/snakemake/config/utils.py:98:16:</a> C419 Unnecessary list comprehension
- <a href='https://github.com/latchbio/latch/blob/e9fdcb0a64e77eddfca52ffbcc794c18f7091159/latch_cli/snakemake/config/utils.py#L98'>latch_cli/snakemake/config/utils.py:98:16:</a> C419 Unnecessary list comprehension.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| C419 | 74 | 37 | 37 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @DimitriPapadopoulos on 2024-01-01 14:06_

---

_Label `cli` added by @charliermarsh on 2024-01-02 01:58_

---

_Comment by @charliermarsh on 2024-01-02 01:58_

Thanks! This is correct IMO.

---

_Merged by @charliermarsh on 2024-01-02 01:58_

---

_Closed by @charliermarsh on 2024-01-02 01:58_

---

_Branch deleted on 2024-01-02 08:22_

---

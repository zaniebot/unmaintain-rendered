```yaml
number: 4269
title: "RUF010: New rule for removing blank comments"
type: pull_request
state: closed
author: andreyfedoseev
labels: []
assignees: []
base: main
head: RUF010-blank-comments
created_at: 2023-05-07T14:41:46Z
updated_at: 2023-07-21T03:35:28Z
url: https://github.com/astral-sh/ruff/pull/4269
synced_at: 2026-01-12T15:55:15Z
```

# RUF010: New rule for removing blank comments

---

_@andreyfedoseev_

Feature proposal: https://github.com/charliermarsh/ruff/issues/4238

---

_Comment by @github-actions[bot] on 2023-05-07 14:52_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+239, -0, 0 error(s))

<details><summary>airflow (+77, -0)</summary>
<p>

```diff
+ airflow/config_templates/airflow_local_settings.py:189:18: RUF010 [*] Blank comments are useless and should be removed
+ airflow/config_templates/airflow_local_settings.py:190:17: RUF010 [*] Blank comments are useless and should be removed
+ airflow/config_templates/airflow_local_settings.py:191:18: RUF010 [*] Blank comments are useless and should be removed
+ airflow/migrations/db_types.py:24:38: RUF010 [*] Blank comments are useless and should be removed
+ airflow/migrations/db_types.py:30:38: RUF010 [*] Blank comments are useless and should be removed
+ airflow/www/security.py:157:79: RUF010 [*] Blank comments are useless and should be removed
+ airflow/www/security.py:159:79: RUF010 [*] Blank comments are useless and should be removed
+ airflow/www/security.py:63:79: RUF010 [*] Blank comments are useless and should be removed
+ airflow/www/security.py:65:79: RUF010 [*] Blank comments are useless and should be removed
+ airflow/www/views.py:4089:86: RUF010 [*] Blank comments are useless and should be removed
+ airflow/www/views.py:4091:86: RUF010 [*] Blank comments are useless and should be removed
+ airflow/www/views.py:556:86: RUF010 [*] Blank comments are useless and should be removed
+ airflow/www/views.py:558:86: RUF010 [*] Blank comments are useless and should be removed
+ airflow/www/views.py:605:86: RUF010 [*] Blank comments are useless and should be removed
+ airflow/www/views.py:607:86: RUF010 [*] Blank comments are useless and should be removed
+ setup.py:505:109: RUF010 [*] Blank comments are useless and should be removed
+ setup.py:507:109: RUF010 [*] Blank comments are useless and should be removed
+ setup.py:571:109: RUF010 [*] Blank comments are useless and should be removed
+ setup.py:573:109: RUF010 [*] Blank comments are useless and should be removed
+ tests/providers/amazon/aws/hooks/test_eks.py:228:7: RUF010 [*] Blank comments are useless and should be removed
+ tests/providers/amazon/aws/hooks/test_eks.py:233:7: RUF010 [*] Blank comments are useless and should be removed
+ tests/providers/google/cloud/operators/test_bigquery.py:662:65: RUF010 [*] Blank comments are useless and should be removed
+ tests/providers/google/cloud/operators/test_bigquery.py:664:65: RUF010 [*] Blank comments are useless and should be removed
+ tests/providers/google/cloud/operators/test_bigquery.py:699:65: RUF010 [*] Blank comments are useless and should be removed
+ tests/providers/google/cloud/operators/test_bigquery.py:701:65: RUF010 [*] Blank comments are useless and should be removed
+ tests/serialization/test_dag_serialization.py:980:65: RUF010 [*] Blank comments are useless and should be removed
+ tests/serialization/test_dag_serialization.py:982:65: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/bigquery/example_bigquery_queries_async.py:260:63: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/bigquery/example_bigquery_queries_async.py:261:43: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_memorystore/example_cloud_memorystore_memcached.py:141:63: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_memorystore/example_cloud_memorystore_memcached.py:142:43: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_memorystore/example_cloud_memorystore_redis.py:262:63: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_memorystore/example_cloud_memorystore_redis.py:263:43: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_sql/example_cloud_sql.py:153:53: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_sql/example_cloud_sql.py:154:53: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_sql/example_cloud_sql.py:155:53: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_sql/example_cloud_sql.py:169:53: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_sql/example_cloud_sql.py:170:53: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_sql/example_cloud_sql.py:171:53: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_sql/example_cloud_sql.py:194:53: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_sql/example_cloud_sql.py:195:53: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_sql/example_cloud_sql.py:196:53: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_sql/example_cloud_sql.py:236:53: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_sql/example_cloud_sql.py:237:53: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_sql/example_cloud_sql.py:238:53: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_sql/example_cloud_sql.py:245:53: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_sql/example_cloud_sql.py:246:53: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_sql/example_cloud_sql.py:247:53: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_sql/example_cloud_sql.py:256:53: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_sql/example_cloud_sql.py:257:53: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_sql/example_cloud_sql.py:258:53: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_sql/example_cloud_sql.py:316:63: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/cloud_sql/example_cloud_sql.py:317:43: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/compute/example_compute.py:274:63: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/compute/example_compute.py:275:43: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/compute/example_compute_igm.py:242:63: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/compute/example_compute_igm.py:243:43: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/compute/example_compute_ssh.py:117:63: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/compute/example_compute_ssh.py:118:43: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/dataform/example_dataform.py:296:63: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/dataform/example_dataform.py:297:43: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/pubsub/example_pubsub.py:146:63: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/pubsub/example_pubsub.py:147:43: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/spanner/example_spanner.py:159:63: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/spanner/example_spanner.py:160:43: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/translate/example_translate.py:56:63: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/translate/example_translate.py:57:43: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/workflows/example_workflows.py:231:63: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/cloud/workflows/example_workflows.py:232:43: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/datacatalog/example_datacatalog_entries.py:199:63: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/datacatalog/example_datacatalog_entries.py:200:43: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/datacatalog/example_datacatalog_search_catalog.py:220:63: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/datacatalog/example_datacatalog_search_catalog.py:221:43: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/datacatalog/example_datacatalog_tag_templates.py:182:63: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/datacatalog/example_datacatalog_tag_templates.py:183:43: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/datacatalog/example_datacatalog_tags.py:232:63: RUF010 [*] Blank comments are useless and should be removed
+ tests/system/providers/google/datacatalog/example_datacatalog_tags.py:233:43: RUF010 [*] Blank comments are useless and should be removed
```

</p>
</details>
<details><summary>disnake (+10, -0)</summary>
<p>

```diff
+ disnake/types/gateway.py:155:5: RUF010 [*] Blank comments are useless and should be removed
+ disnake/types/gateway.py:157:5: RUF010 [*] Blank comments are useless and should be removed
+ disnake/types/gateway.py:177:5: RUF010 [*] Blank comments are useless and should be removed
+ disnake/types/gateway.py:179:5: RUF010 [*] Blank comments are useless and should be removed
+ disnake/types/gateway.py:243:5: RUF010 [*] Blank comments are useless and should be removed
+ disnake/types/gateway.py:245:5: RUF010 [*] Blank comments are useless and should be removed
+ disnake/types/gateway.py:46:5: RUF010 [*] Blank comments are useless and should be removed
+ disnake/types/gateway.py:48:5: RUF010 [*] Blank comments are useless and should be removed
+ disnake/types/gateway.py:58:5: RUF010 [*] Blank comments are useless and should be removed
+ disnake/types/gateway.py:60:5: RUF010 [*] Blank comments are useless and should be removed
```

</p>
</details>
<details><summary>zulip (+152, -0)</summary>
<p>

```diff
+ analytics/lib/counts.py:105:32: RUF010 [*] Blank comments are useless and should be removed
+ analytics/lib/counts.py:25:19: RUF010 [*] Blank comments are useless and should be removed
+ analytics/lib/counts.py:289:53: RUF010 [*] Blank comments are useless and should be removed
+ analytics/lib/counts.py:33:23: RUF010 [*] Blank comments are useless and should be removed
+ analytics/lib/counts.py:347:36: RUF010 [*] Blank comments are useless and should be removed
+ analytics/lib/counts.py:705:32: RUF010 [*] Blank comments are useless and should be removed
+ analytics/views/installation_activity.py:406:7: RUF010 [*] Blank comments are useless and should be removed
+ analytics/views/installation_activity.py:443:7: RUF010 [*] Blank comments are useless and should be removed
+ analytics/views/installation_activity.py:475:7: RUF010 [*] Blank comments are useless and should be removed
+ analytics/views/installation_activity.py:515:7: RUF010 [*] Blank comments are useless and should be removed
+ zerver/lib/email_mirror.py:181:23: RUF010 [*] Blank comments are useless and should be removed
+ zerver/lib/send_email.py:36:19: RUF010 [*] Blank comments are useless and should be removed
+ zerver/management/commands/deliver_scheduled_emails.py:21:11: RUF010 [*] Blank comments are useless and should be removed
+ zerver/management/commands/deliver_scheduled_messages.py:16:11: RUF010 [*] Blank comments are useless and should be removed
+ zerver/management/commands/email_mirror.py:32:11: RUF010 [*] Blank comments are useless and should be removed
+ zerver/management/commands/enqueue_digest_emails.py:12:19: RUF010 [*] Blank comments are useless and should be removed
+ zerver/management/commands/sync_ldap_user_data.py:14:11: RUF010 [*] Blank comments are useless and should be removed
+ zerver/models.py:1503:29: RUF010 [*] Blank comments are useless and should be removed
+ zerver/models.py:1579:35: RUF010 [*] Blank comments are useless and should be removed
+ zerver/tests/test_users.py:326:69: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:1007:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:1009:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:1056:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:1058:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:1140:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:1142:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:1172:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:1174:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:131:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:133:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:239:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:241:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:319:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:321:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:326:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:328:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:359:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:361:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:390:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:392:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:434:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:436:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:517:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:519:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:524:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:526:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:569:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:571:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:644:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:646:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:69:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/computed_settings.py:71:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/configured_settings.py:1:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/configured_settings.py:3:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:112:16: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:114:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:11:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:122:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:134:16: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:136:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:140:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:14:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:158:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:166:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:184:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:189:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:194:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:19:16: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:208:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:21:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:224:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:236:6: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:239:6: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:269:8: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:271:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:273:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:277:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:284:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:290:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:296:8: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:298:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:300:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:317:8: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:319:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:321:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:328:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:351:8: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:353:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:355:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:386:8: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:388:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:38:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:392:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:394:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:410:10: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:426:10: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:429:10: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:434:10: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:439:10: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:443:10: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:447:10: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:467:6: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:470:6: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:48:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:497:8: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:499:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:501:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:509:8: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:511:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:513:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:517:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:521:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:524:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:529:8: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:52:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:531:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:540:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:544:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:560:16: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:563:8: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:565:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:576:8: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:578:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:592:8: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:594:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:5:64: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:604:8: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:606:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:617:16: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:620:8: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:622:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:62:16: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:633:8: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:635:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:641:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:649:16: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:64:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:651:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:673:17: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:678:16: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:67:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:680:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:696:16: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:69:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:716:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:718:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:746:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:748:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:7:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/prod_settings_template.py:802:2: RUF010 [*] Blank comments are useless and should be removed
+ zproject/settings.py:17:72: RUF010 [*] Blank comments are useless and should be removed
+ zproject/settings.py:2:72: RUF010 [*] Blank comments are useless and should be removed
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.7±0.07ms     2.8 MB/sec    1.01     14.8±0.04ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.7 MB/sec    1.01      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    361.0±1.28µs     8.2 MB/sec    1.03    370.4±1.12µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.02      6.2±0.04ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.01ms     5.5 MB/sec    1.01      7.5±0.00ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1537.3±11.62µs    10.8 MB/sec    1.02   1572.7±2.79µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.9±0.29µs    17.9 MB/sec    1.03    170.4±0.50µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.00ms     7.7 MB/sec    1.02      3.3±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.01      5.9±0.02ms     6.9 MB/sec    1.00      5.9±0.01ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.01   1155.4±3.83µs    14.4 MB/sec    1.00   1146.2±4.85µs    14.5 MB/sec
parser/numpy/globals.py                    1.01    118.0±1.83µs    25.0 MB/sec    1.00    116.6±0.59µs    25.3 MB/sec
parser/pydantic/types.py                   1.02      2.5±0.03ms    10.1 MB/sec    1.00      2.5±0.00ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.3±0.12ms     2.5 MB/sec    1.00     16.3±0.14ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.0 MB/sec    1.00      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    486.8±5.34µs     6.1 MB/sec    1.00    485.5±8.56µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.06ms     3.7 MB/sec    1.00      6.9±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.02      8.4±0.11ms     4.8 MB/sec    1.00      8.3±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1781.3±27.81µs     9.3 MB/sec    1.00  1760.1±18.25µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.7±3.27µs    15.2 MB/sec    1.02    199.3±6.73µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.8 MB/sec    1.00      3.7±0.05ms     6.8 MB/sec
parser/large/dataset.py                    1.01      6.7±0.05ms     6.0 MB/sec    1.00      6.7±0.03ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1285.4±17.97µs    13.0 MB/sec    1.00  1280.7±14.57µs    13.0 MB/sec
parser/numpy/globals.py                    1.01    132.4±2.18µs    22.3 MB/sec    1.00    131.3±1.55µs    22.5 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.02ms     8.9 MB/sec    1.00      2.8±0.02ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-05-07 17:33_

Thanks for this! Looking through the codebases, I think most of the errors flagged in the ecosystem check are false positives. Could we try implementing this by looking at the token stream, rather than using a regex on the physical lines? (A regex will also be more expensive in practice.)

E.g., in the `tokens.rs` checker, we could look at `Comment` tokens, see if they're blank, and track some state to see if they're part of a multi-line comment. Would that work?

---

_Review requested from @charliermarsh by @charliermarsh on 2023-05-07 17:33_

---

_Comment by @MichaReiser on 2023-05-08 06:18_

> Could we try implementing this by looking at the token stream, rather than using a regex on the physical lines? (A regex will also be more expensive in practice.)

It should be possible to use `Indexer.comment_ranges` to avoid iterating over all tokens. 

---

_Comment by @charliermarsh on 2023-05-12 16:17_

As an aside, it looks like this can be implemented as `PLR2044`: https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/empty-comment.html

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/blank_comment.rs`:51 on 2023-05-13 12:21_

Ruff has the `Indexer` data structure that exposes the range of all comments (`Indexer::comment_ranges`). You can use this with `Locator.slice(range)` to retrieve all comments and call the rule at the top level (module) once. This should be faster than running a regex on every line. 

I think we should also be able to replace the regex all together by manually skipping the `#` and then testing if `text.trim().is_empty()`.



---

_@MichaReiser reviewed on 2023-05-13 12:21_

---

_@MichaReiser reviewed on 2023-05-13 12:22_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/snapshots/ruff__rules__ruff__tests__RUF010_RUF010.py.snap`:9 on 2023-05-13 12:22_

Nit: Can we change the range to only highlight the comment

---

_Review comment by @sladyn98 on `crates/ruff/src/rules/ruff/rules/blank_comment.rs`:51 on 2023-05-22 07:24_

Yeah I guess we could do something like this

```
// Retrieve the comment using Locator.slice
        let comment = Locator.slice(range);

        // Skip the # at the start of the comment and trim white spaces
        let trimmed_comment = comment.strip_prefix("#").unwrap_or("").trim();

        // If the trimmed comment is empty, it's a blank comment
        if trimmed_comment.is_empty() {
            let mut diagnostic = Diagnostic::new(BlankComment, range);
            if autofix.into() && settings.rules.should_fix(Rule::BlankComment) {
                diagnostic.set_fix(Edit::deletion(range.start(), range.end()));
            }
            diagnostics.push(diagnostic);
        }
    }
    
 ```

---

_@sladyn98 reviewed on 2023-05-22 07:24_

---

_Comment by @andreyfedoseev on 2023-05-22 08:53_

Below are a few examples that make it somewhat tricky:

1. Basic case, blank comment follows some code, comment should be removed

```
print("hello")  #
```

2. Multi-line comments, blank lines should be preserved:

```
# First line
# 
#
# Last line
# 
```

3. However, blank multi-line comments should be removed:

```
# The blank comment below should be removed

#
#

#
# This should not be removed
#

```

4. Also, we need to be careful about what we consider a multi line comment

```
# The comment on the next line is not part of a multi-line comment
print("hello")  #
```

For that we need to keep track two types of context for each comment token:

1. Does a comment token follow a code fragment?
2. Is a comment token a part of a continuous multi-line comment?

---

_Comment by @sladyn98 on 2023-05-22 09:11_

We could use a commentContext for something like this
```
pub struct CommentContext {
    is_comment_continuous: bool,
    is_comment_after_code: bool,
}

pub fn check_blank_comment(
    diagnostics: &mut Vec<Diagnostic>,
    line: &Line,
    settings: &Settings,
    autofix: flags::Autofix,
    context: &mut CommentContext,
) {
    // Retrieve the comment using Locator.slice
    let comment = Locator.slice(range);

    // Skip the # at the start of the comment and trim white spaces
    let trimmed_comment = comment.strip_prefix("#").unwrap_or("").trim();

    // Flag to check if current comment is blank
    let is_blank = trimmed_comment.is_empty();

    // If the trimmed comment is empty and either not part of continuous comment or after a code, it's a blank comment to be removed
    if is_blank && (!context.is_comment_continuous || context.is_comment_after_code) {
        let mut diagnostic = Diagnostic::new(BlankComment, range);
        if autofix.into() && settings.rules.should_fix(Rule::BlankComment) {
            diagnostic.set_fix(Edit::deletion(range.start(), range.end()));
        }
        diagnostics.push(diagnostic);
    }

    // Update the context for the next line
    context.is_comment_continuous = !is_blank;
    context.is_comment_after_code = false;
}

// You'll need to handle these two context flags while parsing the whole script

```
Not sure how will we store the context for each comment token

---

_Comment by @MichaReiser on 2023-05-22 09:20_

@andreyfedoseev I think the todo rule does something very similar. It checks whether two comments are in a row and if it contains the necessary metadata

https://github.com/charliermarsh/ruff/blob/c6e5fed6587c95839a814ee8dfa845b465351229/crates/ruff/src/rules/flake8_todos/rules.rs#L293-L339

---

_Comment by @charliermarsh on 2023-07-21 03:35_

Gonna close this out for now since it's been inactive for a bit -- aways happy to re-open if we decide to pick the work back up :)

---

_Closed by @charliermarsh on 2023-07-21 03:35_

---

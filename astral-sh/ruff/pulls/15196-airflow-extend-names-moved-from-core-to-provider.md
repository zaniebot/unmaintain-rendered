```yaml
number: 15196
title: "[`airflow`] Extend names moved from core to provider (`AIR303`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extend-AIR303
created_at: 2024-12-30T06:23:10Z
updated_at: 2025-01-02T04:45:00Z
url: https://github.com/astral-sh/ruff/pull/15196
synced_at: 2026-01-12T15:55:50Z
```

# [`airflow`] Extend names moved from core to provider (`AIR303`)

---

_@Lee-W_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->


## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Many core Airflow features have been deprecated and moved to Airflow Providers since users might need to install an additional package (e.g., `apache-airflow-provider-fab==1.0.0`); a separate rule (AIR303) is created for this.

* `airflow.hooks.S3_hook.S3Hook` → `airflow.providers.amazon.aws.hooks.s3.S3Hook`
* `airflow.hooks.S3_hook.provide_bucket_name` → `airflow.providers.amazon.aws.hooks.s3.provide_bucket_name`
* `airflow.hooks.base_hook.BaseHook` → `airflow.hooks.base.BaseHook`
* `airflow.hooks.dbapi_hook.DbApiHook` → `airflow.providers.common.sql.hooks.sql.DbApiHook`
* `airflow.hooks.docker_hook.DockerHook` → `airflow.providers.docker.hooks.docker.DockerHook`
* `airflow.hooks.druid_hook.DruidDbApiHook` → `airflow.providers.apache.druid.hooks.druid.DruidDbApiHook`
* `airflow.hooks.druid_hook.DruidHook` → `airflow.providers.apache.druid.hooks.druid.DruidHook`
* `airflow.hooks.hive_hooks.HIVE_QUEUE_PRIORITIES` → `airflow.providers.apache.hive.hooks.hive.HIVE_QUEUE_PRIORITIES`
* `airflow.hooks.hive_hooks.HiveCliHook` → `airflow.providers.apache.hive.hooks.hive.HiveCliHook`
* `airflow.hooks.hive_hooks.HiveMetastoreHook` → `airflow.providers.apache.hive.hooks.hive.HiveMetastoreHook`
* `airflow.hooks.hive_hooks.HiveServer2Hook` → `airflow.providers.apache.hive.hooks.hive.HiveServer2Hook`
* `airflow.hooks.http_hook.HttpHook` → `airflow.providers.http.hooks.http.HttpHook`
* `airflow.hooks.jdbc_hook.JdbcHook` → `airflow.providers.jdbc.hooks.jdbc.JdbcHook`
* `airflow.hooks.jdbc_hook.jaydebeapi` → `airflow.providers.jdbc.hooks.jdbc.jaydebeapi`
* `airflow.hooks.mssql_hook.MsSqlHook` → `airflow.providers.microsoft.mssql.hooks.mssql.MsSqlHook`
* `airflow.hooks.mysql_hook.MySqlHook` → `airflow.providers.mysql.hooks.mysql.MySqlHook`
* `airflow.hooks.oracle_hook.OracleHook` → `airflow.providers.oracle.hooks.oracle.OracleHook`
* `airflow.hooks.pig_hook.PigCliHook` → `airflow.providers.apache.pig.hooks.pig.PigCliHook`
* `airflow.hooks.postgres_hook.PostgresHook` → `airflow.providers.postgres.hooks.postgres.PostgresHook`
* `airflow.hooks.presto_hook.PrestoHook` → `airflow.providers.presto.hooks.presto.PrestoHook`
* `airflow.hooks.samba_hook.SambaHook` → `airflow.providers.samba.hooks.samba.SambaHook`
* `airflow.hooks.slack_hook.SlackHook` → `airflow.providers.slack.hooks.slack.SlackHook`
* `airflow.hooks.sqlite_hook.SqliteHook` → `airflow.providers.sqlite.hooks.sqlite.SqliteHook`
* `airflow.hooks.webhdfs_hook.WebHDFSHook` → `airflow.providers.apache.hdfs.hooks.webhdfs.WebHDFSHook`
* `airflow.hooks.zendesk_hook.ZendeskHook` → `airflow.providers.zendesk.hooks.zendesk.ZendeskHook`
* `airflow.operators.check_operator.SQLCheckOperator` → `airflow.providers.common.sql.operators.sql.SQLCheckOperator`
* `airflow.operators.check_operator.SQLIntervalCheckOperator` → `airflow.providers.common.sql.operators.sql.SQLIntervalCheckOperator`
* `airflow.operators.check_operator.SQLThresholdCheckOperator` → `airflow.providers.common.sql.operators.sql.SQLThresholdCheckOperator`
* `airflow.operators.check_operator.SQLValueCheckOperator` → `airflow.providers.common.sql.operators.sql.SQLValueCheckOperator`
* `airflow.operators.check_operator.CheckOperator` → `airflow.providers.common.sql.operators.sql.SQLCheckOperator`
* `airflow.operators.check_operator.IntervalCheckOperator` → `airflow.providers.common.sql.operators.sql.SQLIntervalCheckOperator`
* `airflow.operators.check_operator.ThresholdCheckOperator` → `airflow.providers.common.sql.operators.sql.SQLThresholdCheckOperator`
* `airflow.operators.check_operator.ValueCheckOperator` → `airflow.providers.common.sql.operators.sql.SQLValueCheckOperator`
* `airflow.operators.docker_operator.DockerOperator` → `airflow.providers.docker.operators.docker.DockerOperator`
* `airflow.operators.druid_check_operator.DruidCheckOperator` → `airflow.providers.apache.druid.operators.druid_check.DruidCheckOperator`
* `airflow.operators.gcs_to_s3.GCSToS3Operator` → `airflow.providers.amazon.aws.transfers.gcs_to_s3.GCSToS3Operator`
* `airflow.operators.google_api_to_s3_transfer.GoogleApiToS3Operator` → `airflow.providers.amazon.aws.transfers.google_api_to_s3.GoogleApiToS3Operator`
* `airflow.operators.google_api_to_s3_transfer.GoogleApiToS3Transfer` → `airflow.providers.amazon.aws.transfers.google_api_to_s3.GoogleApiToS3Operator`
* `airflow.operators.hive_operator.HiveOperator` → `airflow.providers.apache.hive.operators.hive.HiveOperator`
* `airflow.operators.hive_stats_operator.HiveStatsCollectionOperator` → `airflow.providers.apache.hive.operators.hive_stats.HiveStatsCollectionOperator`
* `airflow.operators.hive_to_druid.HiveToDruidOperator` → `airflow.providers.apache.druid.transfers.hive_to_druid.HiveToDruidOperator`
* `airflow.operators.hive_to_druid.HiveToDruidTransfer` → `airflow.providers.apache.druid.transfers.hive_to_druid.HiveToDruidOperator`
* `airflow.operators.hive_to_mysql.HiveToMySqlOperator` → `airflow.providers.apache.hive.transfers.hive_to_mysql.HiveToMySqlOperator`
* `airflow.operators.hive_to_mysql.HiveToMySqlTransfer` → `airflow.providers.apache.hive.transfers.hive_to_mysql.HiveToMySqlOperator`
* `airflow.operators.hive_to_samba_operator.HiveToSambaOperator` → `airflow.providers.apache.hive.transfers.hive_to_samba.HiveToSambaOperator`
* `airflow.operators.http_operator.SimpleHttpOperator` → `airflow.providers.http.operators.http.SimpleHttpOperator`
* `airflow.operators.jdbc_operator.JdbcOperator` → `airflow.providers.jdbc.operators.jdbc.JdbcOperator`
* `airflow.operators.latest_only_operator.LatestOnlyOperator` → `airflow.operators.latest_only.LatestOnlyOperator`
* `airflow.operators.mssql_operator.MsSqlOperator` → `airflow.providers.microsoft.mssql.operators.mssql.MsSqlOperator`
* `airflow.operators.mssql_to_hive.MsSqlToHiveOperator` → `airflow.providers.apache.hive.transfers.mssql_to_hive.MsSqlToHiveOperator`
* `airflow.operators.mssql_to_hive.MsSqlToHiveTransfer` → `airflow.providers.apache.hive.transfers.mssql_to_hive.MsSqlToHiveOperator`
* `airflow.operators.mysql_operator.MySqlOperator` → `airflow.providers.mysql.operators.mysql.MySqlOperator`
* `airflow.operators.mysql_to_hive.MySqlToHiveOperator` → `airflow.providers.apache.hive.transfers.mysql_to_hive.MySqlToHiveOperator`
* `airflow.operators.mysql_to_hive.MySqlToHiveTransfer` → `airflow.providers.apache.hive.transfers.mysql_to_hive.MySqlToHiveOperator`
* `airflow.operators.oracle_operator.OracleOperator` → `airflow.providers.oracle.operators.oracle.OracleOperator`
* `airflow.operators.papermill_operator.PapermillOperator` → `airflow.providers.papermill.operators.papermill.PapermillOperator`
* `airflow.operators.pig_operator.PigOperator` → `airflow.providers.apache.pig.operators.pig.PigOperator`
* `airflow.operators.postgres_operator.Mapping` → `airflow.providers.postgres.operators.postgres.Mapping`
* `airflow.operators.postgres_operator.PostgresOperator` → `airflow.providers.postgres.operators.postgres.PostgresOperator`
* `airflow.operators.presto_check_operator.SQLCheckOperator` → `airflow.providers.common.sql.operators.sql.SQLCheckOperator`
* `airflow.operators.presto_check_operator.SQLIntervalCheckOperator` → `airflow.providers.common.sql.operators.sql.SQLIntervalCheckOperator`
* `airflow.operators.presto_check_operator.SQLValueCheckOperator` → `airflow.providers.common.sql.operators.sql.SQLValueCheckOperator`
* `airflow.operators.presto_check_operator.PrestoCheckOperator` → `airflow.providers.common.sql.operators.sql.SQLCheckOperator`
* `airflow.operators.presto_check_operator.PrestoIntervalCheckOperator` → `airflow.providers.common.sql.operators.sql.SQLIntervalCheckOperator`
* `airflow.operators.presto_check_operator.PrestoValueCheckOperator` → `airflow.providers.common.sql.operators.sql.SQLValueCheckOperator`
* `airflow.operators.presto_to_mysql.PrestoToMySqlOperator` → `airflow.providers.mysql.transfers.presto_to_mysql.PrestoToMySqlOperator`
* `airflow.operators.presto_to_mysql.PrestoToMySqlTransfer` → `airflow.providers.mysql.transfers.presto_to_mysql.PrestoToMySqlOperator`
* `airflow.operators.redshift_to_s3_operator.RedshiftToS3Operator` → `airflow.providers.amazon.aws.transfers.redshift_to_s3.RedshiftToS3Operator`
* `airflow.operators.redshift_to_s3_operator.RedshiftToS3Transfer` → `airflow.providers.amazon.aws.transfers.redshift_to_s3.RedshiftToS3Operator`
* `airflow.operators.s3_file_transform_operator.S3FileTransformOperator` → `airflow.providers.amazon.aws.operators.s3_file_transform.S3FileTransformOperator`
* `airflow.operators.s3_to_hive_operator.S3ToHiveOperator` → `airflow.providers.apache.hive.transfers.s3_to_hive.S3ToHiveOperator`
* `airflow.operators.s3_to_hive_operator.S3ToHiveTransfer` → `airflow.providers.apache.hive.transfers.s3_to_hive.S3ToHiveOperator`
* `airflow.operators.s3_to_redshift_operator.S3ToRedshiftOperator` → `airflow.providers.amazon.aws.transfers.s3_to_redshift.S3ToRedshiftOperator`
* `airflow.operators.s3_to_redshift_operator.S3ToRedshiftTransfer` → `airflow.providers.amazon.aws.transfers.s3_to_redshift.S3ToRedshiftOperator`
* `airflow.operators.slack_operator.SlackAPIOperator` → `airflow.providers.slack.operators.slack.SlackAPIOperator`
* `airflow.operators.slack_operator.SlackAPIPostOperator` → `airflow.providers.slack.operators.slack.SlackAPIPostOperator`
* `airflow.operators.sql.BaseSQLOperator` → `airflow.providers.common.sql.operators.sql.BaseSQLOperator`
* `airflow.operators.sql.BranchSQLOperator` → `airflow.providers.common.sql.operators.sql.BranchSQLOperator`
* `airflow.operators.sql.SQLCheckOperator` → `airflow.providers.common.sql.operators.sql.SQLCheckOperator`
* `airflow.operators.sql.SQLColumnCheckOperator` → `airflow.providers.common.sql.operators.sql.SQLColumnCheckOperator`
* `airflow.operators.sql.SQLIntervalCheckOperator` → `airflow.providers.common.sql.operators.sql.SQLIntervalCheckOperator`
* `airflow.operators.sql.SQLTableCheckOperator` → `airflow.providers.common.sql.operators.sql.SQLTableCheckOperator`
* `airflow.operators.sql.SQLThresholdCheckOperator` → `airflow.providers.common.sql.operators.sql.SQLThresholdCheckOperator`
* `airflow.operators.sql.SQLValueCheckOperator` → `airflow.providers.common.sql.operators.sql.SQLValueCheckOperator`
* `airflow.operators.sql._convert_to_float_if_possible` → `airflow.providers.common.sql.operators.sql._convert_to_float_if_possible`
* `airflow.operators.sql.parse_boolean` → `airflow.providers.common.sql.operators.sql.parse_boolean`
* `airflow.operators.sql_branch_operator.BranchSQLOperator` → `airflow.providers.common.sql.operators.sql.BranchSQLOperator`
* `airflow.operators.sql_branch_operator.BranchSqlOperator` → `airflow.providers.common.sql.operators.sql.BranchSQLOperator`
* `airflow.operators.sqlite_operator.SqliteOperator` → `airflow.providers.sqlite.operators.sqlite.SqliteOperator`
* `airflow.sensors.hive_partition_sensor.HivePartitionSensor` → `airflow.providers.apache.hive.sensors.hive_partition.HivePartitionSensor`
* `airflow.sensors.http_sensor.HttpSensor` → `airflow.providers.http.sensors.http.HttpSensor`
* `airflow.sensors.metastore_partition_sensor.MetastorePartitionSensor` → `airflow.providers.apache.hive.sensors.metastore_partition.MetastorePartitionSensor`
* `airflow.sensors.named_hive_partition_sensor.NamedHivePartitionSensor` → `airflow.providers.apache.hive.sensors.named_hive_partition.NamedHivePartitionSensor`
* `airflow.sensors.s3_key_sensor.S3KeySensor` → `airflow.providers.amazon.aws.sensors.s3.S3KeySensor`
* `airflow.sensors.sql.SqlSensor` → `airflow.providers.common.sql.sensors.sql.SqlSensor`
* `airflow.sensors.sql_sensor.SqlSensor` → `airflow.providers.common.sql.sensors.sql.SqlSensor`
* `airflow.sensors.web_hdfs_sensor.WebHdfsSensor` → `airflow.providers.apache.hdfs.sensors.web_hdfs.WebHdfsSensor`


## Test Plan

<!-- How was it tested? -->

A test fixture has been included for the rule.

---

_Renamed from "Extend air303" to "[airflow]: extend names moved from core to provider (AIR303)" by @Lee-W on 2024-12-30 06:24_

---

_Comment by @github-actions[bot] on 2024-12-30 06:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @Lee-W on 2024-12-31 09:56_

---

_Label `rule` added by @MichaReiser on 2024-12-31 14:15_

---

_Label `preview` added by @MichaReiser on 2024-12-31 14:15_

---

_@MichaReiser approved on 2024-12-31 14:16_

That's a lot of names... Thank you

---

_Merged by @MichaReiser on 2024-12-31 14:16_

---

_Closed by @MichaReiser on 2024-12-31 14:16_

---

_Comment by @Lee-W on 2025-01-01 23:54_

> That's a lot of names... Thank you

Thank you for your prompt reviewing 🙌 There will be a few batch to come but this one should be the biggest one haha

---

_Renamed from "[airflow]: extend names moved from core to provider (AIR303)" to "[`airflow`] Extend names moved from core to provider (`AIR303`)" by @dhruvmanila on 2025-01-02 04:45_

---

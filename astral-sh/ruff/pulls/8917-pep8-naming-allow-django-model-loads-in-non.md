```yaml
number: 8917
title: "[`pep8-naming`] Allow Django model loads in `non-lowercase-variable-in-function` (`N806`)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/django
created_at: 2023-11-30T01:29:49Z
updated_at: 2023-11-30T01:43:42Z
url: https://github.com/astral-sh/ruff/pull/8917
synced_at: 2026-01-10T23:40:55Z
```

# [`pep8-naming`] Allow Django model loads in `non-lowercase-variable-in-function` (`N806`)

---

_Pull request opened by @charliermarsh on 2023-11-30 01:29_

## Summary

Allows assignments of the form, e.g., `Attachment = apps.get_model("zerver", "Attachment")`, for better compatibility with Django.

Closes https://github.com/astral-sh/ruff/issues/7675.

## Test Plan

`cargo test`


---

_Label `rule` added by @charliermarsh on 2023-11-30 01:29_

---

_Comment by @charliermarsh on 2023-11-30 01:30_

Small change I started on the plane, might as well wrap it up.

---

_Comment by @github-actions[bot] on 2023-11-30 01:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -301 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -301 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0009_remove_messages_to_stream_stat.py#L10'>analytics/migrations/0009_remove_messages_to_stream_stat.py:10:5:</a> N806 Variable `StreamCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0009_remove_messages_to_stream_stat.py#L11'>analytics/migrations/0009_remove_messages_to_stream_stat.py:11:5:</a> N806 Variable `RealmCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0009_remove_messages_to_stream_stat.py#L12'>analytics/migrations/0009_remove_messages_to_stream_stat.py:12:5:</a> N806 Variable `InstallationCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0009_remove_messages_to_stream_stat.py#L13'>analytics/migrations/0009_remove_messages_to_stream_stat.py:13:5:</a> N806 Variable `FillState` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0009_remove_messages_to_stream_stat.py#L9'>analytics/migrations/0009_remove_messages_to_stream_stat.py:9:5:</a> N806 Variable `UserCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0010_clear_messages_sent_values.py#L10'>analytics/migrations/0010_clear_messages_sent_values.py:10:5:</a> N806 Variable `StreamCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0010_clear_messages_sent_values.py#L11'>analytics/migrations/0010_clear_messages_sent_values.py:11:5:</a> N806 Variable `RealmCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0010_clear_messages_sent_values.py#L12'>analytics/migrations/0010_clear_messages_sent_values.py:12:5:</a> N806 Variable `InstallationCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0010_clear_messages_sent_values.py#L13'>analytics/migrations/0010_clear_messages_sent_values.py:13:5:</a> N806 Variable `FillState` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0010_clear_messages_sent_values.py#L9'>analytics/migrations/0010_clear_messages_sent_values.py:9:5:</a> N806 Variable `UserCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0011_clear_analytics_tables.py#L10'>analytics/migrations/0011_clear_analytics_tables.py:10:5:</a> N806 Variable `InstallationCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0011_clear_analytics_tables.py#L11'>analytics/migrations/0011_clear_analytics_tables.py:11:5:</a> N806 Variable `FillState` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0011_clear_analytics_tables.py#L7'>analytics/migrations/0011_clear_analytics_tables.py:7:5:</a> N806 Variable `UserCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0011_clear_analytics_tables.py#L8'>analytics/migrations/0011_clear_analytics_tables.py:8:5:</a> N806 Variable `StreamCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0011_clear_analytics_tables.py#L9'>analytics/migrations/0011_clear_analytics_tables.py:9:5:</a> N806 Variable `RealmCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/confirmation/migrations/0009_confirmation_expiry_date_backfill.py#L15'>confirmation/migrations/0009_confirmation_expiry_date_backfill.py:15:5:</a> N806 Variable `Confirmation` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L595'>zerver/lib/test_helpers.py:595:9:</a> N806 Variable `ArchivedAttachment` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L596'>zerver/lib/test_helpers.py:596:9:</a> N806 Variable `ArchivedMessage` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L597'>zerver/lib/test_helpers.py:597:9:</a> N806 Variable `ArchivedUserMessage` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L598'>zerver/lib/test_helpers.py:598:9:</a> N806 Variable `Attachment` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L599'>zerver/lib/test_helpers.py:599:9:</a> N806 Variable `BotConfigData` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L600'>zerver/lib/test_helpers.py:600:9:</a> N806 Variable `BotStorageData` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L601'>zerver/lib/test_helpers.py:601:9:</a> N806 Variable `Client` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L602'>zerver/lib/test_helpers.py:602:9:</a> N806 Variable `CustomProfileField` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L603'>zerver/lib/test_helpers.py:603:9:</a> N806 Variable `CustomProfileFieldValue` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L604'>zerver/lib/test_helpers.py:604:9:</a> N806 Variable `DefaultStream` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L605'>zerver/lib/test_helpers.py:605:9:</a> N806 Variable `DefaultStreamGroup` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L606'>zerver/lib/test_helpers.py:606:9:</a> N806 Variable `EmailChangeStatus` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L607'>zerver/lib/test_helpers.py:607:9:</a> N806 Variable `Huddle` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L608'>zerver/lib/test_helpers.py:608:9:</a> N806 Variable `Message` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L609'>zerver/lib/test_helpers.py:609:9:</a> N806 Variable `MultiuseInvite` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L610'>zerver/lib/test_helpers.py:610:9:</a> N806 Variable `UserTopic` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L611'>zerver/lib/test_helpers.py:611:9:</a> N806 Variable `PreregistrationUser` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L612'>zerver/lib/test_helpers.py:612:9:</a> N806 Variable `PushDeviceToken` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L613'>zerver/lib/test_helpers.py:613:9:</a> N806 Variable `Reaction` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L614'>zerver/lib/test_helpers.py:614:9:</a> N806 Variable `Realm` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L615'>zerver/lib/test_helpers.py:615:9:</a> N806 Variable `RealmAuditLog` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L616'>zerver/lib/test_helpers.py:616:9:</a> N806 Variable `RealmDomain` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L617'>zerver/lib/test_helpers.py:617:9:</a> N806 Variable `RealmEmoji` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L618'>zerver/lib/test_helpers.py:618:9:</a> N806 Variable `RealmFilter` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L619'>zerver/lib/test_helpers.py:619:9:</a> N806 Variable `Recipient` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L623'>zerver/lib/test_helpers.py:623:9:</a> N806 Variable `ScheduledEmail` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L624'>zerver/lib/test_helpers.py:624:9:</a> N806 Variable `ScheduledMessage` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L625'>zerver/lib/test_helpers.py:625:9:</a> N806 Variable `Service` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L626'>zerver/lib/test_helpers.py:626:9:</a> N806 Variable `Stream` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L627'>zerver/lib/test_helpers.py:627:9:</a> N806 Variable `Subscription` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L628'>zerver/lib/test_helpers.py:628:9:</a> N806 Variable `UserActivity` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L629'>zerver/lib/test_helpers.py:629:9:</a> N806 Variable `UserActivityInterval` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L630'>zerver/lib/test_helpers.py:630:9:</a> N806 Variable `UserGroup` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L631'>zerver/lib/test_helpers.py:631:9:</a> N806 Variable `UserGroupMembership` in function should be lowercase
... 251 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| N806 | 301 | 0 | 301 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -301 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -301 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0009_remove_messages_to_stream_stat.py#L10'>analytics/migrations/0009_remove_messages_to_stream_stat.py:10:5:</a> N806 Variable `StreamCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0009_remove_messages_to_stream_stat.py#L11'>analytics/migrations/0009_remove_messages_to_stream_stat.py:11:5:</a> N806 Variable `RealmCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0009_remove_messages_to_stream_stat.py#L12'>analytics/migrations/0009_remove_messages_to_stream_stat.py:12:5:</a> N806 Variable `InstallationCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0009_remove_messages_to_stream_stat.py#L13'>analytics/migrations/0009_remove_messages_to_stream_stat.py:13:5:</a> N806 Variable `FillState` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0009_remove_messages_to_stream_stat.py#L9'>analytics/migrations/0009_remove_messages_to_stream_stat.py:9:5:</a> N806 Variable `UserCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0010_clear_messages_sent_values.py#L10'>analytics/migrations/0010_clear_messages_sent_values.py:10:5:</a> N806 Variable `StreamCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0010_clear_messages_sent_values.py#L11'>analytics/migrations/0010_clear_messages_sent_values.py:11:5:</a> N806 Variable `RealmCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0010_clear_messages_sent_values.py#L12'>analytics/migrations/0010_clear_messages_sent_values.py:12:5:</a> N806 Variable `InstallationCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0010_clear_messages_sent_values.py#L13'>analytics/migrations/0010_clear_messages_sent_values.py:13:5:</a> N806 Variable `FillState` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0010_clear_messages_sent_values.py#L9'>analytics/migrations/0010_clear_messages_sent_values.py:9:5:</a> N806 Variable `UserCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0011_clear_analytics_tables.py#L10'>analytics/migrations/0011_clear_analytics_tables.py:10:5:</a> N806 Variable `InstallationCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0011_clear_analytics_tables.py#L11'>analytics/migrations/0011_clear_analytics_tables.py:11:5:</a> N806 Variable `FillState` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0011_clear_analytics_tables.py#L7'>analytics/migrations/0011_clear_analytics_tables.py:7:5:</a> N806 Variable `UserCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0011_clear_analytics_tables.py#L8'>analytics/migrations/0011_clear_analytics_tables.py:8:5:</a> N806 Variable `StreamCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/analytics/migrations/0011_clear_analytics_tables.py#L9'>analytics/migrations/0011_clear_analytics_tables.py:9:5:</a> N806 Variable `RealmCount` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/confirmation/migrations/0009_confirmation_expiry_date_backfill.py#L15'>confirmation/migrations/0009_confirmation_expiry_date_backfill.py:15:5:</a> N806 Variable `Confirmation` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L595'>zerver/lib/test_helpers.py:595:9:</a> N806 Variable `ArchivedAttachment` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L596'>zerver/lib/test_helpers.py:596:9:</a> N806 Variable `ArchivedMessage` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L597'>zerver/lib/test_helpers.py:597:9:</a> N806 Variable `ArchivedUserMessage` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L598'>zerver/lib/test_helpers.py:598:9:</a> N806 Variable `Attachment` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L599'>zerver/lib/test_helpers.py:599:9:</a> N806 Variable `BotConfigData` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L600'>zerver/lib/test_helpers.py:600:9:</a> N806 Variable `BotStorageData` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L601'>zerver/lib/test_helpers.py:601:9:</a> N806 Variable `Client` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L602'>zerver/lib/test_helpers.py:602:9:</a> N806 Variable `CustomProfileField` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L603'>zerver/lib/test_helpers.py:603:9:</a> N806 Variable `CustomProfileFieldValue` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L604'>zerver/lib/test_helpers.py:604:9:</a> N806 Variable `DefaultStream` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L605'>zerver/lib/test_helpers.py:605:9:</a> N806 Variable `DefaultStreamGroup` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L606'>zerver/lib/test_helpers.py:606:9:</a> N806 Variable `EmailChangeStatus` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L607'>zerver/lib/test_helpers.py:607:9:</a> N806 Variable `Huddle` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L608'>zerver/lib/test_helpers.py:608:9:</a> N806 Variable `Message` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L609'>zerver/lib/test_helpers.py:609:9:</a> N806 Variable `MultiuseInvite` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L610'>zerver/lib/test_helpers.py:610:9:</a> N806 Variable `UserTopic` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L611'>zerver/lib/test_helpers.py:611:9:</a> N806 Variable `PreregistrationUser` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L612'>zerver/lib/test_helpers.py:612:9:</a> N806 Variable `PushDeviceToken` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L613'>zerver/lib/test_helpers.py:613:9:</a> N806 Variable `Reaction` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L614'>zerver/lib/test_helpers.py:614:9:</a> N806 Variable `Realm` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L615'>zerver/lib/test_helpers.py:615:9:</a> N806 Variable `RealmAuditLog` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L616'>zerver/lib/test_helpers.py:616:9:</a> N806 Variable `RealmDomain` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L617'>zerver/lib/test_helpers.py:617:9:</a> N806 Variable `RealmEmoji` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L618'>zerver/lib/test_helpers.py:618:9:</a> N806 Variable `RealmFilter` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L619'>zerver/lib/test_helpers.py:619:9:</a> N806 Variable `Recipient` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L623'>zerver/lib/test_helpers.py:623:9:</a> N806 Variable `ScheduledEmail` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L624'>zerver/lib/test_helpers.py:624:9:</a> N806 Variable `ScheduledMessage` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L625'>zerver/lib/test_helpers.py:625:9:</a> N806 Variable `Service` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L626'>zerver/lib/test_helpers.py:626:9:</a> N806 Variable `Stream` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L627'>zerver/lib/test_helpers.py:627:9:</a> N806 Variable `Subscription` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L628'>zerver/lib/test_helpers.py:628:9:</a> N806 Variable `UserActivity` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L629'>zerver/lib/test_helpers.py:629:9:</a> N806 Variable `UserActivityInterval` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L630'>zerver/lib/test_helpers.py:630:9:</a> N806 Variable `UserGroup` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/fd57a9033b3488d96845b0e84fe481778a401788/zerver/lib/test_helpers.py#L631'>zerver/lib/test_helpers.py:631:9:</a> N806 Variable `UserGroupMembership` in function should be lowercase
... 251 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| N806 | 301 | 0 | 301 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2023-11-30 01:43_

---

_Closed by @charliermarsh on 2023-11-30 01:43_

---

_Branch deleted on 2023-11-30 01:43_

---

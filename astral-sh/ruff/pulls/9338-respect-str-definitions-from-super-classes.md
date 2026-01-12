```yaml
number: 9338
title: "Respect `__str__` definitions from super classes"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dj
created_at: 2023-12-31T22:15:48Z
updated_at: 2023-12-31T22:32:27Z
url: https://github.com/astral-sh/ruff/pull/9338
synced_at: 2026-01-12T15:55:28Z
```

# Respect `__str__` definitions from super classes

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/9242.

---

_Label `bug` added by @charliermarsh on 2023-12-31 22:15_

---

_Merged by @charliermarsh on 2023-12-31 22:25_

---

_Closed by @charliermarsh on 2023-12-31 22:25_

---

_Branch deleted on 2023-12-31 22:25_

---

_Comment by @github-actions[bot] on 2023-12-31 22:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+18 -22 violations, +0 -0 fixes in 1 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+18 -22 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/analytics/models.py#L42'>analytics/models.py:42:16:</a> DJ001 Avoid using `null=True` on string-based fields such as CharField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/analytics/models.py#L42'>analytics/models.py:42:16:</a> DJ001 Avoid using `null=True` on string-based fields such as `CharField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/corporate/models.py#L27'>corporate/models.py:27:26:</a> DJ001 Avoid using `null=True` on string-based fields such as CharField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/corporate/models.py#L27'>corporate/models.py:27:26:</a> DJ001 Avoid using `null=True` on string-based fields such as `CharField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/corporate/models.py#L421'>corporate/models.py:421:19:</a> DJ001 Avoid using `null=True` on string-based fields such as URLField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/corporate/models.py#L421'>corporate/models.py:421:19:</a> DJ001 Avoid using `null=True` on string-based fields such as `URLField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/custom_profile_fields.py#L186'>zerver/models/custom_profile_fields.py:186:22:</a> DJ001 Avoid using `null=True` on string-based fields such as TextField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/custom_profile_fields.py#L186'>zerver/models/custom_profile_fields.py:186:22:</a> DJ001 Avoid using `null=True` on string-based fields such as `TextField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/messages.py#L114'>zerver/models/messages.py:114:7:</a> DJ008 Model does not define `__str__` method
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/messages.py#L123'>zerver/models/messages.py:123:7:</a> DJ008 Model does not define `__str__` method
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/messages.py#L57'>zerver/models/messages.py:57:24:</a> DJ001 Avoid using `null=True` on string-based fields such as TextField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/messages.py#L57'>zerver/models/messages.py:57:24:</a> DJ001 Avoid using `null=True` on string-based fields such as `TextField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/messages.py#L688'>zerver/models/messages.py:688:7:</a> DJ008 Model does not define `__str__` method
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/messages.py#L709'>zerver/models/messages.py:709:7:</a> DJ008 Model does not define `__str__` method
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/messages.py#L73'>zerver/models/messages.py:73:20:</a> DJ001 Avoid using `null=True` on string-based fields such as TextField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/messages.py#L73'>zerver/models/messages.py:73:20:</a> DJ001 Avoid using `null=True` on string-based fields such as `TextField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/prereg_users.py#L73'>zerver/models/prereg_users.py:73:17:</a> DJ001 Avoid using `null=True` on string-based fields such as CharField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/prereg_users.py#L73'>zerver/models/prereg_users.py:73:17:</a> DJ001 Avoid using `null=True` on string-based fields such as `CharField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/push_notifications.py#L32'>zerver/models/push_notifications.py:32:18:</a> DJ001 Avoid using `null=True` on string-based fields such as TextField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/push_notifications.py#L32'>zerver/models/push_notifications.py:32:18:</a> DJ001 Avoid using `null=True` on string-based fields such as `TextField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/realm_emoji.py#L48'>zerver/models/realm_emoji.py:48:17:</a> DJ001 Avoid using `null=True` on string-based fields such as TextField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/realm_emoji.py#L48'>zerver/models/realm_emoji.py:48:17:</a> DJ001 Avoid using `null=True` on string-based fields such as `TextField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/realms.py#L138'>zerver/models/realms.py:138:28:</a> DJ001 Avoid using `null=True` on string-based fields such as URLField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/realms.py#L138'>zerver/models/realms.py:138:28:</a> DJ001 Avoid using `null=True` on string-based fields such as `URLField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/realms.py#L534'>zerver/models/realms.py:534:24:</a> DJ001 Avoid using `null=True` on string-based fields such as URLField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/realms.py#L534'>zerver/models/realms.py:534:24:</a> DJ001 Avoid using `null=True` on string-based fields such as `URLField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/scheduled_jobs.py#L162'>zerver/models/scheduled_jobs.py:162:23:</a> DJ001 Avoid using `null=True` on string-based fields such as TextField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/scheduled_jobs.py#L162'>zerver/models/scheduled_jobs.py:162:23:</a> DJ001 Avoid using `null=True` on string-based fields such as `TextField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/scheduled_jobs.py#L42'>zerver/models/scheduled_jobs.py:42:15:</a> DJ001 Avoid using `null=True` on string-based fields such as EmailField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/scheduled_jobs.py#L42'>zerver/models/scheduled_jobs.py:42:15:</a> DJ001 Avoid using `null=True` on string-based fields such as `EmailField`
... 11 additional changes omitted for rule DJ001
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DJ001 | 36 | 18 | 18 | 0 | 0 |
| DJ008 | 4 | 0 | 4 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+18 -22 violations, +0 -0 fixes in 1 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+18 -22 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/analytics/models.py#L42'>analytics/models.py:42:16:</a> DJ001 Avoid using `null=True` on string-based fields such as CharField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/analytics/models.py#L42'>analytics/models.py:42:16:</a> DJ001 Avoid using `null=True` on string-based fields such as `CharField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/corporate/models.py#L27'>corporate/models.py:27:26:</a> DJ001 Avoid using `null=True` on string-based fields such as CharField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/corporate/models.py#L27'>corporate/models.py:27:26:</a> DJ001 Avoid using `null=True` on string-based fields such as `CharField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/corporate/models.py#L421'>corporate/models.py:421:19:</a> DJ001 Avoid using `null=True` on string-based fields such as URLField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/corporate/models.py#L421'>corporate/models.py:421:19:</a> DJ001 Avoid using `null=True` on string-based fields such as `URLField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/custom_profile_fields.py#L186'>zerver/models/custom_profile_fields.py:186:22:</a> DJ001 Avoid using `null=True` on string-based fields such as TextField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/custom_profile_fields.py#L186'>zerver/models/custom_profile_fields.py:186:22:</a> DJ001 Avoid using `null=True` on string-based fields such as `TextField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/messages.py#L114'>zerver/models/messages.py:114:7:</a> DJ008 Model does not define `__str__` method
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/messages.py#L123'>zerver/models/messages.py:123:7:</a> DJ008 Model does not define `__str__` method
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/messages.py#L57'>zerver/models/messages.py:57:24:</a> DJ001 Avoid using `null=True` on string-based fields such as TextField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/messages.py#L57'>zerver/models/messages.py:57:24:</a> DJ001 Avoid using `null=True` on string-based fields such as `TextField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/messages.py#L688'>zerver/models/messages.py:688:7:</a> DJ008 Model does not define `__str__` method
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/messages.py#L709'>zerver/models/messages.py:709:7:</a> DJ008 Model does not define `__str__` method
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/messages.py#L73'>zerver/models/messages.py:73:20:</a> DJ001 Avoid using `null=True` on string-based fields such as TextField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/messages.py#L73'>zerver/models/messages.py:73:20:</a> DJ001 Avoid using `null=True` on string-based fields such as `TextField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/prereg_users.py#L73'>zerver/models/prereg_users.py:73:17:</a> DJ001 Avoid using `null=True` on string-based fields such as CharField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/prereg_users.py#L73'>zerver/models/prereg_users.py:73:17:</a> DJ001 Avoid using `null=True` on string-based fields such as `CharField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/push_notifications.py#L32'>zerver/models/push_notifications.py:32:18:</a> DJ001 Avoid using `null=True` on string-based fields such as TextField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/push_notifications.py#L32'>zerver/models/push_notifications.py:32:18:</a> DJ001 Avoid using `null=True` on string-based fields such as `TextField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/realm_emoji.py#L48'>zerver/models/realm_emoji.py:48:17:</a> DJ001 Avoid using `null=True` on string-based fields such as TextField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/realm_emoji.py#L48'>zerver/models/realm_emoji.py:48:17:</a> DJ001 Avoid using `null=True` on string-based fields such as `TextField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/realms.py#L138'>zerver/models/realms.py:138:28:</a> DJ001 Avoid using `null=True` on string-based fields such as URLField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/realms.py#L138'>zerver/models/realms.py:138:28:</a> DJ001 Avoid using `null=True` on string-based fields such as `URLField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/realms.py#L534'>zerver/models/realms.py:534:24:</a> DJ001 Avoid using `null=True` on string-based fields such as URLField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/realms.py#L534'>zerver/models/realms.py:534:24:</a> DJ001 Avoid using `null=True` on string-based fields such as `URLField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/scheduled_jobs.py#L162'>zerver/models/scheduled_jobs.py:162:23:</a> DJ001 Avoid using `null=True` on string-based fields such as TextField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/scheduled_jobs.py#L162'>zerver/models/scheduled_jobs.py:162:23:</a> DJ001 Avoid using `null=True` on string-based fields such as `TextField`
- <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/scheduled_jobs.py#L42'>zerver/models/scheduled_jobs.py:42:15:</a> DJ001 Avoid using `null=True` on string-based fields such as EmailField
+ <a href='https://github.com/zulip/zulip/blob/9f009a2e638220ad4aaa791407e28d4f8dad9922/zerver/models/scheduled_jobs.py#L42'>zerver/models/scheduled_jobs.py:42:15:</a> DJ001 Avoid using `null=True` on string-based fields such as `EmailField`
... 11 additional changes omitted for rule DJ001
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DJ001 | 36 | 18 | 18 | 0 | 0 |
| DJ008 | 4 | 0 | 4 | 0 | 0 |

</p>
</details>




---

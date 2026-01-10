```yaml
number: 10920
title: "[`pylint`] Support inverted comparisons (`PLR1730`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - rule
assignees: []
merged: true
base: main
head: improve-PLR1730
created_at: 2024-04-13T19:37:10Z
updated_at: 2024-04-13T23:03:00Z
url: https://github.com/astral-sh/ruff/pull/10920
synced_at: 2026-01-10T22:37:01Z
```

# [`pylint`] Support inverted comparisons (`PLR1730`)

---

_Pull request opened by @diceroll123 on 2024-04-13 19:37_

## Summary

Adds more aggressive logic to PLR1730, `if-stmt-min-max`

Closes #10907 

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2024-04-13 19:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+7 -0 violations, +0 -0 fixes in 4 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/f7bf36fa3de91ee4369d4ef302c8b1c5b361bfbb/samcli/lib/observability/cw_logs/cw_log_puller.py#L136'>samcli/lib/observability/cw_logs/cw_log_puller.py:136:17:</a> PLR1730 [*] Replace `if` statement with `max` call
+ <a href='https://github.com/aws/aws-sam-cli/blob/f7bf36fa3de91ee4369d4ef302c8b1c5b361bfbb/samcli/lib/observability/xray_traces/xray_event_puller.py#L163'>samcli/lib/observability/xray_traces/xray_event_puller.py:163:21:</a> PLR1730 [*] Replace `if` statement with `max` call
+ <a href='https://github.com/aws/aws-sam-cli/blob/f7bf36fa3de91ee4369d4ef302c8b1c5b361bfbb/samcli/lib/observability/xray_traces/xray_events.py#L52'>samcli/lib/observability/xray_traces/xray_events.py:52:13:</a> PLR1730 [*] Replace `if` statement with `max` call
+ <a href='https://github.com/aws/aws-sam-cli/blob/f7bf36fa3de91ee4369d4ef302c8b1c5b361bfbb/samcli/lib/observability/xray_traces/xray_events.py#L87'>samcli/lib/observability/xray_traces/xray_events.py:87:13:</a> PLR1730 [*] Replace `if` statement with `max` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/ef84bf8f877212e3eb999ee516c4bcf3300c4d1a/rotkehlchen/chain/evm/decoding/utils.py#L42'>rotkehlchen/chain/evm/decoding/utils.py:42:13:</a> PLR1730 [*] Replace `if` statement with `max` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/e7a19aca757820fe53f896ab15ad4128bf94838a/zerver/tests/test_openapi.py#L344'>zerver/tests/test_openapi.py:344:13:</a> PLR1730 [*] Replace `if` statement with `val = min(v, val)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/ad21185dc24b52df3e64623250b2472c487f537a/indico/modules/receipts/controllers/templates.py#L218'>indico/modules/receipts/controllers/templates.py:218:17:</a> PLR1730 [*] Replace `if` statement with `max_index = max(index, max_index)`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1730 | 7 | 7 | 0 | 0 | 0 |

</p>
</details>




---

_@charliermarsh approved on 2024-04-13 21:54_

Thanks!

---

_@charliermarsh approved on 2024-04-13 22:50_

Thanks!

---

_Renamed from "[`pylint`] - add more aggressive logic (`PLR1730`)" to "[`pylint`] Support inverted comparisons (`PLR1730`)" by @charliermarsh on 2024-04-13 22:50_

---

_Label `rule` added by @charliermarsh on 2024-04-13 22:50_

---

_Merged by @charliermarsh on 2024-04-13 22:57_

---

_Closed by @charliermarsh on 2024-04-13 22:57_

---

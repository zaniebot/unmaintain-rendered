```yaml
number: 10583
title: "[`refurb`] Implement `for-loop-set-mutations` (`FURB142`)"
type: pull_request
state: merged
author: alex-700
labels:
  - rule
assignees: []
merged: true
base: main
head: latyshev/furb142
created_at: 2024-03-25T18:17:04Z
updated_at: 2025-06-10T20:46:59Z
url: https://github.com/astral-sh/ruff/pull/10583
synced_at: 2026-01-12T15:55:32Z
```

# [`refurb`] Implement `for-loop-set-mutations` (`FURB142`)

---

_@alex-700_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement `manual_set_update` (`FURB142`) lint. 
see:
- https://github.com/astral-sh/ruff/issues/1348
- [original FURB142](https://github.com/dosisod/refurb/blob/master/refurb/checks/builtin/no_set_for_loop.py)

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
cargo test

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-03-25 18:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+34 -0 violations, +0 -0 fixes in 5 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/hooks/glue_catalog.py#L124'>airflow/providers/amazon/aws/hooks/glue_catalog.py:124:13:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/providers/amazon/aws/hooks/glue_catalog.py#L82'>airflow/providers/amazon/aws/hooks/glue_catalog.py:82:13:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/airflow/utils/dag_edges.py#L74'>airflow/utils/dag_edges.py:74:17:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2052'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2052:13:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/utils/docker_command_utils.py#L522'>dev/breeze/src/airflow_breeze/utils/docker_command_utils.py:522:5:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/utils/provider_dependencies.py#L54'>dev/breeze/src/airflow_breeze/utils/provider_dependencies.py:54:9:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L827'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:827:17:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/stats/calculate_statistics_provider_testing_issues.py#L110'>dev/stats/calculate_statistics_provider_testing_issues.py:110:5:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/dev/stats/calculate_statistics_provider_testing_issues.py#L117'>dev/stats/calculate_statistics_provider_testing_issues.py:117:5:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/scripts/tools/list-integrations.py#L67'>scripts/tools/list-integrations.py:67:9:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/tests/jobs/test_scheduler_job.py#L3143'>tests/jobs/test_scheduler_job.py:3143:9:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/airflow/blob/714a933479f9dc1c3ef5916e43292efc182a0857/tests/jobs/test_scheduler_job.py#L3157'>tests/jobs/test_scheduler_job.py:3157:9:</a> FURB142 [*] Use of `set.add()` in a for loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/document/test_models.py#L113'>tests/unit/bokeh/document/test_models.py:113:9:</a> FURB142 [*] Use of `set.add()` in a for loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/e483befbaad2133d573d9f818e05601a805266bb/rotkehlchen/accounting/structures/defi.py#L94'>rotkehlchen/accounting/structures/defi.py:94:13:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/rotki/rotki/blob/e483befbaad2133d573d9f818e05601a805266bb/rotkehlchen/chain/bitcoin/bch/utils.py#L166'>rotkehlchen/chain/bitcoin/bch/utils.py:166:5:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/rotki/rotki/blob/e483befbaad2133d573d9f818e05601a805266bb/rotkehlchen/chain/evm/decoding/velodrome/velodrome_cache.py#L118'>rotkehlchen/chain/evm/decoding/velodrome/velodrome_cache.py:118:9:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/rotki/rotki/blob/e483befbaad2133d573d9f818e05601a805266bb/rotkehlchen/chain/evm/decoding/velodrome/velodrome_cache.py#L122'>rotkehlchen/chain/evm/decoding/velodrome/velodrome_cache.py:122:9:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/rotki/rotki/blob/e483befbaad2133d573d9f818e05601a805266bb/rotkehlchen/tests/exchanges/test_okx.py#L33'>rotkehlchen/tests/exchanges/test_okx.py:33:5:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/rotki/rotki/blob/e483befbaad2133d573d9f818e05601a805266bb/rotkehlchen/tests/unit/test_data_dir.py#L77'>rotkehlchen/tests/unit/test_data_dir.py:77:5:</a> FURB142 [*] Use of `set.add()` in a for loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+14 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/38b2988e0fdefbf466c846870606c1a41449e941/corporate/views/support.py#L501'>corporate/views/support.py:501:9:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/38b2988e0fdefbf466c846870606c1a41449e941/corporate/views/support.py#L506'>corporate/views/support.py:506:9:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/38b2988e0fdefbf466c846870606c1a41449e941/corporate/views/support.py#L515'>corporate/views/support.py:515:9:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/38b2988e0fdefbf466c846870606c1a41449e941/corporate/views/support.py#L526'>corporate/views/support.py:526:9:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/38b2988e0fdefbf466c846870606c1a41449e941/zerver/data_import/mattermost.py#L221'>zerver/data_import/mattermost.py:221:9:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/38b2988e0fdefbf466c846870606c1a41449e941/zerver/data_import/mattermost.py#L224'>zerver/data_import/mattermost.py:224:9:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/38b2988e0fdefbf466c846870606c1a41449e941/zerver/data_import/mattermost.py#L260'>zerver/data_import/mattermost.py:260:13:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/38b2988e0fdefbf466c846870606c1a41449e941/zerver/data_import/rocketchat.py#L256'>zerver/data_import/rocketchat.py:256:9:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/38b2988e0fdefbf466c846870606c1a41449e941/zerver/lib/bulk_create.py#L250'>zerver/lib/bulk_create.py:250:5:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/38b2988e0fdefbf466c846870606c1a41449e941/zerver/lib/display_recipient.py#L157'>zerver/lib/display_recipient.py:157:5:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/38b2988e0fdefbf466c846870606c1a41449e941/zerver/lib/server_initialization.py#L84'>zerver/lib/server_initialization.py:84:5:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/38b2988e0fdefbf466c846870606c1a41449e941/zerver/lib/test_fixtures.py#L357'>zerver/lib/test_fixtures.py:357:17:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/38b2988e0fdefbf466c846870606c1a41449e941/zerver/tests/test_import_export.py#L1126'>zerver/tests/test_import_export.py:1126:21:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/38b2988e0fdefbf466c846870606c1a41449e941/zerver/views/message_send.py#L43'>zerver/views/message_send.py:43:9:</a> FURB142 [*] Use of `set.add()` in a for loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/bf5e80e457ccad1c037146fcf60d50edf5c6b786/indico/modules/events/abstracts/controllers/reviewing.py#L199'>indico/modules/events/abstracts/controllers/reviewing.py:199:13:</a> FURB142 [*] Use of `set.add()` in a for loop
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB142 | 34 | 34 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2024-03-26 07:49_

---

_@MichaReiser approved on 2024-03-26 07:58_

Thanks. This looks good to me. But I want to leave it to @AlexWaygood to also take a look.


My only concern with the rule as is is its name. From reading the rule name (or even the error message), it gives me the impression that I'm not supposed to update a set "manually" (unclear what that means) at all. Maybe `for-set-mutations` or `for-iterable-set-mutations` 



My reasoning. This is a style rule that guides users towards writing more idiomatic Python. 

---

_Comment by @AlexWaygood on 2024-03-26 10:59_

> My only concern with the rule as is is its name. From reading the rule name (or even the error message), it gives me the impression that I'm not supposed to update a set "manually" (unclear what that means) at all. Maybe `for-set-mutations` or `for-iterable-set-mutations`

I agree with @MichaReiser. The original name that Refurb has seems quite good to me ([`no-set-for-loop`](https://github.com/dosisod/refurb/blob/master/docs/checks.md#furb142-no-set-for-loop)), but obviously doesn't fit with our naming conventions. Maybe `for-loop-set-mutations`? In the error message, rather than saying "use of manual set update", I'd probably just say "use of `set.add` in a for loop".

---

_@AlexWaygood reviewed on 2024-03-26 11:03_

Overall really nicely done -- seems like a useful, uncontroversial rule and the autofixes are great. Just need to work on the docs a little bit :-)

---

_Renamed from "[`refurb`] Implement `manual_set_update` (`FURB142`)" to "[`refurb`] Implement `for-loop-set-mutations` (`FURB142`)" by @AlexWaygood on 2024-03-26 11:29_

---

_Comment by @alex-700 on 2024-03-26 11:32_

@MichaReiser @AlexWaygood thanks for the review!

> My only concern with the rule as is is its name. From reading the rule name (or even the error message), it gives me the impression that I'm not supposed to update a set "manually" (unclear what that means) at all. Maybe for-set-mutations or for-iterable-set-mutations

I'm fully aggree with the naming concern here, but could not invent something better. 
Stick to @AlexWaygood 's option `for-loop-set-mutations`, which I really like. Thanks!

---

_@AlexWaygood approved on 2024-03-26 11:38_

LGTM, thanks!

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-03-26 11:38_

---

_@MichaReiser reviewed on 2024-03-26 11:50_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/refurb/FURB142.py`:21 on 2024-03-26 11:50_

Would you mind adding a test case where the value passed to add also depends on other variables other than the loop variable, like https://github.com/apache/airflow/blob/e41b3c58e2d2f5968bd97b8d3b05a16e926be980/airflow/utils/dag_edges.py#L74

---

_@alex-700 reviewed on 2024-03-26 12:38_

---

_Review comment by @alex-700 on `crates/ruff_linter/resources/test/fixtures/refurb/FURB142.py`:21 on 2024-03-26 12:38_

Added.
See [commit](https://github.com/astral-sh/ruff/pull/10583/commits/02fa99ebbec92bca7d20be09d95da270baf0e08b)

---

_@MichaReiser reviewed on 2024-03-27 08:25_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/refurb/FURB142.py`:21 on 2024-03-27 08:25_

Thanks

---

_Merged by @MichaReiser on 2024-03-27 08:26_

---

_Closed by @MichaReiser on 2024-03-27 08:26_

---

_Branch deleted on 2025-06-10 20:46_

---

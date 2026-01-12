```yaml
number: 12043
title: "[Ruff 0.5] Stabilise 11 `FURB` rules"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: ruff-0.5
head: refurb-stabilisations
created_at: 2024-06-26T10:52:56Z
updated_at: 2024-06-26T12:24:52Z
url: https://github.com/astral-sh/ruff/pull/12043
synced_at: 2026-01-12T15:55:40Z
```

# [Ruff 0.5] Stabilise 11 `FURB` rules

---

_@AlexWaygood_

## Summary

Stabilise the following rules for release 0.5:
- `print-empty-string` (`FURB105`)
- `readlines-in-for` (`FURB129`)
- `if-expr-min-max` (`FURB136`)
- ~~`math-constant` (`FURB152`)~~
- `bit-count` (`FURB161`)
- `redundant-log-base` (`FURB163`)
- `regex-flag-alias` (`FURB167`)
- `isinstance-type-none` (`FURB168`)
- `type-none-comparison` (`FURB169`)
- `implicit-cwd` (`FURB177`)
- `hashlib-digest-hex` (`FURB181`)
- `list-reverse-copy` (`FURB187`)

These rules have all been in `preview` for over 90 days and several minor releases. There are no open issues about any of them, and there have not been any open issues about any of them for many months. For each rule, the documentation gives an accurate description of what the rule does and the implementation seems sound.

These are all stylistic rules, but I think their recommendations are all fairly uncontroverisal. Several other Refurb rules have been kept in preview for this release, as there are either outstanding issues with the implementation or their recommendations are (in my opinion) highly opinionated/controversial.

## Test Plan

`cargo test -p ruff_linter`


---

_Label `rule` added by @AlexWaygood on 2024-06-26 10:52_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-26 10:52_

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-06-26 10:52_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-06-26 10:52_

---

_Comment by @github-actions[bot] on 2024-06-26 11:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+42 -0 violations, +0 -0 fixes in 3 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+15 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/jobs/backfill_job_runner.py#L923'>airflow/jobs/backfill_job_runner.py:923:13:</a> FURB187 [*] Use of assignment of `reversed` on list `dagrun_infos`
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/providers/apache/kafka/operators/consume.py#L158'>airflow/providers/apache/kafka/operators/consume.py:158:30:</a> FURB136 [*] Replace `if` expression with `min(messages_left, self.max_batch_size)`
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/providers/apache/spark/hooks/spark_submit.py#L306'>airflow/providers/apache/spark/hooks/spark_submit.py:306:19:</a> FURB167 [*] Use of regular expression alias `re.I`
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py#L149'>airflow/providers/cncf/kubernetes/executors/kubernetes_executor_utils.py:149:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/providers/exasol/hooks/exasol.py#L68'>airflow/providers/exasol/hooks/exasol.py:68:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/providers/mysql/hooks/mysql.py#L171'>airflow/providers/mysql/hooks/mysql.py:171:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/airflow/providers/postgres/hooks/postgres.py#L173'>airflow/providers/postgres/hooks/postgres.py:173:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2486'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2486:13:</a> PERF403 Use a dictionary comprehension instead of a for-loop
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/dev/breeze/src/airflow_breeze/global_constants.py#L401'>dev/breeze/src/airflow_breeze/global_constants.py:401:21:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/dev/perf/sql_queries.py#L143'>dev/perf/sql_queries.py:143:41:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/docs/exts/redirects.py#L44'>docs/exts/redirects.py:44:21:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/scripts/ci/pre_commit/newsfragments.py#L36'>scripts/ci/pre_commit/newsfragments.py:36:43:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/scripts/ci/testing/summarize_captured_warnings.py#L246'>scripts/ci/testing/summarize_captured_warnings.py:246:11:</a> FURB177 Prefer `Path.cwd()` over `Path().resolve()` for current-directory lookups
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/tests/providers/apache/spark/hooks/test_spark_submit.py#L74'>tests/providers/apache/spark/hooks/test_spark_submit.py:74:17:</a> PERF403 Use a dictionary comprehension instead of a for-loop
... 1 additional changes omitted for rule PERF403
+ <a href='https://github.com/apache/airflow/blob/dcaf82a155337e36d133ff673bafc5cf50303034/tests/system/providers/google/cloud/gcs/resources/transform_script.py#L26'>tests/system/providers/google/cloud/gcs/resources/transform_script.py:26:39:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/visual.py#L109'>src/bokeh/core/property/visual.py:109:116:</a> FURB167 [*] Use of regular expression alias `re.I`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/util.py#L366'>src/bokeh/embed/util.py:366:60:</a> FURB167 [*] Use of regular expression alias `re.S`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/util.py#L381'>src/bokeh/embed/util.py:381:60:</a> FURB167 [*] Use of regular expression alias `re.S`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+24 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/tools/lib/provision_inner.py#L101'>tools/lib/provision_inner.py:101:51:</a> FURB129 Instead of calling `readlines()`, iterate over file object directly
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/lib/compatibility.py#L46'>zerver/lib/compatibility.py:46:60:</a> FURB167 [*] Use of regular expression alias `re.X`
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/lib/compatibility.py#L97'>zerver/lib/compatibility.py:97:48:</a> FURB167 [*] Use of regular expression alias `re.I`
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/lib/compatibility.py#L97'>zerver/lib/compatibility.py:97:55:</a> FURB167 [*] Use of regular expression alias `re.X`
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/lib/compatibility.py#L99'>zerver/lib/compatibility.py:99:61:</a> FURB167 [*] Use of regular expression alias `re.I`
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/lib/compatibility.py#L99'>zerver/lib/compatibility.py:99:68:</a> FURB167 [*] Use of regular expression alias `re.X`
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/lib/markdown/include.py#L30'>zerver/lib/markdown/include.py:30:48:</a> FURB167 [*] Use of regular expression alias `re.M`
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/lib/user_agent.py#L12'>zerver/lib/user_agent.py:12:5:</a> FURB167 [*] Use of regular expression alias `re.X`
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/management/commands/create_default_stream_groups.py#L67'>zerver/management/commands/create_default_stream_groups.py:67:13:</a> FURB105 [*] Unnecessary empty string passed to `print`
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/management/commands/deactivate_user.py#L38'>zerver/management/commands/deactivate_user.py:38:9:</a> FURB105 [*] Unnecessary empty string passed to `print`
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/management/commands/delete_old_unclaimed_attachments.py#L64'>zerver/management/commands/delete_old_unclaimed_attachments.py:64:13:</a> FURB105 [*] Unnecessary empty string passed to `print`
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/management/commands/delete_old_unclaimed_attachments.py#L68'>zerver/management/commands/delete_old_unclaimed_attachments.py:68:13:</a> FURB105 [*] Unnecessary empty string passed to `print`
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/management/commands/delete_old_unclaimed_attachments.py#L72'>zerver/management/commands/delete_old_unclaimed_attachments.py:72:13:</a> FURB105 [*] Unnecessary empty string passed to `print`
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/management/commands/register_server.py#L103'>zerver/management/commands/register_server.py:103:13:</a> FURB105 [*] Unnecessary empty string passed to `print`
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/migrations/0041_create_attachments_for_old_messages.py#L14'>zerver/migrations/0041_create_attachments_for_old_messages.py:14:52:</a> FURB167 [*] Use of regular expression alias `re.M`
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/migrations/0371_invalid_characters_in_topics.py#L35'>zerver/migrations/0371_invalid_characters_in_topics.py:35:5:</a> FURB105 [*] Unnecessary empty string passed to `print`
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/migrations/0375_invalid_characters_in_stream_names.py#L33'>zerver/migrations/0375_invalid_characters_in_stream_names.py:33:5:</a> FURB105 [*] Unnecessary empty string passed to `print`
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/migrations/0383_revoke_invitations_from_deactivated_users.py#L47'>zerver/migrations/0383_revoke_invitations_from_deactivated_users.py:47:5:</a> FURB105 [*] Unnecessary empty string passed to `print`
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/migrations/0423_fix_email_gateway_attachment_owner.py#L40'>zerver/migrations/0423_fix_email_gateway_attachment_owner.py:40:5:</a> FURB105 [*] Unnecessary empty string passed to `print`
... 3 additional changes omitted for rule FURB105
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/openapi/markdown_extension.py#L173'>zerver/openapi/markdown_extension.py:173:42:</a> FURB167 [*] Use of regular expression alias `re.M`
+ <a href='https://github.com/zulip/zulip/blob/dcf2c67f2d084339148cc8397ff37c47da2a0d8e/zerver/openapi/markdown_extension.py#L173'>zerver/openapi/markdown_extension.py:173:49:</a> FURB167 [*] Use of regular expression alias `re.S`
... 2 additional changes omitted for rule FURB167
</pre>

</p>
</details>
<details><summary>Changes by rule (7 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB167 | 15 | 15 | 0 | 0 | 0 |
| FURB105 | 12 | 12 | 0 | 0 | 0 |
| PERF403 | 6 | 6 | 0 | 0 | 0 |
| FURB129 | 6 | 6 | 0 | 0 | 0 |
| FURB187 | 1 | 1 | 0 | 0 | 0 |
| FURB136 | 1 | 1 | 0 | 0 | 0 |
| FURB177 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-06-26 11:35_

Having seen the ecosystem report, I'll remove `math-constant` from the proposed stabilisations. It seems it has quite a few false positives.

---

_Renamed from "[Ruff 0.5] Stabilise 12 `FURB` rules" to "[Ruff 0.5] Stabilise 11 `FURB` rules" by @AlexWaygood on 2024-06-26 11:38_

---

_Comment by @AlexWaygood on 2024-06-26 11:56_

> Having seen the ecosystem report, I'll remove `math-constant` from the proposed stabilisations. It seems it has quite a few false positives.

Now that I've made that change, the updated ecosystem hits all LGTM. I think in each instance, following the recommendation would be a reasonably uncontroversial style/readability improvement.

---

_@charliermarsh approved on 2024-06-26 12:01_

These rules are quite complex so I anticipate some issues. But we're clearly not getting more issues by keeping them in preview at this point, so seems fine.

---

_Merged by @AlexWaygood on 2024-06-26 12:24_

---

_Closed by @AlexWaygood on 2024-06-26 12:24_

---

_Branch deleted on 2024-06-26 12:24_

---

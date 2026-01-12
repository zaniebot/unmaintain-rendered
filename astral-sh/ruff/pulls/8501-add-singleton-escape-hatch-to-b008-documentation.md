```yaml
number: 8501
title: Add singleton escape hatch to B008 documentation
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/doc
created_at: 2023-11-06T01:00:00Z
updated_at: 2023-11-07T05:00:22Z
url: https://github.com/astral-sh/ruff/pull/8501
synced_at: 2026-01-10T23:40:55Z
```

# Add singleton escape hatch to B008 documentation

---

_Pull request opened by @charliermarsh on 2023-11-06 01:00_

## Summary:

Closes: https://github.com/astral-sh/ruff/issues/8378.


---

_Review requested from @zanieb by @charliermarsh on 2023-11-06 01:00_

---

_Label `documentation` added by @charliermarsh on 2023-11-06 01:00_

---

_@charliermarsh reviewed on 2023-11-06 01:00_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/function_call_in_argument_default.rs`:77 on 2023-11-06 01:00_

I'm not a huge fan of bugbear's error messages which tend to be very long (https://github.com/PyCQA/flake8-bugbear/blob/907e0dd29a99818591a604d4557c70ea33204712/bugbear.py#L1660-L1667). I prefer to put that level of detail in our documentation, or make it evident via the proposed fix. But I do think this one can be a bit clearer. Wdyt @zanieb?

---

_Comment by @github-actions[bot] on 2023-11-06 01:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+93 -93 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+16 -16 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/ca27772f0aea1c98aa2367d4450bf477ca851d23/airflow/metrics/datadog_logger.py#L49'>airflow/metrics/datadog_logger.py:49:44:</a> B008 Do not perform function call `AllowListValidator` in argument defaults
+ <a href='https://github.com/apache/airflow/blob/ca27772f0aea1c98aa2367d4450bf477ca851d23/airflow/metrics/datadog_logger.py#L49'>airflow/metrics/datadog_logger.py:49:44:</a> B008 Do not perform function call `AllowListValidator` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/apache/airflow/blob/ca27772f0aea1c98aa2367d4450bf477ca851d23/airflow/metrics/datadog_logger.py#L51'>airflow/metrics/datadog_logger.py:51:48:</a> B008 Do not perform function call `AllowListValidator` in argument defaults
+ <a href='https://github.com/apache/airflow/blob/ca27772f0aea1c98aa2367d4450bf477ca851d23/airflow/metrics/datadog_logger.py#L51'>airflow/metrics/datadog_logger.py:51:48:</a> B008 Do not perform function call `AllowListValidator` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/apache/airflow/blob/ca27772f0aea1c98aa2367d4450bf477ca851d23/airflow/metrics/otel_logger.py#L169'>airflow/metrics/otel_logger.py:169:30:</a> B008 Do not perform function call `AllowListValidator` in argument defaults
+ <a href='https://github.com/apache/airflow/blob/ca27772f0aea1c98aa2367d4450bf477ca851d23/airflow/metrics/otel_logger.py#L169'>airflow/metrics/otel_logger.py:169:30:</a> B008 Do not perform function call `AllowListValidator` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/apache/airflow/blob/ca27772f0aea1c98aa2367d4450bf477ca851d23/airflow/metrics/statsd_logger.py#L72'>airflow/metrics/statsd_logger.py:72:44:</a> B008 Do not perform function call `AllowListValidator` in argument defaults
+ <a href='https://github.com/apache/airflow/blob/ca27772f0aea1c98aa2367d4450bf477ca851d23/airflow/metrics/statsd_logger.py#L72'>airflow/metrics/statsd_logger.py:72:44:</a> B008 Do not perform function call `AllowListValidator` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/apache/airflow/blob/ca27772f0aea1c98aa2367d4450bf477ca851d23/airflow/metrics/statsd_logger.py#L74'>airflow/metrics/statsd_logger.py:74:48:</a> B008 Do not perform function call `AllowListValidator` in argument defaults
+ <a href='https://github.com/apache/airflow/blob/ca27772f0aea1c98aa2367d4450bf477ca851d23/airflow/metrics/statsd_logger.py#L74'>airflow/metrics/statsd_logger.py:74:48:</a> B008 Do not perform function call `AllowListValidator` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
... 22 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/src/bokeh/server/tornado.py#L274'>src/bokeh/server/tornado.py:274:48:</a> B008 Do not perform function call `NullAuth` in argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/src/bokeh/server/tornado.py#L274'>src/bokeh/server/tornado.py:274:48:</a> B008 Do not perform function call `NullAuth` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/3d891d038fb165c5987f1a0d24c4a0a9c62992b9/latch/verified/pathway.py#L17'>latch/verified/pathway.py:17:33:</a> B008 Do not perform function call `LatchDir` in argument defaults
+ <a href='https://github.com/latchbio/latch/blob/3d891d038fb165c5987f1a0d24c4a0a9c62992b9/latch/verified/pathway.py#L17'>latch/verified/pathway.py:17:33:</a> B008 Do not perform function call `LatchDir` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+75 -75 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/analytics/views/stats.py#L254'>analytics/views/stats.py:254:33:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/analytics/views/stats.py#L254'>analytics/views/stats.py:254:33:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/analytics/views/stats.py#L255'>analytics/views/stats.py:255:31:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/analytics/views/stats.py#L255'>analytics/views/stats.py:255:31:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/analytics/views/support.py#L165'>analytics/views/support.py:165:35:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/analytics/views/support.py#L165'>analytics/views/support.py:165:35:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/confirmation/models.py#L118'>confirmation/models.py:118:67:</a> B008 Do not perform function call `UnspecifiedValue` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/confirmation/models.py#L118'>confirmation/models.py:118:67:</a> B008 Do not perform function call `UnspecifiedValue` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/actions/invites.py#L54'>zerver/actions/invites.py:54:73:</a> B008 Do not perform function call `UnspecifiedValue` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/actions/invites.py#L54'>zerver/actions/invites.py:54:73:</a> B008 Do not perform function call `UnspecifiedValue` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/models.py#L2526'>zerver/models.py:2526:73:</a> B008 Do not perform function call `UnspecifiedValue` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/models.py#L2526'>zerver/models.py:2526:73:</a> B008 Do not perform function call `UnspecifiedValue` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/tests/test_has_request_variables.py#L138'>zerver/tests/test_has_request_variables.py:138:61:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/tests/test_has_request_variables.py#L138'>zerver/tests/test_has_request_variables.py:138:61:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/tests/test_typed_endpoint.py#L87'>zerver/tests/test_typed_endpoint.py:87:39:</a> B008 Do not perform function call `Foo` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/tests/test_typed_endpoint.py#L87'>zerver/tests/test_typed_endpoint.py:87:39:</a> B008 Do not perform function call `Foo` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/tornado/views.py#L114'>zerver/tornado/views.py:114:37:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/tornado/views.py#L114'>zerver/tornado/views.py:114:37:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L154'>zerver/views/custom_profile_fields.py:154:36:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L154'>zerver/views/custom_profile_fields.py:154:36:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L209'>zerver/views/custom_profile_fields.py:209:36:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L209'>zerver/views/custom_profile_fields.py:209:36:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L259'>zerver/views/custom_profile_fields.py:259:24:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L259'>zerver/views/custom_profile_fields.py:259:24:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L259'>zerver/views/custom_profile_fields.py:259:43:</a> B008 Do not perform function call `check_list` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L259'>zerver/views/custom_profile_fields.py:259:43:</a> B008 Do not perform function call `check_list` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L270'>zerver/views/custom_profile_fields.py:270:23:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L270'>zerver/views/custom_profile_fields.py:270:23:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L270'>zerver/views/custom_profile_fields.py:270:42:</a> B008 Do not perform function call `check_list` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L270'>zerver/views/custom_profile_fields.py:270:42:</a> B008 Do not perform function call `check_list` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L293'>zerver/views/custom_profile_fields.py:293:48:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L293'>zerver/views/custom_profile_fields.py:293:48:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L294'>zerver/views/custom_profile_fields.py:294:24:</a> B008 Do not perform function call `check_list` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L294'>zerver/views/custom_profile_fields.py:294:24:</a> B008 Do not perform function call `check_list` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/events_register.py#L52'>zerver/views/events_register.py:52:54:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/events_register.py#L52'>zerver/views/events_register.py:52:54:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/events_register.py#L53'>zerver/views/events_register.py:53:24:</a> B008 Do not perform function call `check_dict` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/events_register.py#L53'>zerver/views/events_register.py:53:24:</a> B008 Do not perform function call `check_dict` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/events_register.py#L78'>zerver/views/events_register.py:78:23:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/events_register.py#L78'>zerver/views/events_register.py:78:23:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
... 110 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B008 | 186 | 93 | 93 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+93 -93 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+16 -16 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/ca27772f0aea1c98aa2367d4450bf477ca851d23/airflow/metrics/datadog_logger.py#L49'>airflow/metrics/datadog_logger.py:49:44:</a> B008 Do not perform function call `AllowListValidator` in argument defaults
+ <a href='https://github.com/apache/airflow/blob/ca27772f0aea1c98aa2367d4450bf477ca851d23/airflow/metrics/datadog_logger.py#L49'>airflow/metrics/datadog_logger.py:49:44:</a> B008 Do not perform function call `AllowListValidator` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/apache/airflow/blob/ca27772f0aea1c98aa2367d4450bf477ca851d23/airflow/metrics/datadog_logger.py#L51'>airflow/metrics/datadog_logger.py:51:48:</a> B008 Do not perform function call `AllowListValidator` in argument defaults
+ <a href='https://github.com/apache/airflow/blob/ca27772f0aea1c98aa2367d4450bf477ca851d23/airflow/metrics/datadog_logger.py#L51'>airflow/metrics/datadog_logger.py:51:48:</a> B008 Do not perform function call `AllowListValidator` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/apache/airflow/blob/ca27772f0aea1c98aa2367d4450bf477ca851d23/airflow/metrics/otel_logger.py#L169'>airflow/metrics/otel_logger.py:169:30:</a> B008 Do not perform function call `AllowListValidator` in argument defaults
+ <a href='https://github.com/apache/airflow/blob/ca27772f0aea1c98aa2367d4450bf477ca851d23/airflow/metrics/otel_logger.py#L169'>airflow/metrics/otel_logger.py:169:30:</a> B008 Do not perform function call `AllowListValidator` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/apache/airflow/blob/ca27772f0aea1c98aa2367d4450bf477ca851d23/airflow/metrics/statsd_logger.py#L72'>airflow/metrics/statsd_logger.py:72:44:</a> B008 Do not perform function call `AllowListValidator` in argument defaults
+ <a href='https://github.com/apache/airflow/blob/ca27772f0aea1c98aa2367d4450bf477ca851d23/airflow/metrics/statsd_logger.py#L72'>airflow/metrics/statsd_logger.py:72:44:</a> B008 Do not perform function call `AllowListValidator` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/apache/airflow/blob/ca27772f0aea1c98aa2367d4450bf477ca851d23/airflow/metrics/statsd_logger.py#L74'>airflow/metrics/statsd_logger.py:74:48:</a> B008 Do not perform function call `AllowListValidator` in argument defaults
+ <a href='https://github.com/apache/airflow/blob/ca27772f0aea1c98aa2367d4450bf477ca851d23/airflow/metrics/statsd_logger.py#L74'>airflow/metrics/statsd_logger.py:74:48:</a> B008 Do not perform function call `AllowListValidator` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
... 22 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/src/bokeh/server/tornado.py#L274'>src/bokeh/server/tornado.py:274:48:</a> B008 Do not perform function call `NullAuth` in argument defaults
+ <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/src/bokeh/server/tornado.py#L274'>src/bokeh/server/tornado.py:274:48:</a> B008 Do not perform function call `NullAuth` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/3d891d038fb165c5987f1a0d24c4a0a9c62992b9/latch/verified/pathway.py#L17'>latch/verified/pathway.py:17:33:</a> B008 Do not perform function call `LatchDir` in argument defaults
+ <a href='https://github.com/latchbio/latch/blob/3d891d038fb165c5987f1a0d24c4a0a9c62992b9/latch/verified/pathway.py#L17'>latch/verified/pathway.py:17:33:</a> B008 Do not perform function call `LatchDir` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+75 -75 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/analytics/views/stats.py#L254'>analytics/views/stats.py:254:33:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/analytics/views/stats.py#L254'>analytics/views/stats.py:254:33:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/analytics/views/stats.py#L255'>analytics/views/stats.py:255:31:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/analytics/views/stats.py#L255'>analytics/views/stats.py:255:31:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/analytics/views/support.py#L165'>analytics/views/support.py:165:35:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/analytics/views/support.py#L165'>analytics/views/support.py:165:35:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/confirmation/models.py#L118'>confirmation/models.py:118:67:</a> B008 Do not perform function call `UnspecifiedValue` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/confirmation/models.py#L118'>confirmation/models.py:118:67:</a> B008 Do not perform function call `UnspecifiedValue` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/actions/invites.py#L54'>zerver/actions/invites.py:54:73:</a> B008 Do not perform function call `UnspecifiedValue` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/actions/invites.py#L54'>zerver/actions/invites.py:54:73:</a> B008 Do not perform function call `UnspecifiedValue` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/models.py#L2526'>zerver/models.py:2526:73:</a> B008 Do not perform function call `UnspecifiedValue` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/models.py#L2526'>zerver/models.py:2526:73:</a> B008 Do not perform function call `UnspecifiedValue` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/tests/test_has_request_variables.py#L138'>zerver/tests/test_has_request_variables.py:138:61:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/tests/test_has_request_variables.py#L138'>zerver/tests/test_has_request_variables.py:138:61:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/tests/test_typed_endpoint.py#L87'>zerver/tests/test_typed_endpoint.py:87:39:</a> B008 Do not perform function call `Foo` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/tests/test_typed_endpoint.py#L87'>zerver/tests/test_typed_endpoint.py:87:39:</a> B008 Do not perform function call `Foo` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/tornado/views.py#L114'>zerver/tornado/views.py:114:37:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/tornado/views.py#L114'>zerver/tornado/views.py:114:37:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L154'>zerver/views/custom_profile_fields.py:154:36:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L154'>zerver/views/custom_profile_fields.py:154:36:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L209'>zerver/views/custom_profile_fields.py:209:36:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L209'>zerver/views/custom_profile_fields.py:209:36:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L259'>zerver/views/custom_profile_fields.py:259:24:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L259'>zerver/views/custom_profile_fields.py:259:24:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L259'>zerver/views/custom_profile_fields.py:259:43:</a> B008 Do not perform function call `check_list` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L259'>zerver/views/custom_profile_fields.py:259:43:</a> B008 Do not perform function call `check_list` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L270'>zerver/views/custom_profile_fields.py:270:23:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L270'>zerver/views/custom_profile_fields.py:270:23:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L270'>zerver/views/custom_profile_fields.py:270:42:</a> B008 Do not perform function call `check_list` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L270'>zerver/views/custom_profile_fields.py:270:42:</a> B008 Do not perform function call `check_list` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L293'>zerver/views/custom_profile_fields.py:293:48:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L293'>zerver/views/custom_profile_fields.py:293:48:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L294'>zerver/views/custom_profile_fields.py:294:24:</a> B008 Do not perform function call `check_list` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/custom_profile_fields.py#L294'>zerver/views/custom_profile_fields.py:294:24:</a> B008 Do not perform function call `check_list` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/events_register.py#L52'>zerver/views/events_register.py:52:54:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/events_register.py#L52'>zerver/views/events_register.py:52:54:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/events_register.py#L53'>zerver/views/events_register.py:53:24:</a> B008 Do not perform function call `check_dict` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/events_register.py#L53'>zerver/views/events_register.py:53:24:</a> B008 Do not perform function call `check_dict` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
- <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/events_register.py#L78'>zerver/views/events_register.py:78:23:</a> B008 Do not perform function call `REQ` in argument defaults
+ <a href='https://github.com/zulip/zulip/blob/8cf436370ccc44f9d6927b6054954fb18f70c9c2/zerver/views/events_register.py#L78'>zerver/views/events_register.py:78:23:</a> B008 Do not perform function call `REQ` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable
... 110 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B008 | 186 | 93 | 93 | 0 | 0 |

</p>
</details>




---

_@konstin approved on 2023-11-06 09:12_

I'd like a shorter error message with a fix showing using the singleton

---

_Merged by @charliermarsh on 2023-11-07 04:53_

---

_Closed by @charliermarsh on 2023-11-07 04:53_

---

_Branch deleted on 2023-11-07 04:53_

---

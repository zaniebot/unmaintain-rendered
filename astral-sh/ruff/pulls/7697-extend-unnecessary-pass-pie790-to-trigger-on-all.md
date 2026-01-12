```yaml
number: 7697
title: "Extend `unnecessary-pass` (`PIE790`) to trigger on all unnecessary `pass` statements"
type: pull_request
state: merged
author: tjkuson
labels: []
assignees: []
merged: true
base: main
head: pass
created_at: 2023-09-28T14:35:48Z
updated_at: 2023-09-29T10:11:30Z
url: https://github.com/astral-sh/ruff/pull/7697
synced_at: 2026-01-12T15:55:24Z
```

# Extend `unnecessary-pass` (`PIE790`) to trigger on all unnecessary `pass` statements

---

_@tjkuson_

## Summary

Extend `unnecessary-pass` (`PIE790`) to trigger on all unnecessary `pass` statements by checking for `pass` statements in any class or function body with more than one statement.

Closes #7600.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-09-28 14:53_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+15, -0, 0 error(s))

<details><summary>airflow (+2, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/55fbdfea4235569c91942e6b2227090366e0e75a/airflow/models/abstractoperator.py#L696'>airflow/models/abstractoperator.py:696:17:</a> PIE790 [*] Unnecessary `pass` statement
+ <a href='https://github.com/apache/airflow/blob/55fbdfea4235569c91942e6b2227090366e0e75a/airflow/utils/log/file_task_handler.py#L459'>airflow/utils/log/file_task_handler.py:459:17:</a> PIE790 [*] Unnecessary `pass` statement
</pre>

</p>
</details>
<details><summary>bokeh (+2, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/96546ee6bfd68f660496924e71fd24e06afa38c4/src/bokeh/embed/util.py#L163'>src/bokeh/embed/util.py:163:13:</a> PIE790 [*] Unnecessary `pass` statement
+ <a href='https://github.com/bokeh/bokeh/blob/96546ee6bfd68f660496924e71fd24e06afa38c4/tests/unit/bokeh/command/subcommands/test_serve.py#L528'>tests/unit/bokeh/command/subcommands/test_serve.py:528:25:</a> PIE790 [*] Unnecessary `pass` statement
</pre>

</p>
</details>
<details><summary>content (+10, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/ad4c5b3f570f1b9de1377caddc376af68eb67d05/Packs/FeedRecordedFuture/Integrations/FeedRecordedFuture/FeedRecordedFuture.py#L197'>Packs/FeedRecordedFuture/Integrations/FeedRecordedFuture/FeedRecordedFuture.py:197:17:</a> PIE790 [*] Unnecessary `pass` statement
+ <a href='https://github.com/demisto/content/blob/ad4c5b3f570f1b9de1377caddc376af68eb67d05/Packs/FireEyeHX/Integrations/FireEyeHX/FireEyeHX.py#L1946'>Packs/FireEyeHX/Integrations/FireEyeHX/FireEyeHX.py:1946:17:</a> PIE790 [*] Unnecessary `pass` statement
+ <a href='https://github.com/demisto/content/blob/ad4c5b3f570f1b9de1377caddc376af68eb67d05/Packs/FireEyeHX/Integrations/FireEyeHX/FireEyeHX.py#L2023'>Packs/FireEyeHX/Integrations/FireEyeHX/FireEyeHX.py:2023:9:</a> PIE790 [*] Unnecessary `pass` statement
+ <a href='https://github.com/demisto/content/blob/ad4c5b3f570f1b9de1377caddc376af68eb67d05/Packs/FireEyeHX/Integrations/FireEyeHXv2/FireEyeHXv2_test.py#L138'>Packs/FireEyeHX/Integrations/FireEyeHXv2/FireEyeHXv2_test.py:138:5:</a> PIE790 [*] Unnecessary `pass` statement
+ <a href='https://github.com/demisto/content/blob/ad4c5b3f570f1b9de1377caddc376af68eb67d05/Packs/Gmail/Integrations/Gmail/Gmail.py#L108'>Packs/Gmail/Integrations/Gmail/Gmail.py:108:9:</a> PIE790 [*] Unnecessary `pass` statement
+ <a href='https://github.com/demisto/content/blob/ad4c5b3f570f1b9de1377caddc376af68eb67d05/Packs/GooglePubSub/Integrations/GooglePubSub/GooglePubSub.py#L846'>Packs/GooglePubSub/Integrations/GooglePubSub/GooglePubSub.py:846:17:</a> PIE790 [*] Unnecessary `pass` statement
+ <a href='https://github.com/demisto/content/blob/ad4c5b3f570f1b9de1377caddc376af68eb67d05/Packs/MicrosoftExchangeOnPremise/Integrations/EWSv2/EWSv2.py#L490'>Packs/MicrosoftExchangeOnPremise/Integrations/EWSv2/EWSv2.py:490:13:</a> PIE790 [*] Unnecessary `pass` statement
+ <a href='https://github.com/demisto/content/blob/ad4c5b3f570f1b9de1377caddc376af68eb67d05/Packs/Slack/Integrations/Slack/Slack.py#L1461'>Packs/Slack/Integrations/Slack/Slack.py:1461:13:</a> PIE790 [*] Unnecessary `pass` statement
+ <a href='https://github.com/demisto/content/blob/ad4c5b3f570f1b9de1377caddc376af68eb67d05/Packs/TrendMicroEmailSecurity/Integrations/TrendMicroEmailSecurityEventCollector/TrendMicroEmailSecurityEventCollector.py#L383'>Packs/TrendMicroEmailSecurity/Integrations/TrendMicroEmailSecurityEventCollector/TrendMicroEmailSecurityEventCollector.py:383:9:</a> PIE790 [*] Unnecessary `pass` statement
+ <a href='https://github.com/demisto/content/blob/ad4c5b3f570f1b9de1377caddc376af68eb67d05/Packs/XMatters/Integrations/xMatters/xMatters.py#L456'>Packs/XMatters/Integrations/xMatters/xMatters.py:456:13:</a> PIE790 [*] Unnecessary `pass` statement
</pre>

</p>
</details>
<details><summary>zulip (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/c12d249627bb819586c43c25a40c3b96038b0244/zerver/lib/rate_limiter.py#L592'>zerver/lib/rate_limiter.py:592:9:</a> PIE790 [*] Unnecessary `pass` statement
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PIE790 | 15 | 15 | 0 |



---

_Marked ready for review by @tjkuson on 2023-09-28 14:54_

---

_@charliermarsh reviewed on 2023-09-28 23:39_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pie/rules/no_unnecessary_pass.rs`:71 on 2023-09-28 23:39_

I'm genuinely unsure what's faster: this, or passing the `pass` statement itself to `no_unnecessary_pass`, and then using `traversal::suite` to find the containing body and see if it's of length one or not.

---

_@charliermarsh reviewed on 2023-09-28 23:42_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pie/rules/no_unnecessary_pass.rs`:71 on 2023-09-28 23:42_

Like this is probably faster if `pass` statements are common, but they're relatively rare.

---

_@charliermarsh approved on 2023-09-29 01:23_

---

_Merged by @charliermarsh on 2023-09-29 01:39_

---

_Closed by @charliermarsh on 2023-09-29 01:39_

---

_Branch deleted on 2023-09-29 10:11_

---

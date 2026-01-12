```yaml
number: 7884
title: "add autofix for `PIE804`"
type: pull_request
state: merged
author: diceroll123
labels:
  - fixes
assignees: []
merged: true
base: main
head: PIE804-autofix
created_at: 2023-10-10T03:22:47Z
updated_at: 2023-10-11T01:13:51Z
url: https://github.com/astral-sh/ruff/pull/7884
synced_at: 2026-01-12T15:55:25Z
```

# add autofix for `PIE804`

---

_@diceroll123_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Adds autofix for `PIE804`, closes #6783. 

I've split the `if`s apart to make the diagnostics easier to read. Open to suggestions if that's not desired!

## Test Plan

<!-- How was it tested? -->

`cargo test`, and manually.

---

_Renamed from "add autofix for PIE804" to "add autofix for `PIE804`" by @diceroll123 on 2023-10-10 03:24_

---

_@diceroll123 reviewed on 2023-10-10 03:27_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_dict_kwargs.rs`:92 on 2023-10-10 03:27_

not _particularly_ proud of this, is there a less scary-looking way to do this?

---

_Comment by @github-actions[bot] on 2023-10-10 03:38_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+24, -24, 0 error(s))

<details><summary>airflow (+24, -24)</summary>
<p>

<pre>
- [*] 16008 fixable with the `--fix` option (6314 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 16031 fixable with the `--fix` option (6314 hidden fixes can be enabled with the `--unsafe-fixes` option).
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/amazon/aws/operators/test_eks.py#L728'>tests/providers/amazon/aws/operators/test_eks.py:728:30:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/amazon/aws/operators/test_eks.py#L728'>tests/providers/amazon/aws/operators/test_eks.py:728:30:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L102'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:102:9:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L102'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:102:9:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L132'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:132:9:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L132'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:132:9:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L181'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:181:9:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L181'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:181:9:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L219'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:219:9:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L219'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:219:9:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L268'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:268:9:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L268'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:268:9:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L61'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:61:9:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/amazon/aws/utils/test_waiter_with_logging.py#L61'>tests/providers/amazon/aws/utils/test_waiter_with_logging.py:61:9:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/cncf/kubernetes/utils/test_pod_manager.py#L803'>tests/providers/cncf/kubernetes/utils/test_pod_manager.py:803:30:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/cncf/kubernetes/utils/test_pod_manager.py#L803'>tests/providers/cncf/kubernetes/utils/test_pod_manager.py:803:30:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/google/cloud/hooks/test_life_sciences.py#L125'>tests/providers/google/cloud/hooks/test_life_sciences.py:125:24:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/google/cloud/hooks/test_life_sciences.py#L125'>tests/providers/google/cloud/hooks/test_life_sciences.py:125:24:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/google/cloud/hooks/test_life_sciences.py#L154'>tests/providers/google/cloud/hooks/test_life_sciences.py:154:24:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/google/cloud/hooks/test_life_sciences.py#L154'>tests/providers/google/cloud/hooks/test_life_sciences.py:154:24:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/google/cloud/hooks/test_life_sciences.py#L231'>tests/providers/google/cloud/hooks/test_life_sciences.py:231:24:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/google/cloud/hooks/test_life_sciences.py#L231'>tests/providers/google/cloud/hooks/test_life_sciences.py:231:24:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/google/cloud/hooks/test_life_sciences.py#L259'>tests/providers/google/cloud/hooks/test_life_sciences.py:259:24:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/google/cloud/hooks/test_life_sciences.py#L259'>tests/providers/google/cloud/hooks/test_life_sciences.py:259:24:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/google/cloud/operators/test_kubernetes_engine.py#L404'>tests/providers/google/cloud/operators/test_kubernetes_engine.py:404:30:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/google/cloud/operators/test_kubernetes_engine.py#L404'>tests/providers/google/cloud/operators/test_kubernetes_engine.py:404:30:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/google/firebase/hooks/test_firestore.py#L100'>tests/providers/google/firebase/hooks/test_firestore.py:100:24:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/google/firebase/hooks/test_firestore.py#L100'>tests/providers/google/firebase/hooks/test_firestore.py:100:24:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/google/firebase/hooks/test_firestore.py#L122'>tests/providers/google/firebase/hooks/test_firestore.py:122:24:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/google/firebase/hooks/test_firestore.py#L122'>tests/providers/google/firebase/hooks/test_firestore.py:122:24:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/google/firebase/hooks/test_firestore.py#L189'>tests/providers/google/firebase/hooks/test_firestore.py:189:24:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/google/firebase/hooks/test_firestore.py#L189'>tests/providers/google/firebase/hooks/test_firestore.py:189:24:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/google/firebase/hooks/test_firestore.py#L216'>tests/providers/google/firebase/hooks/test_firestore.py:216:24:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/google/firebase/hooks/test_firestore.py#L216'>tests/providers/google/firebase/hooks/test_firestore.py:216:24:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/telegram/hooks/test_telegram.py#L106'>tests/providers/telegram/hooks/test_telegram.py:106:9:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/telegram/hooks/test_telegram.py#L106'>tests/providers/telegram/hooks/test_telegram.py:106:9:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/telegram/hooks/test_telegram.py#L125'>tests/providers/telegram/hooks/test_telegram.py:125:9:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/telegram/hooks/test_telegram.py#L125'>tests/providers/telegram/hooks/test_telegram.py:125:9:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/telegram/hooks/test_telegram.py#L144'>tests/providers/telegram/hooks/test_telegram.py:144:9:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/telegram/hooks/test_telegram.py#L144'>tests/providers/telegram/hooks/test_telegram.py:144:9:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/telegram/hooks/test_telegram.py#L171'>tests/providers/telegram/hooks/test_telegram.py:171:9:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/telegram/hooks/test_telegram.py#L171'>tests/providers/telegram/hooks/test_telegram.py:171:9:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/telegram/hooks/test_telegram.py#L191'>tests/providers/telegram/hooks/test_telegram.py:191:9:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/providers/telegram/hooks/test_telegram.py#L191'>tests/providers/telegram/hooks/test_telegram.py:191:9:</a> PIE804 [*] Unnecessary `dict` kwargs
- <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/utils/test_compression.py#L76'>tests/utils/test_compression.py:76:13:</a> PIE804 Unnecessary `dict` kwargs
+ <a href='https://github.com/apache/airflow/blob/8fdf3582c2967161dd794f7efb53691d092f0ce6/tests/utils/test_compression.py#L76'>tests/utils/test_compression.py:76:13:</a> PIE804 [*] Unnecessary `dict` kwargs
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PIE804 | 46 | 23 | 23 |



---

_@charliermarsh reviewed on 2023-10-11 00:59_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_dict_kwargs.rs`:87 on 2023-10-11 00:59_

I restructured it such that, when we validate the keyword arguments, we also return the valid keyword argument _name_. That lets us avoid a bunch of unwraps below. In other words: when we validate, we return the validated data.

---

_Label `autofix` added by @charliermarsh on 2023-10-11 00:59_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/flake8_pie/rules/unnecessary_dict_kwargs.rs`:87 on 2023-10-11 01:00_

ooo. Love it! Thanks!

---

_@diceroll123 reviewed on 2023-10-11 01:00_

---

_Merged by @charliermarsh on 2023-10-11 01:07_

---

_Closed by @charliermarsh on 2023-10-11 01:07_

---

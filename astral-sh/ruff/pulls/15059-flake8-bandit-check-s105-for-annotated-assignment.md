```yaml
number: 15059
title: "[`flake8-bandit`] Check `S105` for annotated assignment"
type: pull_request
state: merged
author: tarasmatsyk
labels:
  - rule
assignees: []
merged: true
base: main
head: fix-S105-false-negative
created_at: 2024-12-19T12:00:45Z
updated_at: 2024-12-19T12:28:33Z
url: https://github.com/astral-sh/ruff/pull/15059
synced_at: 2026-01-12T15:55:50Z
```

# [`flake8-bandit`] Check `S105` for annotated assignment

---

_@tarasmatsyk_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

A follow up PR on https://github.com/astral-sh/ruff/issues/14991
Ruff ignores hardcoded passwords for typed variables. Add a rule to catch passwords in typed code bases

## Test Plan

Includes 2 more test typed variables 


---

_Label `rule` added by @MichaReiser on 2024-12-19 12:05_

---

_Comment by @github-actions[bot] on 2024-12-19 12:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_sdk_config/latch.py#L64'>src/latch_sdk_config/latch.py:64:23:</a> S105 Possible hardcoded password assigned to: "get_secret"
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_sdk_config/latch.py#L65'>src/latch_sdk_config/latch.py:65:29:</a> S105 Possible hardcoded password assigned to: "get_secret_local"
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/f2e1c1eae8fcfa8b20ed3db45d1142899e3e96ff/zerver/migrations/0209_user_profile_no_empty_password.py#L69'>zerver/migrations/0209_user_profile_no_empty_password.py:69:44:</a> S105 Possible hardcoded password assigned to: "USER_PASSWORD_CHANGED"
+ <a href='https://github.com/zulip/zulip/blob/f2e1c1eae8fcfa8b20ed3db45d1142899e3e96ff/zerver/tests/test_signup.py#L934'>zerver/tests/test_signup.py:934:32:</a> S105 Possible hardcoded password assigned to: "password"
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S105 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_sdk_config/latch.py#L64'>src/latch_sdk_config/latch.py:64:23:</a> S105 Possible hardcoded password assigned to: "get_secret"
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_sdk_config/latch.py#L65'>src/latch_sdk_config/latch.py:65:29:</a> S105 Possible hardcoded password assigned to: "get_secret_local"
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/f2e1c1eae8fcfa8b20ed3db45d1142899e3e96ff/zerver/migrations/0209_user_profile_no_empty_password.py#L69'>zerver/migrations/0209_user_profile_no_empty_password.py:69:44:</a> S105 Possible hardcoded password assigned to: "USER_PASSWORD_CHANGED"
+ <a href='https://github.com/zulip/zulip/blob/f2e1c1eae8fcfa8b20ed3db45d1142899e3e96ff/zerver/tests/test_signup.py#L934'>zerver/tests/test_signup.py:934:32:</a> S105 Possible hardcoded password assigned to: "password"
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S105 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1669 on 2024-12-19 12:17_

I think we should switch the condition so that we only check when the rule is enabled
```suggestion
			if checker.enabled(Rule::HardcodedPasswordString) {
            	if let Some(value) = value.as_deref() {
                    flake8_bandit::rules::assign_hardcoded_password_string(
                        checker,
                        value,
                        std::slice::from_ref(target),
                    );
                }
            }
```

---

_@dhruvmanila approved on 2024-12-19 12:17_

---

_Renamed from "[flake8-bandit] Fix false negative S105 for typed variables" to "[`flake8-bandit`] Check `S105` for annotated assignment" by @dhruvmanila on 2024-12-19 12:17_

---

_@tarasmatsyk reviewed on 2024-12-19 12:22_

---

_Review comment by @tarasmatsyk on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1669 on 2024-12-19 12:22_

done,

I was using `Rule::LambdaAssignment` as an example:
https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/checkers/ast/analyze/statement.rs#L1633 

---

_Merged by @MichaReiser on 2024-12-19 12:26_

---

_Closed by @MichaReiser on 2024-12-19 12:26_

---

_Branch deleted on 2024-12-19 12:27_

---

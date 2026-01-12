```yaml
number: 8140
title: "[`SIM112`] Ignore `https_proxy`, `http_proxy`, and `no_proxy`"
type: pull_request
state: merged
author: harupy
labels:
  - rule
assignees: []
merged: true
base: main
head: proxy-sim112
created_at: 2023-10-23T11:46:28Z
updated_at: 2023-10-23T13:48:43Z
url: https://github.com/astral-sh/ruff/pull/8140
synced_at: 2026-01-12T15:55:25Z
```

# [`SIM112`] Ignore `https_proxy`, `http_proxy`, and `no_proxy`

---

_@harupy_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Close #8123 

## Test Plan

<!-- How was it tested? -->

New test cases


---

_Review comment by @konstin on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_expr.rs`:119 on 2023-10-23 11:51_

Could you add a comment linking to the sources/discussion for why these are lowercase?

---

_@konstin approved on 2023-10-23 11:51_

---

_@harupy reviewed on 2023-10-23 11:59_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_expr.rs`:119 on 2023-10-23 11:59_

Added!

---

_Comment by @github-actions[bot] on 2023-10-23 12:31_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+4, -1, 0 error(s))

<details><summary>zulip (+4, -1)</summary>
<p>

<pre>
- [*] 7906 fixable with the `--fix` option (12150 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 7909 fixable with the `--fix` option (12150 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ <a href='https://github.com/zulip/zulip/blob/b1f6ff20efd1021f592bf75c47803c83ab8fc6e4/tools/lib/provision.py#L410'>tools/lib/provision.py:410:60:</a> RUF100 [*] Unused `noqa` directive (unused: `SIM112`)
+ <a href='https://github.com/zulip/zulip/blob/b1f6ff20efd1021f592bf75c47803c83ab8fc6e4/tools/lib/provision.py#L411'>tools/lib/provision.py:411:62:</a> RUF100 [*] Unused `noqa` directive (unused: `SIM112`)
+ <a href='https://github.com/zulip/zulip/blob/b1f6ff20efd1021f592bf75c47803c83ab8fc6e4/tools/lib/provision.py#L412'>tools/lib/provision.py:412:56:</a> RUF100 [*] Unused `noqa` directive (unused: `SIM112`)
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| RUF100 | 3 | 3 | 0 |



---

_@zanieb approved on 2023-10-23 13:48_

---

_Merged by @zanieb on 2023-10-23 13:48_

---

_Closed by @zanieb on 2023-10-23 13:48_

---

_Label `rule` added by @zanieb on 2023-10-23 13:48_

---

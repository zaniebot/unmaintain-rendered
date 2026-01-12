```yaml
number: 8108
title: Upgrade mutable-argument-defaults to unsafe
type: pull_request
state: merged
author: cjolowicz
labels: []
assignees: []
merged: true
base: main
head: upgrade-mutable-argument-default-to-unsafe
created_at: 2023-10-21T15:34:00Z
updated_at: 2023-10-21T19:48:18Z
url: https://github.com/astral-sh/ruff/pull/8108
synced_at: 2026-01-12T02:32:41Z
```

# Upgrade mutable-argument-defaults to unsafe

---

_Pull request opened by @cjolowicz on 2023-10-21 15:34_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #8104

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->

Covered by snapshot tests.


---

_Comment by @github-actions[bot] on 2023-10-21 15:48_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+3, -3, 0 error(s))

<details><summary>airflow (+1, -1)</summary>
<p>

<pre>
- [*] 16165 fixable with the `--fix` option (6215 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 16165 fixable with the `--fix` option (6225 hidden fixes can be enabled with the `--unsafe-fixes` option).
</pre>

</p>
</details>
<details><summary>bokeh (+1, -1)</summary>
<p>

<pre>
- [*] 17800 fixable with the `--fix` option (4370 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 17800 fixable with the `--fix` option (4404 hidden fixes can be enabled with the `--unsafe-fixes` option).
</pre>

</p>
</details>
<details><summary>latch (+1, -1)</summary>
<p>

<pre>
- [*] 206 fixable with the `--fix` option (35 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 206 fixable with the `--fix` option (40 hidden fixes can be enabled with the `--unsafe-fixes` option).
</pre>

</p>
</details>



---

_@zanieb reviewed on 2023-10-21 15:55_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:206 on 2023-10-21 15:55_

cc @charliermarsh 

Can we add a note here explaining _why_ the fix is unsafe? I'm not sure why it was marked as unapplicable originally as the fix looks relatively safe to me?

---

_@cjolowicz reviewed on 2023-10-21 16:05_

---

_Review comment by @cjolowicz on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:206 on 2023-10-21 16:05_

Commented on this in the issue: https://github.com/astral-sh/ruff/issues/8104#issuecomment-1773834910

---

_@cjolowicz reviewed on 2023-10-21 16:30_

---

_Review comment by @cjolowicz on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:206 on 2023-10-21 16:30_

I've added a `Known problems` section to explain why the fix is unsafe.

---

_@charliermarsh reviewed on 2023-10-21 19:29_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:206 on 2023-10-21 19:29_

Thank you!

---

_Merged by @charliermarsh on 2023-10-21 19:29_

---

_Closed by @charliermarsh on 2023-10-21 19:29_

---

_Branch deleted on 2023-10-21 19:48_

---

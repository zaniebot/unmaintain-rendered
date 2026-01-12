```yaml
number: 7970
title: "add autofix for `D301`"
type: pull_request
state: merged
author: diceroll123
labels:
  - fixes
assignees: []
merged: true
base: main
head: fix-D301
created_at: 2023-10-16T00:38:16Z
updated_at: 2023-10-18T02:26:15Z
url: https://github.com/astral-sh/ruff/pull/7970
synced_at: 2026-01-12T15:55:25Z
```

# add autofix for `D301`

---

_@diceroll123_

## Summary

Add fix for `D301`

## Test Plan

`cargo test` and manually


---

_Comment by @github-actions[bot] on 2023-10-16 01:05_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+3, -3, 0 error(s))

<details><summary>bokeh (+1, -1)</summary>
<p>

<pre>
- [*] 19268 fixable with the `--fix` option (4369 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 19268 fixable with the `--fix` option (4370 hidden fixes can be enabled with the `--unsafe-fixes` option).
</pre>

</p>
</details>
<details><summary>latch (+1, -1)</summary>
<p>

<pre>
- [*] 205 fixable with the `--fix` option (33 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 205 fixable with the `--fix` option (35 hidden fixes can be enabled with the `--unsafe-fixes` option).
</pre>

</p>
</details>
<details><summary>zulip (+1, -1)</summary>
<p>

<pre>
- [*] 7906 fixable with the `--fix` option (12146 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 7906 fixable with the `--fix` option (12148 hidden fixes can be enabled with the `--unsafe-fixes` option).
</pre>

</p>
</details>



---

_@charliermarsh reviewed on 2023-10-18 01:28_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pydocstyle/rules/backslashes.rs`:83 on 2023-10-18 01:28_

This does change the meaning (content) of the docstring, so I think it should at least be an `unsafe_edit`, right?

---

_@diceroll123 reviewed on 2023-10-18 01:30_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/pydocstyle/rules/backslashes.rs`:83 on 2023-10-18 01:30_

Good point! I'll modify accordingly.

---

_Label `autofix` added by @charliermarsh on 2023-10-18 02:11_

---

_Merged by @charliermarsh on 2023-10-18 02:19_

---

_Closed by @charliermarsh on 2023-10-18 02:19_

---

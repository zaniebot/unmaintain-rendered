```yaml
number: 7910
title: "add autofix for `PLR1714`"
type: pull_request
state: merged
author: diceroll123
labels:
  - fixes
assignees: []
merged: true
base: main
head: autofix-PLR1714
created_at: 2023-10-11T06:09:43Z
updated_at: 2023-10-11T14:49:31Z
url: https://github.com/astral-sh/ruff/pull/7910
synced_at: 2026-01-12T15:55:25Z
```

# add autofix for `PLR1714`

---

_@diceroll123_

## Summary

Add autofix for `PLR1714` using tuples.

If added complexity is desired, we can lean into the `set` part by doing some kind of builtin check on all of the comparator elements for starters, since we otherwise don't know if something's hashable.

## Test Plan

`cargo test`, and manually.

---

_Comment by @github-actions[bot] on 2023-10-11 06:26_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+5, -5, 0 error(s))

<details><summary>airflow (+1, -1)</summary>
<p>

<pre>
- [*] 16031 fixable with the `--fix` option (6314 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 16031 fixable with the `--fix` option (6330 hidden fixes can be enabled with the `--unsafe-fixes` option).
</pre>

</p>
</details>
<details><summary>aws-sam-cli (+1, -1)</summary>
<p>

<pre>
- [*] 756 fixable with the `--fix` option (121 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 756 fixable with the `--fix` option (122 hidden fixes can be enabled with the `--unsafe-fixes` option).
</pre>

</p>
</details>
<details><summary>bokeh (+1, -1)</summary>
<p>

<pre>
- [*] 17800 fixable with the `--fix` option (4721 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 17800 fixable with the `--fix` option (4727 hidden fixes can be enabled with the `--unsafe-fixes` option).
</pre>

</p>
</details>
<details><summary>securedrop (+1, -1)</summary>
<p>

<pre>
- [*] 48 fixable with the `--fix` option (1 hidden fix can be enabled with the `--unsafe-fixes` option).
+ [*] 48 fixable with the `--fix` option (3 hidden fixes can be enabled with the `--unsafe-fixes` option).
</pre>

</p>
</details>
<details><summary>pymilvus (+1, -1)</summary>
<p>

<pre>
- [*] 233 fixable with the `--fix` option (129 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 233 fixable with the `--fix` option (132 hidden fixes can be enabled with the `--unsafe-fixes` option).
</pre>

</p>
</details>



---

_Review comment by @konstin on `crates/ruff_linter/src/rules/pylint/rules/repeated_equality_comparison.rs`:147 on 2023-10-11 13:05_

```suggestion
                        elts: comparators.iter().copied().cloned().collect(),
```

---

_@konstin approved on 2023-10-11 13:08_

---

_Label `autofix` added by @charliermarsh on 2023-10-11 14:30_

---

_Merged by @charliermarsh on 2023-10-11 14:42_

---

_Closed by @charliermarsh on 2023-10-11 14:42_

---

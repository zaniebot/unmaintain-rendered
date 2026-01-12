```yaml
number: 7690
title: Update logical line rules for the new f-string tokens
type: pull_request
state: merged
author: dhruvmanila
labels:
  - rule
  - python312
assignees: []
merged: true
base: dhruv/pep-701
head: dhruv/fstring-logical-line-rules
created_at: 2023-09-28T03:58:43Z
updated_at: 2023-09-29T02:46:11Z
url: https://github.com/astral-sh/ruff/pull/7690
synced_at: 2026-01-12T15:55:24Z
```

# Update logical line rules for the new f-string tokens

---

_@dhruvmanila_

## Summary

This PR updates the logical line rules for the new f-string tokens.

Specifically, it tries to ignore the rules for which it might be hard to differentiate between different usages inside f-strings. For some tokens, the whitespace addition or removal could change the semantics so they're ignored for every scenario inside f-string.

- Ignore `E225` for the `=` operator. For example, `f"{x=}"` we don't want to add whitespace as the output will then be different than what the user indended.
- Ignore `E231` for the `:` operator. For example, `f"{x:.3f}"` we don't want to add whitespace as the output will be different:
```
In [1]: a = 100

In [2]: f'{a: .3f}'
Out[2]: ' 100.000'

In [3]: f'{a:.3f}'
Out[3]: '100.000'
```

- Ignore `E201`, `E202` for `{` and `}` tokens. The double braces are used as an escape mechanism to include the raw character (`f"{{foo}}"`) but if a dictionary is being used then the whitespace is required (`f"{ {'a': 1} }"`).

## Test Plan

Add new test cases for f-strings along with nested f-strings and update the snapshots.


---

_Comment by @dhruvmanila on 2023-09-28 03:59_

Current dependencies on/for this PR:
* main
  * **PR #7376** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7376" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7690** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7690" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #7667** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7667" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7597** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7597" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7588** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7588" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #7589** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7589" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7586** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7586" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7515** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7515" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7477** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7477" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7387** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7387" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7378** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7378" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7329** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7329" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7328** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7328" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7327** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7327" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7326** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7326" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7325** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7325" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/7690?utm_source=stack-comment).

---

_Comment by @dhruvmanila on 2023-09-28 03:59_

Current dependencies on/for this PR:
* main
  * **PR #7376** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7376" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7690** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7690" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #7667** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7667" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7597** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7597" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7588** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7588" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #7589** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7589" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7586** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7586" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7515** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7515" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7477** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7477" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7387** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7387" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7378** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7378" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7329** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7329" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7328** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7328" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7327** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7327" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7326** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7326" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7325** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7325" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/7690?utm_source=stack-comment).

---

_Marked ready for review by @dhruvmanila on 2023-09-28 04:06_

---

_Review requested from @charliermarsh by @dhruvmanila on 2023-09-28 04:06_

---

_Label `rule` added by @dhruvmanila on 2023-09-28 04:07_

---

_Label `python312` added by @dhruvmanila on 2023-09-28 04:07_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:148 on 2023-09-28 04:08_

It looks like we're not setting `prev_token` because we're continuing. Is that ok?

---

_@charliermarsh reviewed on 2023-09-28 04:08_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:146 on 2023-09-28 04:08_

Should we just skip all of this when we're inside an f-string?

---

_@charliermarsh reviewed on 2023-09-28 04:08_

---

_Comment by @codspeed-hq[bot] on 2023-09-28 04:08_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/fstring-logical-line-rules)

### Merging #7690 will **degrade performances by 2.28%**

<sub>Comparing <code>dhruv/fstring-logical-line-rules</code> (11e27da) with <code>dhruv/pep-701</code> (d60f30e)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/fstring-logical-line-rules)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/pep-701` | `dhruv/fstring-logical-line-rules` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[large/dataset.py]` | 93.4 ms | 95.6 ms | -2.28% |


---

_@charliermarsh reviewed on 2023-09-28 04:10_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/missing_whitespace_around_operator.rs`:162 on 2023-09-28 04:10_

Again, should we just have something like:

```rust
if fstrings > 0 {
    continue;
}
```

Or is it important that we run this body logic?

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/missing_whitespace_around_operator.rs`:162 on 2023-09-28 04:11_

Oh, so we're trying to catch _some_ of these errors within f-strings, but not all -- is that right?

---

_@charliermarsh reviewed on 2023-09-28 04:11_

---

_@charliermarsh reviewed on 2023-09-28 04:11_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:165 on 2023-09-28 04:11_

Again we're not setting `prev_token` due to the use of `continue`, I think.

---

_@charliermarsh reviewed on 2023-09-28 04:12_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:146 on 2023-09-28 04:12_

Oh, or actually, we _do_ want to catch some errors within f-strings -- is that right?

---

_@dhruvmanila reviewed on 2023-09-28 04:12_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:148 on 2023-09-28 04:12_

Good point. I think it's ok but I'll remove it. It might also make sense to remove the `continue;` as pointed out in the other comment.

---

_@dhruvmanila reviewed on 2023-09-28 04:14_

---

_@charliermarsh reviewed on 2023-09-28 04:14_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:146 on 2023-09-28 04:14_

Removing whitespaces before the punctuations should be fine. It's only when they're removed after when the semantics changes.

So, `f"{foo   :.3f}"` and `f"{foo:.3f}"` are equivalent.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:148 on 2023-09-28 04:14_

Is the goal to detect _some_ violations in f-strings, but skip others that could be false positives? Or is it to avoid flagging _anything_ in f-strings (as happens today on `main`, IIUC)?

---

_@dhruvmanila reviewed on 2023-09-28 04:16_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/missing_whitespace_around_operator.rs`:162 on 2023-09-28 04:16_

Yes, it's only when the equal sign is present which in f-string context will be used as debug expression. So, `f"{x=}` and `f"{x = }"` gives different output.

---

_@dhruvmanila reviewed on 2023-09-28 04:16_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:146 on 2023-09-28 04:16_

> Oh, or actually, we _do_ want to catch some errors within f-strings -- is that right?

Yes

---

_@dhruvmanila reviewed on 2023-09-28 04:17_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:148 on 2023-09-28 04:17_

The former, but only in cases where it's easier to differentiate between usages.

---

_Comment by @github-actions[bot] on 2023-09-28 04:18_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -105, 0 error(s))

<details><summary>rotki (+0, -37)</summary>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/base.py#L148'>rotkehlchen/accounting/structures/base.py:148:37:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/base.py#L149'>rotkehlchen/accounting/structures/base.py:149:35:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/base.py#L150'>rotkehlchen/accounting/structures/base.py:150:30:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/base.py#L151'>rotkehlchen/accounting/structures/base.py:151:29:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/base.py#L152'>rotkehlchen/accounting/structures/base.py:152:31:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/base.py#L153'>rotkehlchen/accounting/structures/base.py:153:34:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/base.py#L154'>rotkehlchen/accounting/structures/base.py:154:26:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/base.py#L155'>rotkehlchen/accounting/structures/base.py:155:28:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/base.py#L156'>rotkehlchen/accounting/structures/base.py:156:35:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/base.py#L157'>rotkehlchen/accounting/structures/base.py:157:26:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/base.py#L158'>rotkehlchen/accounting/structures/base.py:158:31:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/eth2.py#L139'>rotkehlchen/accounting/structures/eth2.py:139:58:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/eth2.py#L139'>rotkehlchen/accounting/structures/eth2.py:139:77:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/eth2.py#L265'>rotkehlchen/accounting/structures/eth2.py:265:139:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/eth2.py#L265'>rotkehlchen/accounting/structures/eth2.py:265:53:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/eth2.py#L265'>rotkehlchen/accounting/structures/eth2.py:265:72:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/eth2.py#L394'>rotkehlchen/accounting/structures/eth2.py:394:55:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/eth2.py#L394'>rotkehlchen/accounting/structures/eth2.py:394:74:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/eth2.py#L394'>rotkehlchen/accounting/structures/eth2.py:394:91:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/evm_event.py#L261'>rotkehlchen/accounting/structures/evm_event.py:261:28:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/evm_event.py#L262'>rotkehlchen/accounting/structures/evm_event.py:262:33:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/evm_event.py#L263'>rotkehlchen/accounting/structures/evm_event.py:263:28:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/evm_event.py#L264'>rotkehlchen/accounting/structures/evm_event.py:264:28:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/accounting/structures/evm_event.py#L265'>rotkehlchen/accounting/structures/evm_event.py:265:31:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/chain/ethereum/modules/aave/v2/decoder.py#L175'>rotkehlchen/chain/ethereum/modules/aave/v2/decoder.py:175:126:</a> E231 [*] Missing whitespace after ':'
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/fval.py#L159'>rotkehlchen/fval.py:159:31:</a> E231 [*] Missing whitespace after ':'
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/fval.py#L49'>rotkehlchen/fval.py:49:27:</a> E231 [*] Missing whitespace after ':'
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/tests/conftest.py#L140'>rotkehlchen/tests/conftest.py:140:47:</a> E231 [*] Missing whitespace after ':'
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/tests/unit/test_defi_oracles.py#L44'>rotkehlchen/tests/unit/test_defi_oracles.py:44:102:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/rotkehlchen/tests/unit/test_defi_oracles.py#L44'>rotkehlchen/tests/unit/test_defi_oracles.py:44:84:</a> E225 Missing whitespace around operator
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/tools/profiling/cpu.py#L12'>tools/profiling/cpu.py:12:35:</a> E231 [*] Missing whitespace after ':'
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/tools/profiling/cpu.py#L13'>tools/profiling/cpu.py:13:33:</a> E231 [*] Missing whitespace after ':'
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/tools/profiling/cpu.py#L30'>tools/profiling/cpu.py:30:36:</a> E231 [*] Missing whitespace after ':'
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/tools/profiling/graph.py#L219'>tools/profiling/graph.py:219:33:</a> E231 [*] Missing whitespace after ':'
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/tools/profiling/sampler.py#L64'>tools/profiling/sampler.py:64:30:</a> E231 [*] Missing whitespace after ':'
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/tools/profiling/sampler.py#L64'>tools/profiling/sampler.py:64:43:</a> E231 [*] Missing whitespace after ':'
- <a href='https://github.com/rotki/rotki/blob/3a9f74fdf8a0f4cfe76b72db05d72b93296010c3/tools/profiling/trace.py#L26'>tools/profiling/trace.py:26:28:</a> E231 [*] Missing whitespace after ':'
</pre>

</p>
</details>
<details><summary>sphinx (+0, -68)</summary>
<p>

<pre>
- 
-      = help: Added missing whitespace after ':'
-      |
-      |                                                                                   ^ E231
-     = help: Added missing whitespace after ':'
-     |
-     |                                                                    ^ E231
-     |                                                      ^ E231
-     |                                                   ^ E231
-     |                                   ^ E231
-     |                                  ^ E231
-     |                                 ^ E231
-    = help: Added missing whitespace after ':'
-    |
-    |                                                                         ^ E231
-    |                                                                 ^ E231
-    |                                                         ^ E231
-    |                                                 ^ E231
-    |                                 ^ E231
-    |                         ^ E231
- 1202 |     """Return an RFC 3339 formatted string representing the given timestamp."""
- 1203 |     seconds, fraction = divmod(timestamp, 1)
- 1204 |     return time.strftime("%Y-%m-%d %H:%M:%S", time.gmtime(seconds)) + f'.{fraction:.3f}'
- 130 |     if total <= 0:
- 131 |         return _('no translated elements!')
- 132 |     return f'{translated / total:.2%}'
- 144 |     if not content:
- 145 |         return ''
- 146 |     return f'{zlib.crc32(content):08x}'
- 22 |     weekday_name = _WEEKDAY_NAME[wd]
- 23 |     month = _MONTH_NAME[mn]
- 24 |     return f'{weekday_name}, {dd:02} {month} {yr:04} {hh:02}:{mm:02}:{ss:02} GMT'
- 282 |             res = r"%.5f\linewidth" % (amount_float / 100.0)
- 283 |         else:
- 284 |             res = f"{amount_float:.5f}{unit}"
- 285 |     return res
- 293 |         table.append([
- 294 |             'TOTAL',
- 295 |             f'{100 * len(all_documented_objects) / len(all_objects):.2f}%',
- 296 |             f'{len(all_objects) - len(all_documented_objects)}',
- 297 |         ])
- 341 |         c = f'<img class="math" src="{img_src}"' + get_tooltip(self, node)
- 342 |         if depth is not None:
- 343 |             c += f' style="vertical-align: {-depth:d}px"'
- 344 |         self.body.append(c + '/>')
- 345 |     raise nodes.SkipNode
- 725 |             for entry, (_proj, _ver, url_path, display_name) in inv_entries:
- 726 |                 display_name = display_name * (display_name != '-')
- 727 |                 print(f'    {entry:<40} {display_name:<40}: {url_path}')
- 728 |     except ValueError as exc:
- 729 |         print(exc.args[0] % exc.args[1:], file=sys.stderr)
- 76 |     logger.info(__('====================== slowest reading durations ======================='))
- 77 |     for docname, d in islice(durations, 5):
- 78 |         logger.info(f'{d:.3f} {docname}')  # NoQA: G004
- <a href='https://github.com/sphinx-doc/sphinx/blob/b9c85984d2f4863e4c09932c746d915e3233507b/sphinx/builders/html/__init__.py#L1204'>sphinx/builders/html/__init__.py:1204:83:</a> E231 Missing whitespace after ':'
- <a href='https://github.com/sphinx-doc/sphinx/blob/b9c85984d2f4863e4c09932c746d915e3233507b/sphinx/builders/html/_assets.py#L146'>sphinx/builders/html/_assets.py:146:34:</a> E231 Missing whitespace after ':'
- <a href='https://github.com/sphinx-doc/sphinx/blob/b9c85984d2f4863e4c09932c746d915e3233507b/sphinx/ext/coverage.py#L295'>sphinx/ext/coverage.py:295:68:</a> E231 Missing whitespace after ':'
- <a href='https://github.com/sphinx-doc/sphinx/blob/b9c85984d2f4863e4c09932c746d915e3233507b/sphinx/ext/duration.py#L78'>sphinx/ext/duration.py:78:25:</a> E231 Missing whitespace after ':'
- <a href='https://github.com/sphinx-doc/sphinx/blob/b9c85984d2f4863e4c09932c746d915e3233507b/sphinx/ext/imgmath.py#L343'>sphinx/ext/imgmath.py:343:51:</a> E231 Missing whitespace after ':'
- <a href='https://github.com/sphinx-doc/sphinx/blob/b9c85984d2f4863e4c09932c746d915e3233507b/sphinx/ext/intersphinx.py#L727'>sphinx/ext/intersphinx.py:727:35:</a> E231 Missing whitespace after ':'
- <a href='https://github.com/sphinx-doc/sphinx/blob/b9c85984d2f4863e4c09932c746d915e3233507b/sphinx/ext/intersphinx.py#L727'>sphinx/ext/intersphinx.py:727:54:</a> E231 Missing whitespace after ':'
- <a href='https://github.com/sphinx-doc/sphinx/blob/b9c85984d2f4863e4c09932c746d915e3233507b/sphinx/transforms/__init__.py#L132'>sphinx/transforms/__init__.py:132:33:</a> E231 Missing whitespace after ':'
- <a href='https://github.com/sphinx-doc/sphinx/blob/b9c85984d2f4863e4c09932c746d915e3233507b/sphinx/util/http_date.py#L24'>sphinx/util/http_date.py:24:33:</a> E231 Missing whitespace after ':'
- <a href='https://github.com/sphinx-doc/sphinx/blob/b9c85984d2f4863e4c09932c746d915e3233507b/sphinx/util/http_date.py#L24'>sphinx/util/http_date.py:24:49:</a> E231 Missing whitespace after ':'
- <a href='https://github.com/sphinx-doc/sphinx/blob/b9c85984d2f4863e4c09932c746d915e3233507b/sphinx/util/http_date.py#L24'>sphinx/util/http_date.py:24:57:</a> E231 Missing whitespace after ':'
- <a href='https://github.com/sphinx-doc/sphinx/blob/b9c85984d2f4863e4c09932c746d915e3233507b/sphinx/util/http_date.py#L24'>sphinx/util/http_date.py:24:65:</a> E231 Missing whitespace after ':'
- <a href='https://github.com/sphinx-doc/sphinx/blob/b9c85984d2f4863e4c09932c746d915e3233507b/sphinx/util/http_date.py#L24'>sphinx/util/http_date.py:24:73:</a> E231 Missing whitespace after ':'
- <a href='https://github.com/sphinx-doc/sphinx/blob/b9c85984d2f4863e4c09932c746d915e3233507b/sphinx/writers/latex.py#L284'>sphinx/writers/latex.py:284:34:</a> E231 Missing whitespace after ':'
</pre>

</p>
</details>
Rules changed: 3

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| E225 | 26 | 0 | 26 |
| E231 | 25 | 0 | 25 |
| G004 | 1 | 0 | 1 |



---

_@charliermarsh approved on 2023-09-28 04:24_

This looks good to me -- lets just make sure we're setting `prev_token` in any of those `continue` blocks.

---

_Comment by @dhruvmanila on 2023-09-28 04:25_

> This looks good to me -- lets just make sure we're setting `prev_token` in any of those `continue` blocks.

Thanks! I'll look at that in a bit

---

_Comment by @charliermarsh on 2023-09-28 04:28_

How did you identify this specific subset of logical line rules? Did they show up in the ecosystem CI on the PEP 701 PR?

---

_Comment by @dhruvmanila on 2023-09-28 04:54_

> How did you identify this specific subset of logical line rules? Did they show up in the ecosystem CI on the PEP 701 PR?

Yes, then I thought of checking out all the relevant operators and trivia tokens specific to f-string usages.

---

_Merged by @dhruvmanila on 2023-09-28 05:22_

---

_Closed by @dhruvmanila on 2023-09-28 05:22_

---

_Branch deleted on 2023-09-28 05:22_

---

_Branch restored on 2023-09-29 02:39_

---

_Branch deleted on 2023-09-29 02:46_

---

```yaml
number: 7905
title: "Allow `is` and `is` not for direct type comparisons"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/E721
created_at: 2023-10-11T00:33:33Z
updated_at: 2024-08-22T19:04:25Z
url: https://github.com/astral-sh/ruff/pull/7905
synced_at: 2026-01-10T21:38:31Z
```

# Allow `is` and `is` not for direct type comparisons

---

_Pull request opened by @charliermarsh on 2023-10-11 00:33_

## Summary

This PR updates our E721 implementation and semantics to match the updated `pycodestyle` logic, which I think is an improvement. Specifically, we now allow `type(obj) is int` for exact type comparisons, which were previously impossible. So now, we're largely just linting against code like `type(obj) == int`.

This change is gated to preview mode.

Closes https://github.com/astral-sh/ruff/issues/7904.

## Test Plan

Updated the test fixture and ensured parity with latest Flake8.

---

_Review requested from @zanieb by @charliermarsh on 2023-10-11 00:33_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pycodestyle/rules/type_comparison.rs`:73 on 2023-10-11 00:34_

Unlike pycodestyle, our rule is symmetric... so, e.g., we flag both `type(x) == int` and `int == type(x)`.

---

_@charliermarsh reviewed on 2023-10-11 00:34_

---

_Comment by @github-actions[bot] on 2023-10-11 00:47_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+3, -1, 0 error(s))

<details><summary>rotki (+3, -1)</summary>
<p>

<pre>
- [*] 11 fixable with the `--fix` option (225 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 13 fixable with the `--fix` option (225 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ <a href='https://github.com/rotki/rotki/blob/1b0e71f8476f8cc9003891ac2e4c8cce960414c3/rotkehlchen/tests/api/test_settings.py#L146'>rotkehlchen/tests/api/test_settings.py:146:81:</a> RUF100 [*] Unused `noqa` directive (unused: `E721`)
+ <a href='https://github.com/rotki/rotki/blob/1b0e71f8476f8cc9003891ac2e4c8cce960414c3/rotkehlchen/tests/api/test_settings.py#L150'>rotkehlchen/tests/api/test_settings.py:150:80:</a> RUF100 [*] Unused `noqa` directive (unused: `E721`)
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| RUF100 | 2 | 2 | 0 |



---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pycodestyle/rules/type_comparison.rs`:28 on 2023-10-11 04:32_

I think that the "Use instead" section needs to contain the following:

```python
if type(obj) is type(1):
	pass
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pycodestyle/rules/type_comparison.rs`:102 on 2023-10-11 04:34_

This isn't present in the refactored code, is that an expected behavior?

The following doesn't get violated then:
```python
if type(foo) == types.LambdaType:
    ...
```

---

_@dhruvmanila approved on 2023-10-11 04:39_

---

_@charliermarsh reviewed on 2023-10-12 13:16_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pycodestyle/rules/type_comparison.rs`:102 on 2023-10-12 13:16_

Yeah, they removed this from pycodestyle with the understanding that a user-defined `types` would be a lot more common than accessing the standard library's `types`. I'm torn on it but figured it's fine to follow suit.

---

_Comment by @charliermarsh on 2023-10-17 04:38_

This may need to go out under preview \cc @zanieb

---

_Comment by @charliermarsh on 2023-10-20 18:50_

@zanieb - What do you think of this?

---

_Comment by @zanieb on 2023-10-20 19:21_

I agree preview makes sense for this change.

Perhaps we should clarify in the versioning policy that we will confine changes in the _scope_ of rules to minor releases.

---

_Label `rule` added by @charliermarsh on 2023-10-20 23:16_

---

_Merged by @charliermarsh on 2023-10-20 23:27_

---

_Closed by @charliermarsh on 2023-10-20 23:27_

---

_Branch deleted on 2023-10-20 23:27_

---

_@CharString reviewed on 2024-08-22 19:04_

---

_Review comment by @CharString on `crates/ruff_linter/src/rules/pycodestyle/rules/type_comparison.rs`:73 on 2024-08-22 19:04_

`is` is commutative, but `==` isn't in Python:
```
[ins] In [14]: (a == b) == (b == a)
Out[14]: False

[ins] In [15]: (a is b) == (b is a)
Out[15]: True
```

But I don't think that matters for this linter rule. :)

---

```yaml
number: 10539
title: "[`flake8-bugbear`] Avoid false positive for usage after `continue` (`B031`)"
type: pull_request
state: merged
author: yt2b
labels:
  - bug
assignees: []
merged: true
base: main
head: reuse-of-groupby-generator
created_at: 2024-03-23T23:19:21Z
updated_at: 2024-03-26T14:05:12Z
url: https://github.com/astral-sh/ruff/pull/10539
synced_at: 2026-01-10T22:47:02Z
```

# [`flake8-bugbear`] Avoid false positive for usage after `continue` (`B031`)

---

_Pull request opened by @yt2b on 2024-03-23 23:19_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

#10337
I've fixed the code to count usage of variable.
Usage count inside the block is reset when there is a following statement.
- continue
- break
- return 

## Test Plan

Add test case.


---

_Comment by @github-actions[bot] on 2024-03-23 23:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-24 16:09_

---

_Label `bug` added by @charliermarsh on 2024-03-24 16:09_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-24 20:14_

---

_@charliermarsh approved on 2024-03-25 00:31_

Looks good, thanks!

---

_Renamed from "[`flake8-bugbear`] Fix usage count algorithm (`B031`)" to "[`flake8-bugbear`] Avoid false positive for usage after `continue` (`B031`)" by @charliermarsh on 2024-03-25 00:31_

---

_Merged by @charliermarsh on 2024-03-25 00:38_

---

_Closed by @charliermarsh on 2024-03-25 00:38_

---

_Comment by @dhruvmanila on 2024-03-25 02:05_

I don't think this is the correct solution as it will not work for nested `for` loops. This is when the `continue`/`break`/`return` is _without_ an `if` condition as there isn't any dedicated counter value in the stack for the `for` loop. For example:

```python
for _, group in itertools.groupby(...):
	use_group(group)
	for _ in range(5):
		use_group(group)  # B031
		continue
	use_group(group)  # B031
```

This PR stops highlighting the second `B031` in the above snippet which is incorrect. The `continue` is only for the `inner` for loop. For `return`, it would be different as it actually exits the loop.

---

_Comment by @charliermarsh on 2024-03-25 02:14_

@dhruvmanila - It seems like we're fixing a common false positive but introducing a less common false negative, so it seems like the right tradeoff, unless you disagree? Feel free to file an issue if so.

---

_Branch deleted on 2024-03-26 14:05_

---

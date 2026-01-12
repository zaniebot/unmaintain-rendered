```yaml
number: 8466
title: Fix multiline lambda expression statement formating
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
assignees: []
merged: true
base: main
head: fix-multiline-statement-lambda
created_at: 2023-11-03T08:03:13Z
updated_at: 2023-11-05T14:35:25Z
url: https://github.com/astral-sh/ruff/pull/8466
synced_at: 2026-01-12T15:55:26Z
```

# Fix multiline lambda expression statement formating

---

_@MichaReiser_

## Summary

This PR fixes a bug in our formatter where a multiline lambda expression statement was formatted over multiple lines without adding parentheses. 

The PR "fixes" the problem by not splitting the lambda parameters if it is not parenthesized

## Test Plan

Added test


---

_Comment by @MichaReiser on 2023-11-03 08:03_

Current dependencies on/for this PR:
* main
  * **PR #8466** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8466" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #8465** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8465" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8466?utm_source=stack-comment).

---

_Label `formatter` added by @MichaReiser on 2023-11-03 08:03_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-11-03 08:03_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/lambda.py`:209 on 2023-11-03 08:17_

This PR does not fix this formatting. It only adds tests for it

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/parameters.rs`:108 on 2023-11-03 08:19_

testing for `self.parentheses == ParameterParentheses::Never` is, unfortunately, not sufficient because it doesn't handle comments:

```python
a = [
	lambda x # comment
		x 
		# comment
	: b
]
```

This uses the `Never` layout but the lambda has comments. The example also shows that testing whether the lambda expression itself is parenthesized isn't sufficient. 



---

_@MichaReiser reviewed on 2023-11-03 08:19_

---

_Comment by @github-actions[bot] on 2023-11-03 09:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh reviewed on 2023-11-03 16:55_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/lambda.py`:209 on 2023-11-03 16:55_

What _does_ this PR fix? Sorry, trying to parse!

---

_@charliermarsh reviewed on 2023-11-03 16:55_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/resources/test/fixtures/ruff/expression/lambda.py`:209 on 2023-11-03 16:55_

Wait sorry, I now understand.

---

_@charliermarsh approved on 2023-11-03 16:55_

---

_Merged by @charliermarsh on 2023-11-05 14:35_

---

_Closed by @charliermarsh on 2023-11-05 14:35_

---

_Branch deleted on 2023-11-05 14:35_

---

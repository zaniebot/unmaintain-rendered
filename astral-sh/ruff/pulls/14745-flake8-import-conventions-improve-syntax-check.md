```yaml
number: 14745
title: "[`flake8-import-conventions`] Improve syntax check for aliases supplied in configuration for `unconventional-import-alias (ICN001)`"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - configuration
assignees: []
merged: true
base: main
head: validate-extend-alias
created_at: 2024-12-03T00:08:39Z
updated_at: 2024-12-03T04:41:55Z
url: https://github.com/astral-sh/ruff/pull/14745
synced_at: 2026-01-12T15:55:48Z
```

# [`flake8-import-conventions`] Improve syntax check for aliases supplied in configuration for `unconventional-import-alias (ICN001)`

---

_@dylwil3_

This PR improves on #14477 by:

- Ensuring user's do not require module aliases which are unassignable Python identifiers
- Validating the linter settings for `lint.flake8-import-conventions.extend-aliases` (whereas previously we only did this for `lint.flake8-import-conventions.aliases`).

Closes #14662


---

_Label `bug` added by @dylwil3 on 2024-12-03 00:08_

---

_Label `configuration` added by @dylwil3 on 2024-12-03 00:08_

---

_Comment by @github-actions[bot] on 2024-12-03 00:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2024-12-03 04:09_

---

_Review comment by @dhruvmanila on `crates/ruff_workspace/src/options.rs`:1421 on 2024-12-03 04:09_

I think we can skip "True", "False" and "None" here as they are already being checked by the `is_identifier` check below:

https://github.com/astral-sh/ruff/blob/5137fcc9c81610f687b6cb45413ef83c2c5eea73/crates/ruff_python_stdlib/src/identifiers.rs#L19-L22

---

_@dylwil3 reviewed on 2024-12-03 04:29_

---

_Review comment by @dylwil3 on `crates/ruff_workspace/src/options.rs`:1421 on 2024-12-03 04:29_

Nice catch! 

I guess that's a somewhat confusing function name because keywords _are_ identifiers, see: https://docs.python.org/3/reference/lexical_analysis.html#keywords

and also:

```console
>>> "None".isidentifier()
True
```

---

_@dhruvmanila reviewed on 2024-12-03 04:35_

---

_Review comment by @dhruvmanila on `crates/ruff_workspace/src/options.rs`:1421 on 2024-12-03 04:35_

Yes, keywords are identifiers but they cannot be used as an identifier. I think the function is used to test the latter.

---

_@dhruvmanila approved on 2024-12-03 04:38_

---

_Merged by @dylwil3 on 2024-12-03 04:41_

---

_Closed by @dylwil3 on 2024-12-03 04:41_

---

_Branch deleted on 2024-12-03 04:41_

---

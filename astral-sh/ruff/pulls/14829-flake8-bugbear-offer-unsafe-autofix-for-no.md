```yaml
number: 14829
title: "[`flake8-bugbear`] Offer unsafe autofix for `no-explicit-stacklevel` (`B028`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - fixes
assignees: []
merged: true
base: main
head: stacklevel-fix
created_at: 2024-12-07T03:50:22Z
updated_at: 2024-12-07T13:27:11Z
url: https://github.com/astral-sh/ruff/pull/14829
synced_at: 2026-01-10T20:42:27Z
```

# [`flake8-bugbear`] Offer unsafe autofix for `no-explicit-stacklevel` (`B028`)

---

_Pull request opened by @dylwil3 on 2024-12-07 03:50_

This PR introduces an unsafe autofix for [no-explicit-stacklevel (B028)](https://docs.astral.sh/ruff/rules/no-explicit-stacklevel/#no-explicit-stacklevel-b028): we add the `stacklevel` argument, set to `2`.

Closes #14805


---

_@charliermarsh reviewed on 2024-12-07 03:52_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/no_explicit_stacklevel.rs`:49 on 2024-12-07 03:52_

I prefer a style like

```
Set `stacklevel=2`
```

Since these show up inline in editors.

---

_@charliermarsh reviewed on 2024-12-07 03:53_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/no_explicit_stacklevel.rs`:69 on 2024-12-07 03:53_

I'm super surprised this method doesn't need to know if this is a keyword or positional argument!

---

_@charliermarsh approved on 2024-12-07 03:53_

---

_Label `fixes` added by @charliermarsh on 2024-12-07 03:54_

---

_Comment by @github-actions[bot] on 2024-12-07 03:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/no_explicit_stacklevel.rs`:69 on 2024-12-07 04:01_

I was also surprised... it seems like it would actually be unsafe to use this method for positional arguments, since it always adds the argument to the end? If that's correct and you'd like me to make a followup PR, let me know - I'd be happy to!

---

_@dylwil3 reviewed on 2024-12-07 04:01_

---

_Merged by @charliermarsh on 2024-12-07 13:24_

---

_Closed by @charliermarsh on 2024-12-07 13:24_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/no_explicit_stacklevel.rs`:69 on 2024-12-07 13:24_

Yeah feel free to take a look!

---

_@charliermarsh reviewed on 2024-12-07 13:24_

---

_Branch deleted on 2024-12-07 13:27_

---

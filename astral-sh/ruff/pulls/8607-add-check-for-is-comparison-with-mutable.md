```yaml
number: 8607
title: Add check for is comparison with mutable initialisers to rule F632
type: pull_request
state: merged
author: jesse1412
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: is-comp-mutable-initializer
created_at: 2023-11-10T19:00:18Z
updated_at: 2023-11-13T13:10:39Z
url: https://github.com/astral-sh/ruff/pull/8607
synced_at: 2026-01-10T23:40:55Z
```

# Add check for is comparison with mutable initialisers to rule F632

---

_Pull request opened by @jesse1412 on 2023-11-10 19:00_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Adds an extra check to F632 to check for any `is` comparisons to a mutable initialisers.
Implements #8589 .

Example:
```Python
named_var = {}
if named_var is {}:  # F632 (fix)
    pass
```
The if condition will always evaluate to False because it checks on identity and it's impossible to take the same identity as a hard coded list/set/dict initializer.

<!-- How was it tested? -->
Multiple test cases were added to ensure the rule works + doesn't flag false positives + the fix works correctly.

---

_Comment by @github-actions[bot] on 2023-11-10 19:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @vinhocent on 2023-11-10 19:21_

Just wondering - what was the original reasoning for not using a `match` statement in `is_constant()` @charliermarsh ?

---

_Label `rule` added by @charliermarsh on 2023-11-10 20:53_

---

_Comment by @zanieb on 2023-11-10 20:57_

We should probably gate this change to only apply when preview mode is enabled per [the versioning policy](https://docs.astral.sh/ruff/versioning/#version-changes) as I think it fits into

> The scope of a stable rule is significantly increased

Note on how to do this in another pull request https://github.com/astral-sh/ruff/pull/8608#issuecomment-1806414406



---

_Comment by @zanieb on 2023-11-10 21:00_

Thanks for contributing :) the rest of the implementation looks good to me. I don't have the answer to your question though.

---

_Comment by @jesse1412 on 2023-11-10 23:23_

I thiiink I've added the preview flag. And thanks for the encouraging words!

---

_@charliermarsh approved on 2023-11-11 00:19_

Great, thank you!

---

_Label `preview` added by @charliermarsh on 2023-11-11 00:20_

---

_Merged by @charliermarsh on 2023-11-11 00:29_

---

_Closed by @charliermarsh on 2023-11-11 00:29_

---

_Branch deleted on 2023-11-13 13:10_

---

```yaml
number: 13119
title: Disable E741 in stub files
type: pull_request
state: merged
author: calumy
labels:
  - preview
assignees: []
merged: true
base: main
head: disable-E741-in-pyi-files
created_at: 2024-08-26T22:52:35Z
updated_at: 2024-08-27T14:56:38Z
url: https://github.com/astral-sh/ruff/pull/13119
synced_at: 2026-01-10T21:38:32Z
```

# Disable E741 in stub files

---

_Pull request opened by @calumy on 2024-08-26 22:52_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

As noted in #10569, E741 should not be active in `.pyi` files as the file author does not control the variable name. Instead, this is deferred to the author of the package for which the stub file is substituting for.

My rust skills are being stretched here. I assume there is a more elegant way of implementing the check `checker.source_type != PySourceType::Stub` in the `ambiguous_variable_name` function so that it is only implemented in one place (similar to `non_unique_enums`); however, I can't get the borrow checker to agree to this.

## Test Plan

Added a new test case were `E741.py` was duplicated and renamed to `E741.pyi` and a snapshot to confirm that no errors were raised for this new case as E741 should be disabled in `.pyi` files. 


---

_Comment by @github-actions[bot] on 2024-08-26 23:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-08-27 13:34_

---

_@AlexWaygood approved on 2024-08-27 13:51_

Thanks @calumy, this is great! I pushed a few small fixups to your PR:
1. I made the change preview-only for now; we can stabilise it in Ruff 0.7. Strictly speaking we could _probably_ get away with doing this in a patch release, since it's a reduction in scope rather than an increase in scope -- but it's still a big _change_ in scope, so I feel much safer doing it in preview only first :-)
2. I added some docs on how it works differently for `.pyi` files
3. I made it a bit more DRY by putting the check for whether it's a stub file in the `ambiguous_variable_name()` function itself, so we don't have to repeat it everywhere.

LMK if you'd like me to explain any of it, very happy to do so :-)

---

_Label `preview` added by @AlexWaygood on 2024-08-27 13:51_

---

_Merged by @AlexWaygood on 2024-08-27 14:02_

---

_Closed by @AlexWaygood on 2024-08-27 14:02_

---

_Comment by @calumy on 2024-08-27 14:56_

> Thanks @calumy, this is great! I pushed a few small fixups to your PR:
> 
> 1. I made the change preview-only for now; we can stabilise it in Ruff 0.7. Strictly speaking we could _probably_ get away with doing this in a patch release, since it's a reduction in scope rather than an increase in scope -- but it's still a big _change_ in scope, so I feel much safer doing it in preview only first :-)
> 2. I added some docs on how it works differently for `.pyi` files
> 3. I made it a bit more DRY by putting the check for whether it's a stub file in the `ambiguous_variable_name()` function itself, so we don't have to repeat it everywhere.
> 
> LMK if you'd like me to explain any of it, very happy to do so :-)

All makes sense to me; thanks for laying it out clearly.

---

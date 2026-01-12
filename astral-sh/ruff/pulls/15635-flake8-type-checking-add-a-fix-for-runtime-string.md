```yaml
number: 15635
title: "[`flake8-type-checking`] Add a fix for `runtime-string-union` (`TC010`)"
type: pull_request
state: open
author: Daverball
labels:
  - rule
  - fixes
assignees: []
draft: true
base: main
head: feat/tc010-fix2
created_at: 2025-01-21T10:59:06Z
updated_at: 2025-06-25T08:38:10Z
url: https://github.com/astral-sh/ruff/pull/15635
synced_at: 2026-01-12T15:55:52Z
```

# [`flake8-type-checking`] Add a fix for `runtime-string-union` (`TC010`)

---

_@Daverball_

This is an alternative implementation for #15222 so we can compare the two approaches.
/cc @MichaReiser 

## Summary

This adds a fix for `TC010` which removes quotes whenever they can be removed safely and otherwise extends the quotes to the entire union.

This also disables `TC010` explicitly in stubs, rather than rely on the execution context, which can still be runtime in stub files for a small set of type expressions.

## Test Plan

`cargo nextest run`


---

_Comment by @github-actions[bot] on 2025-01-21 11:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @Daverball on 2025-01-21 11:07_

The only thing this approach does worse, is that for the same union expression it can emit a fix for removing quotes only to also emit a fix for extending the quotes, so we may want to ensure the fix for extending the quotes gets applied first. I think this may already be the case since we set `parent` to the start of the extended expression, which should sort before the more narrow fix. But I haven't actually checked.

Technically it doesn't matter which order the fixes get applied, since eventually they will stabilize. Doing the extension first is just more efficient, since the other fix will go away in the next iteration.

But other than that this approach seems cleaner, since we can reuse existing helper functions and rely on already parsed forward references.

This will however slightly change the behavior of the rule. We will no longer emit a violation for invalid type expressions, including byte and f-strings. But I think that's actually an improvement. If the type expression is invalid to begin with, we shouldn't really emit any other diagnostics.

---

_Comment by @Daverball on 2025-01-21 12:08_

This seems to have the same issue where some of the fixes that should be marked safe are marked as unsafe. I assume that either the comment range or the expression range is incorrect in this case. It's also possible that the ranges are correct, but the end of the expression range exactly matches the start of a comment range. Which would make it a off by one error in the overlap check.

---

_Comment by @Daverball on 2025-01-21 12:23_

Alright, I think I understand the problem now, `has_comments` will expand the `TextRange` to the end of the line if it ends right before a comment, i.e. it will treat the comment as belonging to that expression. So it seems like it is actually the wrong function to use in order to determine whether an edit's `TextRange` will intersect a comment, instead we should just directly use `ComentRanges::intersects`.

This problem affects a bunch of existing fixes, should we open an issue for this @MichaReiser?

---

_Label `fixes` added by @dylwil3 on 2025-01-23 23:22_

---

_Label `rule` added by @dylwil3 on 2025-01-23 23:22_

---

_Converted to draft by @dhruvmanila on 2025-02-11 09:44_

---

_Comment by @dhruvmanila on 2025-02-11 09:44_

(Marking this as draft to signal that this is an alternate implementation of an existing PR.)

---

```yaml
number: 14467
title: "[ruff-0.8] [`FAST`] Further improve docs for `fast-api-non-annotated-depencency` (`FAST002`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - documentation
assignees: []
merged: true
base: ruff-0.8
head: fast002-preview-again
created_at: 2024-11-19T18:24:45Z
updated_at: 2024-11-20T12:04:02Z
url: https://github.com/astral-sh/ruff/pull/14467
synced_at: 2026-01-12T15:55:47Z
```

# [ruff-0.8] [`FAST`] Further improve docs for `fast-api-non-annotated-depencency` (`FAST002`)

---

_@AlexWaygood_

This PR is stacked on top of #14466.

## Summary

This PR updates the docs for `FAST002` to note that `typing.Annotated` was added in Python 3.9, but that `typing_extensions.Annotated` can be used on older versions of Python.

Alternatively, we could consider reverting the stabilisation of this rule, since not everybody will necessarily be happy to add `typing_extensions` as a dependency, but the autofix assumes that it is available to be imported on Python 3.8 and lower. However, nobody has raised this issue, and Python 3.8 is now end-of-life, so while this isn't ideal I think it _probably_ shouldn't block the rule being stabilised. If you _are_ on an older version of Python and don't want `typing_extensions` as a dependency, you can always just disable the rule.

I don't have a strong opinion here, though; this does feel sort-of borderline to me. Maybe we should revert the stabilisation?

## Test Plan

`cargo test -p ruff_linter`


---

_Label `documentation` added by @AlexWaygood on 2024-11-19 18:24_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-19 18:24_

---

_Added to milestone `v0.8` by @AlexWaygood on 2024-11-19 18:25_

---

_Comment by @github-actions[bot] on 2024-11-19 18:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-11-20 07:59_

Thinking about this some more, I think we should at least mark the autofix as unsafe if the target version is set to Python 3.8 or lower, since the autofix adds imports of typing_extensions on these older Python versions, which the user might not have or want as a dependency. So the fix could easily break working Python code on older Python versions.

---

_@MichaReiser approved on 2024-11-20 08:00_

Let's say we hold back on stabilizing this rule now because of Python 3.8. How would you change the rule or what are the next steps to stabilize this rule in an upcoming release? 

To me it's unclear what that change would be:

* We could only flag the rule for Python 3.9 or newer, but this makes the rule useless for users targeting Python 3.8 that want the rule
* We could introduce an option, but an option is equivalent to toggling the rule
* We could test if a typing module is already imported. I don't think this will be very useful unless it's done globally and not on a per file level, which Ruff doesn't support. 


That's why think we should stabilize the rule. Users that don't want typing rules can disable the rule. 

---

_Comment by @MichaReiser on 2024-11-20 08:00_

And nice find! 

---

_@AlexWaygood reviewed on 2024-11-20 08:04_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/fastapi/mod.rs`:29 on 2024-11-20 08:04_

Memo to self: Should say "use", not "using"

---

_Comment by @AlexWaygood on 2024-11-20 10:44_

> Thinking about this some more, I think we should at least mark the autofix as unsafe if the target version is set to Python 3.8 or lower, since the autofix adds imports of typing_extensions on these older Python versions, which the user might not have or want as a dependency. So the fix could easily break working Python code on older Python versions.

Okay, this isn't an issue, because the fix is already marked as unsafe on all Python versions. However, there's no `## Fix safety` section in the documentation explaining why this is so. I also can't see any explicit discussion of whether the fix should be safe or unsafe in https://github.com/astral-sh/ruff/pull/11579 :/

---

_Comment by @AlexWaygood on 2024-11-20 10:48_

Landing for now; this is an improvement on the status quo, and we can always add more docs on the safety of the fix as a followup

---

_Merged by @AlexWaygood on 2024-11-20 10:48_

---

_Closed by @AlexWaygood on 2024-11-20 10:48_

---

_Branch deleted on 2024-11-20 10:48_

---

_Comment by @AlexWaygood on 2024-11-20 11:09_

I opened https://github.com/astral-sh/ruff/issues/14484

---

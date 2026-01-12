```yaml
number: 11057
title: "[`flake8-pyi`] Allow overloaded `__exit__` and `__aexit__` definitions (`PYI036`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: pyi036-overloads
created_at: 2024-04-20T13:55:09Z
updated_at: 2024-04-24T15:55:08Z
url: https://github.com/astral-sh/ruff/pull/11057
synced_at: 2026-01-12T15:55:34Z
```

# [`flake8-pyi`] Allow overloaded `__exit__` and `__aexit__` definitions (`PYI036`)

---

_@AlexWaygood_

## Summary

Fixes #11044

We should definitely allow overloaded definitions for `__exit__` and `__aexit__`. Nonetheless, _validating_ overloads is pretty tricky without the full machinery of a type checker, so this PR deliberately takes a pretty conservative approach. If there are exactly two overloads, both overloads have exactly three annotated non-self non-keyword-only arguments each, and both overloads have no keyword-only arguments or variadic arguments -- then, for both overloads, we check to see if the overload matches one of two recognised overload variants (permitting small variations such as if one parameter is hinted with `object` or `_typeshed.Unused`). If there's an unexpected number of arguments, however, we just don't check the overload.

## Test plan

`cargo test`.

---

_Label `bug` added by @AlexWaygood on 2024-04-20 13:55_

---

_Label `rule` added by @AlexWaygood on 2024-04-20 13:55_

---

_Comment by @github-actions[bot] on 2024-04-20 14:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Converted to draft by @AlexWaygood on 2024-04-20 14:12_

---

_Comment by @johnslavik on 2024-04-20 15:20_

Oh, good to see it! I've been having an issue with this recently. 👍👍👍

---

_@AlexWaygood reviewed on 2024-04-20 21:24_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/exit_annotations.rs`:147 on 2024-04-20 21:24_

I think the logic here has been wrong for a while. It should have always been `parameters.posonlyargs.iter().chain(parameters.args.iter())`, not `parameters.args.iter().chain(parameters.posonlyargs.iter())`. I think it didn't come up because it the bug lead to false negatives rather than false positives, so nobody noticed!

---

_Marked ready for review by @AlexWaygood on 2024-04-20 21:24_

---

_@charliermarsh reviewed on 2024-04-23 00:37_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/exit_annotations.rs`:310 on 2024-04-23 00:37_

I would just use a normal `Vec` here personally. It's not a very hot path, right? This is _exactly_ if you're defining one of a small set of methods, and it has an overload?

---

_@charliermarsh approved on 2024-04-23 00:42_

I think if it were me making this decision on a desert island, I'd probably just skip overloads since I wouldn't find the complexity-to-value tradeoff to be good enough (we're trading ~400 lines of code for a scenario that seems somewhat rare). But I defer to you as the owner of the rule, and it looks like you have intuition that this is worth supporting.


---

_@AlexWaygood reviewed on 2024-04-24 13:52_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/exit_annotations.rs`:310 on 2024-04-24 13:52_

It shouldn't be very hot, no. I mainly used a `SmallVec` because it was already being used elsewhere in this file, and it _is_ exceedingly unlikely that anybody _would_ use more than two overloads for `__exit__` or `__aexit__`. Is there a disadvantage to using a SmallVec here?

---

_Comment by @AlexWaygood on 2024-04-24 14:37_

> I think if it were me making this decision on a desert island, I'd probably just skip overloads since I wouldn't find the complexity-to-value tradeoff to be good enough

Yeah I know what you mean. My initial thought when I saw the issue was "well, that's going to add extra complexity to the check, and it's a less common case, so we may as well skip it". But (although we rarely bother with it in typeshed, it seems like extra complexity for little gain), Adam's [recent blog post](https://adamj.eu/tech/2021/07/04/python-type-hints-how-to-type-a-context-manager/) _is_ strictly-speaking correct that it's more accurate to use overloads for this case, and that blog seems to be making the rounds at the moment. And if you _are_ going to use overloads, then I think it's really easy to get it subtly wrong by e.g. using `Exception` instead of `BaseException`, the same as if you don't use overloads. So I do think there _is_ value to checking overloads.

This _may_ be partly sunk-cost fallacy on my part though, given that I've gone ahead an implemented it now...

I think I'll add a few more test cases to make sure that every conceivable scenario is covered, then go ahead with this.

---

_Merged by @AlexWaygood on 2024-04-24 15:48_

---

_Closed by @AlexWaygood on 2024-04-24 15:48_

---

_Branch deleted on 2024-04-24 15:48_

---

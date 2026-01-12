```yaml
number: 12563
title: "[`flake8-return`] Exempt cached properties and other property-like decorators from explicit return rule (`RET501`)"
type: pull_request
state: merged
author: epenet
labels:
  - rule
assignees: []
merged: true
base: main
head: 20240729-0822
created_at: 2024-07-29T06:24:39Z
updated_at: 2024-07-30T11:45:49Z
url: https://github.com/astral-sh/ruff/pull/12563
synced_at: 2026-01-12T15:55:41Z
```

# [`flake8-return`] Exempt cached properties and other property-like decorators from explicit return rule (`RET501`)

---

_@epenet_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Follow-up to #12243, in which regular properties were exempted but not cached properties.

Still linked to https://github.com/home-assistant/core/pull/115031

## Test Plan

<!-- How was it tested? -->


---

_Converted to draft by @epenet on 2024-07-29 06:24_

---

_Comment by @codspeed-hq[bot] on 2024-07-29 06:41_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/epenet:20240729-0822)

### Merging #12563 will **not alter performance**

<sub>Comparing <code>epenet:20240729-0822</code> (c2c96bc) with <code>epenet:20240729-0822</code> (f452453)</sub>



### Summary

`✅ 33` untouched benchmarks






---

_Marked ready for review by @epenet on 2024-07-29 07:08_

---

_Comment by @github-actions[bot] on 2024-07-29 07:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-07-30 06:43_

LGTM. Thank you. I let @AlexWaygood merge to get the blessing of someone who actually understands Python ;)

---

_Label `rule` added by @MichaReiser on 2024-07-30 06:43_

---

_@AlexWaygood approved on 2024-07-30 10:32_

LGTM. I added support for other stdlib property-like decorators as well, such as `@abc.abstractproperty`, `@enum.property`, and `@types.DynamicClassAttribute`. I think the same rationale applies for special-casing these decorators as it does for special-casing `@functools.cached_property`.

---

_@epenet reviewed on 2024-07-30 10:34_

---

_Review comment by @epenet on `crates/ruff_linter/resources/test/fixtures/flake8_return/RET501.py`:36 on 2024-07-30 10:34_

Note: single line functions are ignored anyway.
So the print is needed for sensible testing. 

---

_@AlexWaygood reviewed on 2024-07-30 10:35_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:400 on 2024-07-30 10:35_

@epenet -- you had `semantic.resolve_qualified_name(map_callable(&decorator.expression))` here. I think using `map_callable` would be incorrect in this instance. `map_callable()` is a convenience function for when we don't care whether e.g. a function is decorated with `@functools.lru_cache` or `@functools.lru_cache()` (whether the decorator is a call-expression or not is irrelevant). But for all of these `property`-like decorators, you can only use them as `@property`; it's invalid to use them as `@property()`. So for these decorators, we just want to call `resolve_qualified_name` directly on the expression, instead of passing it through `map_callable` first.

---

_@AlexWaygood reviewed on 2024-07-30 10:35_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_return/RET501.py`:36 on 2024-07-30 10:35_

Ah, thank you!

---

_Renamed from "[`flake8-return`] Exempt cached properties from explicit return rule (`RET501`)" to "[`flake8-return`] Exempt cached properties and other property-like decorators from explicit return rule (`RET501`)" by @AlexWaygood on 2024-07-30 10:42_

---

_@epenet reviewed on 2024-07-30 10:43_

---

_Review comment by @epenet on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:400 on 2024-07-30 10:43_

I copied it from elsewhere in the code base.
I wonder if this applies there as well...
And would it make sense to move this function to a shared location?

---

_@AlexWaygood reviewed on 2024-07-30 10:48_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:400 on 2024-07-30 10:48_

> I copied it from elsewhere in the code base.
> I wonder if this applies there as well...

It's hard to say in general. For a lot of decorators (like `@lru_cache`), you really don't care about whether it's been decorated with `@lru_cache` or `@lru_cache()`. So while there may be other places in the codebase where it's being used incorrectly, I'm pretty confident they're not _all_ incorrect ;-)

> And would it make sense to move this function to a shared location?

Possibly...

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:400 on 2024-07-30 10:53_

Good that you asked. It seems we already have a general-purpose function for this:

https://github.com/astral-sh/ruff/blob/aaa56eb0bd7f2740c83587d03198bc12c8e69648/crates/ruff_python_semantic/src/analyze/visibility.rs#L63-L83

We should just use that for now. I'll apply some fixes to it in a followup PR.

---

_@AlexWaygood reviewed on 2024-07-30 10:54_

---

_Merged by @AlexWaygood on 2024-07-30 11:06_

---

_Closed by @AlexWaygood on 2024-07-30 11:06_

---

_Branch deleted on 2024-07-30 11:12_

---

_@epenet reviewed on 2024-07-30 11:31_

---

_Review comment by @epenet on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:400 on 2024-07-30 11:31_

For reference, I wonder if it might make sense to look at `rules/pydoclint/rules/check_docstring.rs` also:
https://github.com/astral-sh/ruff/blob/459c85ba273ae347402b1759cacd1e7740cef121/crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs#L442-L461

---

_@AlexWaygood reviewed on 2024-07-30 11:45_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_return/rules/function.rs`:400 on 2024-07-30 11:45_

nice, thanks

---

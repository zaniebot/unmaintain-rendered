```yaml
number: 14749
title: "[`flake8-type-checking`] Expands TC006 docs to better explain itself"
type: pull_request
state: merged
author: Daverball
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/tc006
created_at: 2024-12-03T07:42:14Z
updated_at: 2024-12-04T18:46:24Z
url: https://github.com/astral-sh/ruff/pull/14749
synced_at: 2026-01-12T15:55:48Z
```

# [`flake8-type-checking`] Expands TC006 docs to better explain itself

---

_@Daverball_

Closes: #14676

I think the consensus generally was to keep the rule as-is, but expand the docs.

## Summary

Expands the docs for TC006 with an explanation for why the type expression is always quoted, including mention of another potential benefit to this style.

---

_Comment by @github-actions[bot] on 2024-12-03 07:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Converted to draft by @Daverball on 2024-12-03 10:02_

---

_Comment by @Daverball on 2024-12-03 10:03_

Let's see where the discussion goes first.

---

_Marked ready for review by @Daverball on 2024-12-04 07:52_

---

_Comment by @Daverball on 2024-12-04 07:54_

@AlexWaygood Is that explanation satisfactory? It might make sense to add a `quote-casts` setting in the same PR, so we can reference an alternative way to get just the unnecessary import overhead out of casts.

---

_@AlexWaygood reviewed on 2024-12-04 11:24_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_cast_value.rs`:18 on 2024-12-04 11:24_

From our conversation on the issue, my conclusion is that this rule should be thought of as _primarily_ about enforcing stylistic consistency, since there will be other rules added in the future that will be better suited to deal with the performance aspects. I think the first sentence of these docs should reflect that, but the first sentence here still makes it sound like it's primarily focussed on performance :-)

---

_@Daverball reviewed on 2024-12-04 12:07_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_cast_value.rs`:18 on 2024-12-04 12:07_

I'm not sure how to phrase this any differently, you're welcome to try.

---

_@AlexWaygood reviewed on 2024-12-04 12:21_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_cast_value.rs`:18 on 2024-12-04 12:21_

How about this?

```diff
 /// ## What it does
-/// Checks for an unquoted type expression in `typing.cast()` calls.
+/// Checks for unquoted type expressions in `typing.cast()` calls.
 ///
 /// ## Why is this bad?
-/// `typing.cast()` does not do anything at runtime, so the time spent
-/// on evaluating the potentially complex type expression is wasted.
-/// But it's also bad to be inconsistent, so this rule always quotes
-/// the type expression even if its contribution to import/evaluation
-/// time is negligible, as a matter of style.
+/// This rule helps enforce a consistent style across your codebase.
+///
+/// It's often necessary to quote the first argument passed to `cast()`,
+/// as type expressions can involve forward references, or references
+/// to symbols which are only imported in `typing.TYPE_CHECKING` blocks.
+/// This can lead to a visual inconsistency across different `cast()` calls,
+/// where some type expressions are quoted but others are not. By enabling
+/// this rule, you ensure that all type expressions passed to `cast()` are
+/// quoted, enforcing stylistic consistency across all of your `cast()` calls.
```

---

_@Daverball reviewed on 2024-12-04 12:46_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_cast_value.rs`:18 on 2024-12-04 12:46_

That seems fine to me. Although I'd eventually like to cross-reference performance-focused alternatives like a `quote-casts` setting for TC001-003 and/or a new rule to quote type expressions that create `typing._GenericAlias` instances. So if we completely stop explicitly mentioning performance that might come a bit out of left-field.

At least as long as we make the quoting sub-expressions part of TC001-003 and this potentially new rule opt-in. If it's always on (which should technically always be safe) then I don't think that's as necessary. But given your readability pushback I assume there are some people which would prefer to never quote their casts, regardless of any potential benefits.

So in that world I would like to give people enough information in order to make an informed decision about whether TC006, enabling `quote-casts` or doing neither would make more sense for them.

---

_@AlexWaygood reviewed on 2024-12-04 12:59_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_cast_value.rs`:18 on 2024-12-04 12:59_

We could add a sentence to the end about performance?

```diff
 /// ## What it does
-/// Checks for an unquoted type expression in `typing.cast()` calls.
+/// Checks for unquoted type expressions in `typing.cast()` calls.
 ///
 /// ## Why is this bad?
-/// `typing.cast()` does not do anything at runtime, so the time spent
-/// on evaluating the potentially complex type expression is wasted.
-/// But it's also bad to be inconsistent, so this rule always quotes
-/// the type expression even if its contribution to import/evaluation
-/// time is negligible, as a matter of style.
+/// This rule helps enforce a consistent style across your codebase.
+///
+/// It's often necessary to quote the first argument passed to `cast()`,
+/// as type expressions can involve forward references, or references
+/// to symbols which are only imported in `typing.TYPE_CHECKING` blocks.
+/// This can lead to a visual inconsistency across different `cast()` calls,
+/// where some type expressions are quoted but others are not. By enabling
+/// this rule, you ensure that all type expressions passed to `cast()` are
+/// quoted, enforcing stylistic consistency across all of your `cast()` calls.
+///
+/// In some cases where `cast()` is used in a hot loop, this rule may also
+/// help avoid overhead from repeatedly evaluating complex type expressions at
+/// runtime.
```

You don't have to use my suggested rewrite exactly. But from the discussion on the issue, I do think the emphasis should be on style/consistency, rather than performance.

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_cast_value.rs`:18 on 2024-12-04 13:06_

I suppose we could lead into that with something like the following
```
/// Although quoting the type expression in `typing.cast` calls
/// will ensure that the runtime overhead is as small as possible
/// there are alternative rules and settings which will get you
/// most of the way there, while keeping quoted expressions to
/// a minimum.
```
So your text seems good enough for now.

---

_@Daverball reviewed on 2024-12-04 13:06_

---

_@AlexWaygood approved on 2024-12-04 13:10_

Thank you! Really appreciate it

---

_Merged by @AlexWaygood on 2024-12-04 13:16_

---

_Closed by @AlexWaygood on 2024-12-04 13:16_

---

_Branch deleted on 2024-12-04 13:17_

---

_Label `documentation` added by @AlexWaygood on 2024-12-04 18:46_

---

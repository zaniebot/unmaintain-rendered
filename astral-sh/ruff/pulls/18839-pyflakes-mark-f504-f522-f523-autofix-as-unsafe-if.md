```yaml
number: 18839
title: "[`pyflakes`] Mark `F504`/`F522`/`F523` autofix as unsafe if there's a call with side effect"
type: pull_request
state: merged
author: LaBatata101
labels:
  - fixes
assignees: []
merged: true
base: main
head: fix-F504
created_at: 2025-06-20T23:04:18Z
updated_at: 2025-06-26T08:54:11Z
url: https://github.com/astral-sh/ruff/pull/18839
synced_at: 2026-01-12T15:56:26Z
```

# [`pyflakes`] Mark `F504`/`F522`/`F523` autofix as unsafe if there's a call with side effect

---

_@LaBatata101_



<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
Fixes #18806
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Add regression test
<!-- How was it tested? -->


---

_@dscorbett reviewed on 2025-06-20 23:17_

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/pyflakes/rules/strings.rs`:948 on 2025-06-20 23:17_

Using `contains_effect`  would be more reliable and would catch other kinds of expression with side effects.

---

_Comment by @github-actions[bot] on 2025-06-20 23:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@LaBatata101 reviewed on 2025-06-21 13:30_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/pyflakes/rules/strings.rs`:948 on 2025-06-21 13:30_

I think `contains_effect` won't work here because it's mainly used to check for python builtin calls.

---

_@MichaReiser reviewed on 2025-06-23 09:21_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/strings.rs`:948 on 2025-06-23 09:21_

I think `contains_effect` would be preferred here and is also much more accurate than checking for `(` (which e.g. gives incorrect results if you have a parenthesized expression)

---

_@LaBatata101 reviewed on 2025-06-24 14:41_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/pyflakes/rules/strings.rs`:948 on 2025-06-24 14:41_

> I think `contains_effect` would be preferred here and is also much more accurate than checking for `(` (which e.g. gives incorrect results if you have a parenthesized expression)

What should I do about the `is_builtin` parameter from `contain_effect`, if I understood correctly we need to check for any function call and not just builtin calls, right?

---

_@MichaReiser reviewed on 2025-06-24 14:48_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/strings.rs`:948 on 2025-06-24 14:48_

You can call it like this

```
if !contains_effect(target, |id| checker.semantic().has_builtin_binding(id)) {
```

`has_builtin` is used to find some builtins that are known to have no side effect. Removing calls without a side effect is fine because they have no effect.

---

_Review requested from @MichaReiser by @LaBatata101 on 2025-06-24 19:32_

---

_Renamed from "[`pyflakes`] Mark `F504`/`F522`/`F523` autofix as unsafe if there's a call expression in the format part" to "[`pyflakes`] Mark `F504`/`F522`/`F523` autofix as unsafe if there's a call with side effect" by @LaBatata101 on 2025-06-24 19:33_

---

_Review request for @MichaReiser removed by @MichaReiser on 2025-06-25 06:38_

---

_@MichaReiser reviewed on 2025-06-25 06:41_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/strings.rs`:145 on 2025-06-25 06:41_

```suggestion
/// `printf`-style format string because removing such a call could change the runtime behavior.
```

---

_@MichaReiser reviewed on 2025-06-25 06:42_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/strings.rs`:144 on 2025-06-25 06:42_

```suggestion
/// This rule's fix is marked as unsafe if there's a call expression with potential side effects in the
```

---

_@MichaReiser reviewed on 2025-06-25 06:54_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/strings.rs`:810 on 2025-06-25 06:54_

I think this is overly strict. We should only mark the fix as `Unsafe` if any of the arguments to remove have a side effect (and not any argument)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/strings.rs`:887 on 2025-06-25 06:55_

Same here, we should only make this fix unsafe if any removed argument has a side effect. It probably makes sense to add a test showing that the fix doesn't get marked as unsafe if any other (still used) argument has side effects.

---

_@MichaReiser reviewed on 2025-06-25 06:55_

---

_@LaBatata101 reviewed on 2025-06-25 14:13_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/pyflakes/rules/strings.rs`:810 on 2025-06-25 14:13_

> We should only mark the fix as Unsafe if any of the arguments to remove have a side effect (and not any argument)

This phrasing here made me a little confused. Isn't `.any` already doing that?

---

_@MichaReiser reviewed on 2025-06-25 16:39_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/strings.rs`:810 on 2025-06-25 16:39_

It does. But we currently iterate over all arguments. What we should do instead is only look at the arguments that the fix is about to remove.

---

_@LaBatata101 reviewed on 2025-06-25 16:53_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/pyflakes/rules/strings.rs`:810 on 2025-06-25 16:53_

Thanks for the clarification!

---

_Review requested from @MichaReiser by @LaBatata101 on 2025-06-25 22:12_

---

_Label `fixes` added by @MichaReiser on 2025-06-26 08:31_

---

_@MichaReiser approved on 2025-06-26 08:45_

Thank you

---

_Merged by @MichaReiser on 2025-06-26 08:48_

---

_Closed by @MichaReiser on 2025-06-26 08:48_

---

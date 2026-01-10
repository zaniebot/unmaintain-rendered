```yaml
number: 18828
title: "Use the `LinterSettings` on `LintContext` instead of `Checker`"
type: issue
state: closed
author: ntBre
labels:
  - internal
  - help wanted
assignees: []
created_at: 2025-06-20T18:10:22Z
updated_at: 2025-06-23T09:06:45Z
url: https://github.com/astral-sh/ruff/issues/18828
synced_at: 2026-01-10T11:09:58Z
```

# Use the `LinterSettings` on `LintContext` instead of `Checker`

---

_Issue opened by @ntBre on 2025-06-20 18:10_

There is currently a `&'a LinterSettings` field on both `Checker` and `LintContext`, which is redundant because `Checker` also holds a `LintContext`. To remove the redundancy, we should replace uses of `Checker::settings` with `Checker::context.settings`, or, more likely, a `Checker::settings` method since the `context` field is private.

This is a conceptually simple refactor, but when I started it briefly, I ran into some lifetime issues that might make this fairly tedious. For the most part, the compiler seemed to want me to add some explicit lifetimes to `Checker` in more places (e.g. `&'a Checker<'a>` instead of just `&Checker`), but it may also help to add a whole new lifetime parameter to `Checker` so that its `context` field can be `&'a LintContext<'b>`. 

Ref. https://github.com/astral-sh/ruff/pull/18801#discussion_r2158266779


---

_Label `internal` added by @ntBre on 2025-06-20 18:10_

---

_Label `help wanted` added by @ntBre on 2025-06-20 18:10_

---

_Comment by @LaBatata101 on 2025-06-20 21:18_

I can work on this one

---

_Assigned to @LaBatata101 by @AlexWaygood on 2025-06-20 21:37_

---

_Comment by @LaBatata101 on 2025-06-20 22:24_

I've followed your suggestion of adding a new lifetime to `Checker`, but I'm getting this lifetime error that I don't know how to solve. 
```
error: lifetime may not live long enough
    --> crates/ruff_linter/src/checkers/ast/mod.rs:2067:9
     |
760  | impl<'a> Visitor<'a> for Checker<'a, '_> {
     |      -- lifetime `'a` defined here
...
2058 |     fn visit_parameters(&mut self, parameters: &'a Parameters) {
     |                         - let's call the lifetime of this reference `'1`
...
2067 |         analyze::parameters(parameters, self);
     |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ argument requires that `'1` must outlive `'a`
     |
     = note: requirement occurs because of the type `Checker<'_, '_>`, which makes the generic argument `'_` invariant
     = note: the struct `Checker<'a, 'b>` is invariant over the parameter `'a`
     = help: see <https://doc.rust-lang.org/nomicon/subtyping.html> for more information about variance

For more information about this error, try `rustc --explain E0107`.
```
It happens here:
https://github.com/LaBatata101/ruff/blob/530db0d0e826c4e9de998738bd263b895549a6f0/crates/ruff_linter/src/checkers/ast/mod.rs#L2066-L2067

The error shows up after I add the lifetime annotation to these two functions:
https://github.com/LaBatata101/ruff/blob/530db0d0e826c4e9de998738bd263b895549a6f0/crates/ruff_linter/src/rules/flake8_bugbear/rules/function_call_in_argument_default.rs#L132-L135

https://github.com/LaBatata101/ruff/blob/530db0d0e826c4e9de998738bd263b895549a6f0/crates/ruff_linter/src/checkers/ast/analyze/parameters.rs#L8

I've also tested without adding the new lifetime to `Checker` and I get this error too.

---

_Comment by @ntBre on 2025-06-21 01:31_

Could you open a draft PR? I usually have to play with these lifetime errors to figure them out 😅 

---

_Comment by @LaBatata101 on 2025-06-21 02:21_

> Could you open a draft PR? I usually have to play with these lifetime errors to figure them out 😅

Here it is https://github.com/astral-sh/ruff/pull/18845

---

_Closed by @MichaReiser on 2025-06-23 09:06_

---

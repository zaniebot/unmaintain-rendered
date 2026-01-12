```yaml
number: 14668
title: "red-knot: support narrowing for bool(E)"
type: pull_request
state: merged
author: connorskees
labels:
  - ty
assignees: []
merged: true
base: main
head: narrow-bool
created_at: 2024-11-29T06:01:33Z
updated_at: 2024-12-03T03:06:37Z
url: https://github.com/astral-sh/ruff/pull/14668
synced_at: 2026-01-12T15:55:48Z
```

# red-knot: support narrowing for bool(E)

---

_@connorskees_

Resolves https://github.com/astral-sh/ruff/issues/14547 by delegating narrowing to `E` for `bool(E)` where `E` is some expression. 

This change does not include other builtin class constructors which should also work in this position, like `int(..)` or `float(..)`, as the original issue does not mention these. It should be easy enough to add checks for these as well if we want to.

I don't see a lot of markdown tests for malformed input, maybe there's a better place for the no args and too many args cases to go? 

I did see after the fact that it looks like this task was intended for a new hire.. my apologies. I got here from https://github.com/astral-sh/ruff/issues/13694, which is marked help-wanted.

---

_Review requested from @carljm by @connorskees on 2024-11-29 06:01_

---

_Review requested from @MichaReiser by @connorskees on 2024-11-29 06:01_

---

_Review requested from @AlexWaygood by @connorskees on 2024-11-29 06:01_

---

_Review requested from @sharkdp by @connorskees on 2024-11-29 06:01_

---

_Comment by @github-actions[bot] on 2024-11-29 06:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @AlexWaygood on 2024-11-29 07:40_

---

_@AlexWaygood approved on 2024-11-29 12:36_

---

_@sharkdp reviewed on 2024-11-29 13:20_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals/boolean.md`:309 on 2024-11-29 13:20_

I think this test does not (yet) do what you intended. We do not have narrowing support for `if x: …`, so even if you would get rid of the argument length check, we wouldn't see any narrowing here. This could be fixed by using `bool(x is not None, 5)` instead.

On the other hand, the test does not go far enough, because you currently *do* perform narrowing for illegal `bool` calls like `bool(x is not None, y=5)` with a keyword argument. I think we would eventually issue a diagnostic for this call (in call signature checking, not here), but we should probably not narrow in this case.

---

_@connorskees reviewed on 2024-11-29 18:40_

---

_Review comment by @connorskees on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals/boolean.md`:309 on 2024-11-29 18:40_

oh great catch. i've updated the tests to properly fail without this change applied, and added one for kwargs

---

_@sharkdp reviewed on 2024-12-02 08:01_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals/boolean.md`:284 on 2024-12-02 08:01_

The tests here are not really related to the other tests in this file, which are concerned with more intricate narrowing paths involving Boolean expressions. Can we please move this section to a new file, maybe `mdtest/bool-call.md`?

```suggestion
# Narrowing for `bool(..)` checks
```

---

_@sharkdp reviewed on 2024-12-02 08:08_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals/boolean.md`:304 on 2024-12-02 08:08_

`bool()` is `False`, which makes the body of this unreachable. Existing type checkers do not issue diagnostics in unreachable code blocks (i.e. would not yield any `reveal_type` results here).

Maybe we could change it to `if not bool()`, but also, I'm not really sure if this test adds a lot of value. Like what would be a reasonable scenario (code change) where this would fail? So maybe we can just remove it.

---

_@sharkdp reviewed on 2024-12-02 08:14_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/narrow.rs`:388 on 2024-12-02 08:14_

This is neither the type of `expr_call`, nor the type of `expression`, so the name is slightly confusing. Maybe rename it to
```suggestion
        let callable_ty = inference.expression_ty(expr_call.func.scoped_expression_id(self.db, scope));
```

---

_@connorskees reviewed on 2024-12-02 16:17_

---

_Review comment by @connorskees on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals/boolean.md`:304 on 2024-12-02 16:17_

Mostly thinking about a scenario in which we assume `bool` is called with at least one argument, and do something like `&expr_call.arguments.args[0]` without checking that `.len() >= 1`

---

_@connorskees reviewed on 2024-12-02 16:29_

---

_Review comment by @connorskees on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals/boolean.md`:304 on 2024-12-02 16:29_

But happy to remove if we think it's redundant or not useful code coverage! 

---

_Comment by @carljm on 2024-12-02 18:40_

Just a note for future: this issue was assigned to me, and not marked "help wanted", because I created it specifically as a ramp-up task for someone joining the Astral team this week :) It's totally fine that you took it on (thanks for the contribution!), but in future it's best to check if an issue is assigned to someone before starting work on it, and if so, comment on the issue to check if you can take it over without duplicating someone's work-in-progress.

---

_@sharkdp reviewed on 2024-12-02 19:50_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals/boolean.md`:304 on 2024-12-02 19:50_

> Mostly thinking about a scenario in which we assume `bool` is called with at least one argument, and do something like `&expr_call.arguments.args[0]` without checking that `.len() >= 1`

That leads to a panic in the Rust code, but it would not lead to a test failure here (even if that panic would not show up for some reason).

Anyway, I'm okay with the `not bool()` test now. Thanks for the update.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/bool-call.md`:30 on 2024-12-02 20:54_

This isn't critical, but it just means one less thing to think about in the PR that adds call checking (because it will already be marked that this should be a diagnostic):
```suggestion
if bool(x is not None, 5):  # TODO diagnostic
    reveal_type(x)  # revealed: Literal[1] | None

# invalid invocation, too many kwargs
reveal_type(x)  # revealed: Literal[1] | None
if bool(x is not None, y=5):  # TODO diagnostic
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:431 on 2024-12-02 21:02_

This match statement becomes a bit odd, in that we match on a set of conditions specific to constraint functions like `isinstance`, and then check a totally different case in the catch-all arm.

I think it would be clearer if we rework this so that we just match on `callable_ty` itself, and make the first arm match on `Type::FunctionLiteral`, with the "is known" and "is a constraint function" checks moved into the `if` guard of that arm. Then this branch can, in parallel fashion, match on `Type::ClassLiteral`, etc.

---

_@carljm reviewed on 2024-12-02 21:03_

Looks good, thank you! I think we can make the `match` statement a bit clearer / more parallel here.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:429 on 2024-12-03 02:52_

if we capture the `ClassLiteralType` here, we can avoid the `callable_ty.into_class_literal()` below, and the need for handling `Option` from it. We already know we have a class literal.

---

_@carljm reviewed on 2024-12-03 02:52_

---

_@carljm approved on 2024-12-03 03:04_

Looks great, thank you! Merging.

---

_Merged by @carljm on 2024-12-03 03:04_

---

_Closed by @carljm on 2024-12-03 03:04_

---

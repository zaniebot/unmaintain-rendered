```yaml
number: 17567
title: "[red-knot] Update `==` and `!=` narrowing"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: unsound-eq-narrowing
created_at: 2025-04-22T22:31:11Z
updated_at: 2025-04-25T20:55:06Z
url: https://github.com/astral-sh/ruff/pull/17567
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Update `==` and `!=` narrowing

---

_Pull request opened by @MatthewMckee4 on 2025-04-22 22:31_

## Summary

Historically we have avoided narrowing on `==` tests because in many cases it's unsound, since subclasses of a type could compare equal to who-knows-what. But there are a lot of types (literals and unions of them, as well as some known instances like `None` -- single-valued types) whose `__eq__` behavior we know, and which we can safely narrow away based on equality comparisons.

This PR implements equality narrowing in the cases where it is sound. The most elegant way to do this (and the way that is most in-line with our approach up until now) would be to introduce new Type variants `NeverEqualTo[...]` and `AlwaysEqualTo[...]`, and then implement all type relations for those variants, narrow by intersection, and let union and intersection simplification sort it all out. This is analogous to our existing handling for `AlwaysFalse` and `AlwaysTrue`.

But I'm reluctant to add new `Type` variants for this, mostly because they could end up un-simplified in some types and make types even more complex. So let's try this approach, where we handle more of the narrowing logic as a special case.

## Test Plan

Updated and added tests.


---

_Comment by @github-actions[bot] on 2025-04-22 22:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dragonchain (https://github.com/dragonchain/dragonchain)
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/dragonchain/dragonchain/lib/dto/eth.py:98:13: Operator `-=` is unsupported between objects of type `None` and `Literal[60]`
- Found 341 diagnostics
+ Found 340 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/ci_visibility/test_efd.py:80:42: Argument to this function is incorrect: Expected `int`, found `int | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/ci_visibility/test_efd.py:81:43: Argument to this function is incorrect: Expected `int`, found `int | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/ci_visibility/test_efd.py:167:43: Argument to this function is incorrect: Expected `int`, found `int | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/ci_visibility/test_atr.py:97:47: Argument to this function is incorrect: Expected `int`, found `int | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/ci_visibility/test_atr.py:187:39: Argument to this function is incorrect: Expected `int`, found `int | None`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/appsec/_iast/taint_sinks/ast_taint.py:40:54: Operator `+` is unsupported between objects of type `Literal["."] | @Todo(Intersection meta-type)` and `Any | None`
- Found 6920 diagnostics
+ Found 6914 diagnostics

```
</details>


---

_Label `red-knot` added by @AlexWaygood on 2025-04-22 22:58_

---

_Renamed from "[red-knot] Unsound narrowing of == and !=" to "[red-knot] Update `==` and `!=` narrowing" by @MatthewMckee4 on 2025-04-22 23:20_

---

_Marked ready for review by @MatthewMckee4 on 2025-04-22 23:21_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-04-22 23:21_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-04-22 23:21_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-04-22 23:21_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-04-22 23:21_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:404 on 2025-04-22 23:23_

Both of these special cases probably should just go inside `is_union_of_single_valued`, since both `bool` and `LiteralString` are effectively a union of single-valued types. (`LiteralString` is the odd case, since it's an infinite union -- I think it should still work to consider it a union of single-valued, but if that causes breakage somewhere, maybe not.)

If we do that, then we probably don't need a new `Type` method, this would reduce to simple `is_single_valued || is_union_of_single_valued`, which seems reasonable

---

_@carljm approved on 2025-04-22 23:26_

This looks great! I think it remains sound and gives us much better behavior in some common cases. Thank you!!

---

_@MatthewMckee4 reviewed on 2025-04-22 23:32_

---

_Review comment by @MatthewMckee4 on `crates/red_knot_python_semantic/src/types/narrow.rs`:404 on 2025-04-22 23:32_

Sounds good, although, to note one of the mypy primer changes that are no longer there, this will not cover `Any | None` which seemed like a good change.

I also think in general we want `is_positively_narrowable || is_union_of_positively_narrowable`

and in this case `None` is `is_positively_narrowable` and `Any` could be `is_positively_narrowable`?

---

_@carljm reviewed on 2025-04-22 23:51_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:404 on 2025-04-22 23:51_

Good points, this led to some further discussion in Discord, we probably want to try updating the PR so we do handle the `Any | None` case. (I still think it's true that at least `bool` could be considered always a `is_union_of_single_valued`.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2184 on 2025-04-23 00:36_

I think we have a `map` method on `UnionType` that could be used here?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2174 on 2025-04-23 00:39_

This comment and function name are missing the context that this method is not really generalizable to any kind of "positive narrowing" -- the condition on single-valued-ness makes it specifically relevant _only_ to equality narrowing.

For that reason I also don't think it really makes sense to have it as a method on `Type` -- it's just too narrowly-purposed. I would probably make it a free function, possibly even nested inside `evaluate_expr_eq`, which takes an lhs and a rhs type.

---

_@carljm reviewed on 2025-04-23 02:43_

Ok I took a stab at this one myself to make sure I wasn't leading you down an impossible path. And it turned out this can work, though it does require some extra fiddling with `bool` and `LiteralString` to get the right behavior in all cases.

---

_Comment by @carljm on 2025-04-23 04:15_

Would like a quick review on this from one of the other listed reviewers, since @MatthewMckee4 and I co-authored it.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/narrow.rs`:441 on 2025-04-23 07:13_

If I understand correctly, this means that we can't narrow to `Literal["a"]` here, even though it looks like we could:
```py
def _(lhs: LiteralString | None):
    if lhs == "a":
        reveal_type(lhs)  # Could be `Literal["a"]`
```

---

_@sharkdp approved on 2025-04-23 07:17_

This looks great — thank you.

I would find the code a bit easier to understand if we replaced all the `!x.is_subtype_of(y)` checks with `x.is_disjoint_from(y)`, but that's just a matter of preference, I guess.

---

_@AlexWaygood reviewed on 2025-04-23 13:09_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:441 on 2025-04-23 13:09_

and similarly for this:

```py
def _(lhs: LiteralString | None):
    if lhs == None:
        reveal_type(lhs)  # on this PR: `LiteralString | None`
                          # but it could be `None`: we know no inhabitants
                          # of `LiteralString` compare equal to `None`
```

---

_Comment by @AlexWaygood on 2025-04-23 13:20_

I have nothing to add on top of what @sharkdp said! This looks like a great improvement, but the thing with `LiteralString | None` seems like it's still liable to cause confusion. We could at least note it down in an issue as something we'd like to improve in the future.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-23 13:20_

---

_Comment by @carljm on 2025-04-23 17:35_

Thanks for the reviews! I'll see whether we can do something about the `LiteralString | None` case with reasonable complexity.

---

_Comment by @carljm on 2025-04-24 00:43_

Found a bug in the previous implementation around boolean equality with `1` and `0`; that plus the `LiteralString` union issue led to some significant implementation changes, and a number of new tests. So re-requesting review.

---

_Review requested from @sharkdp by @carljm on 2025-04-24 00:43_

---

_Review requested from @AlexWaygood by @carljm on 2025-04-24 12:02_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:438 on 2025-04-24 12:06_

```suggestion
                    (Type::BooleanLiteral(b), Type::IntLiteral(i))
                    | (Type::IntLiteral(i), Type::BooleanLiteral(b)) => i64::from(b) == i,
```

---

_@AlexWaygood approved on 2025-04-24 12:09_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:527 on 2025-04-24 14:56_

```suggestion
            (_, Type::BooleanLiteral(b)) => Some(
                UnionType::from_elements(self.db, [rhs_ty, Type::IntLiteral(i64::from(b))])
                    .negate(self.db),
            ),
```

---

_Merged by @carljm on 2025-04-24 14:56_

---

_Closed by @carljm on 2025-04-24 14:56_

---

_@AlexWaygood approved on 2025-04-24 14:56_

---

_@carljm reviewed on 2025-04-24 14:58_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:527 on 2025-04-24 14:58_

Whoops sorry should have waited for your review. Will put this change up as a separate PR. It's always easier to spell it out the long way when trying to work through the semantics initially...

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:527 on 2025-04-24 15:00_

Eh, no need to apologise, it had been approved!

---

_@AlexWaygood reviewed on 2025-04-24 15:00_

---

_@carljm reviewed on 2025-04-24 15:09_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:527 on 2025-04-24 15:09_

https://github.com/astral-sh/ruff/pull/17610

---

_Branch deleted on 2025-04-25 20:55_

---

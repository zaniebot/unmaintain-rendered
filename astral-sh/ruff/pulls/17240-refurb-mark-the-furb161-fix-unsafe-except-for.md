```yaml
number: 17240
title: "[`refurb`] Mark the `FURB161` fix unsafe except for integers and booleans"
type: pull_request
state: merged
author: VascoSch92
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: unsafe-fixes-FURB161
created_at: 2025-04-06T19:14:42Z
updated_at: 2025-04-18T18:00:56Z
url: https://github.com/astral-sh/ruff/pull/17240
synced_at: 2026-01-12T15:56:01Z
```

# [`refurb`] Mark the `FURB161` fix unsafe except for integers and booleans

---

_@VascoSch92_

The PR fixes #16457 .

Specifically, `FURB161` is marked safe, but the rule generates safe fixes only in specific cases. Therefore, we attempt to mark the fix as unsafe when we are not in one of these cases. 

For instances, the fix is marked as aunsafe just in case of strings (as pointed out in the issue). Let me know if I should change something.
 


---

_Comment by @github-actions[bot] on 2025-04-06 19:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-04-06 23:37_

---

_Label `fixes` added by @ntBre on 2025-04-06 23:37_

---

_Assigned to @ntBre by @ntBre on 2025-04-06 23:37_

---

_@MichaReiser reviewed on 2025-04-07 06:40_

Thanks for looking into this. Could you update the rule's documentation with a `## Fif safety` header and explain when the fix is safe (or isn't)? This will also make reviewing easier

---

_Comment by @dscorbett on 2025-04-07 12:36_

This PR doesn’t fully fix #16457. The point of that issue is that FURB161 should be unsafe by default, with a few exceptions. This PR keeps it safe by default, the only exception being string literals.

---

_Review requested from @MichaReiser by @VascoSch92 on 2025-04-07 19:24_

---

_Comment by @VascoSch92 on 2025-04-07 19:25_

> This PR doesn’t fully fix #16457. The point of that issue is that FURB161 should be unsafe by default, with a few exceptions. This PR keeps it safe by default, the only exception being string literals.

Thanks for the explanation. I approved the modification to make the rule unsafe if we can not determine if the argument of the `bin` method is of type `int` or `bool`

---

_Comment by @dscorbett on 2025-04-07 19:43_

Using `ResolvedPythonType::from` would help catch more expressions than literals, like `-1` or `1 + 2`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/bit_count.rs`:33 on 2025-04-10 20:23_

We also want to say explicitly when it is safe, since it's not always unsafe. Maybe something like this?

```suggestion
/// ## Fix safety
/// This rule's fix is marked as unsafe unless the argument to `bin` can be inferred as
/// an instance of a type that implements the `__index__` and `bit_count` methods.
///
```

I think we could also possibly drop the mention of `__index__` since we're always just calling `bit_count` right? Based on my reading of the `__index__` [docs](https://docs.python.org/3/reference/datamodel.html#object.__index__), it's actually a requirement for calling `bin` itself rather than a requirement to call `bit_count` as seems more relevant here.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/bit_count.rs`:125 on 2025-04-10 20:35_

I think instead of returning a second `bool` here, you can just return an `Applicability` and use that with `Fix::applicable_edit` instead of branching on `fix_is_safe` down below.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/bit_count.rs`:169 on 2025-04-10 20:36_

I can't highlight the line directly, but I think `Expr::BooleanLiteral` should also allow a safe edit, at least that's what it sounds like from the issue.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB161_FURB161.py.snap`:15 on 2025-04-10 20:38_

This is a separate issue from the changes here, but I don't think `Expr::Name`s need to be parenthesized. Let's not modify this here, but I'm surprised to see `(x).bit_count` instead of `x.bit_count`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB161_FURB161.py.snap`:119 on 2025-04-10 20:40_

This one is also a bit unfortunate because this is obviously an integer. I think this would be helped by `ResolvedPythonType::from` as @dscorbett suggested.

---

_@ntBre requested changes on 2025-04-10 20:52_

Thanks for working on this! Overall I think this is good, but we can simplify things a bit by using a fix `Applicability`, and then I think we can do a better job with type inference with `ResolvedPythonType::from`, specifically for `Expr::BinOp` (cases like `1 + 2`) and for `Expr::UnOp` (cases like `-1`).

If we extend the safe handling to `bool`, we may also be able to resolve expressions like `True or False`.

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/refurb/rules/bit_count.rs`:125 on 2025-04-18 10:00_

Done

---

_@VascoSch92 reviewed on 2025-04-18 10:00_

---

_@VascoSch92 reviewed on 2025-04-18 10:28_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB161_FURB161.py.snap`:119 on 2025-04-18 10:28_

Now it is safe ;-)

---

_Comment by @VascoSch92 on 2025-04-18 10:29_

> Thanks for working on this! Overall I think this is good, but we can simplify things a bit by using a fix `Applicability`, and then I think we can do a better job with type inference with `ResolvedPythonType::from`, specifically for `Expr::BinOp` (cases like `1 + 2`) and for `Expr::UnOp` (cases like `-1`).
> 
> If we extend the safe handling to `bool`, we may also be able to resolve expressions like `True or False`.

Hey @ntBre 
I changed to use `ResolvedPythonType`. Let me know if I used it in the correct way ;-)

---

_Review requested from @ntBre by @VascoSch92 on 2025-04-18 10:33_

---

_Comment by @ntBre on 2025-04-18 17:36_

Thanks, it looks good to me! 

Let's save this for a follow-up PR like the `Expr::Name` idea I mentioned above, but I think we can even do better than `ResolvedPythonType` to handle variable assignments too, like the `x = 10` case in the existing tests. There doesn't seem to be an existing `is_bool` function in our `typing` module, but we could easily add one similar to `is_int`:

https://github.com/astral-sh/ruff/blob/27a315b74095fa0d4dd83749b8a5ff567e06a38b/crates/ruff_python_semantic/src/analyze/typing.rs#L968-L971

You definitely don't have to work on this, just a couple of ideas if you're interested :)

I pushed two tiny tweaks:
- reverting a couple of lines that just moved around throughout the PR
- adding a little more context to the fix safety docs

I'll merge as soon as CI passes!

---

_@ntBre approved on 2025-04-18 17:36_

---

_Renamed from "[`refurb`] Unsafe fixes in `FURB161`" to "[`refurb`] Mark the `FURB161` fix unsafe except for integers and booleans" by @ntBre on 2025-04-18 17:38_

---

_Merged by @ntBre on 2025-04-18 17:46_

---

_Closed by @ntBre on 2025-04-18 17:46_

---

_Branch deleted on 2025-04-18 18:00_

---

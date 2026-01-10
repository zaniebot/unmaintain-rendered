```yaml
number: 19424
title: "[`pyupgrade`]  Apply `UP008` only when the `__class__` cell exists"
type: pull_request
state: merged
author: IDrokin117
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix/ruff-19357
created_at: 2025-07-18T15:33:09Z
updated_at: 2025-09-09T18:59:24Z
url: https://github.com/astral-sh/ruff/pull/19424
synced_at: 2026-01-10T17:46:21Z
```

# [`pyupgrade`]  Apply `UP008` only when the `__class__` cell exists

---

_Pull request opened by @IDrokin117 on 2025-07-18 15:33_

## Summary

Resolves #19357 

Skip UP008 diagnostic for `builtins.super(P, self)` calls when `__class__` is not referenced locally, preventing incorrect fixes.  

**Note:** I haven't found concrete information about which cases `__class__` will be loaded into the scope. Let me know if anyone has references, it would be useful to enhance the implementation. I did a lot of tests to determine when `__class__` is loaded. Considered sources:
1.  [Python doc super](https://docs.python.org/3/library/functions.html#super)
2.  [Python doc classes](https://docs.python.org/3/tutorial/classes.html)
3. [pep-3135](https://peps.python.org/pep-3135/#specification)

As I understand it, Python will inject at runtime into local scope a `__class__` variable if it detects references to `super` or `__class__`. This allows calling `super()` and passing appropriate parameters. However, the compiler doesn't do the same for `builtins.super`, so we need to somehow introduce `__class__` into the local scope.

I figured out `__class__` will be in scope with valid value when two conditions are met:
1. `super` or `__class__` names have been loaded within function scope
4. `__class__` is not overridden.

I think my solution isn't elegant, so I would be appreciate a detailed review.

## Test Plan

Added 19 test cases, updated snapshots.


---

_Comment by @github-actions[bot] on 2025-07-18 15:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Converted to draft by @IDrokin117 on 2025-07-20 13:13_

---

_Marked ready for review by @IDrokin117 on 2025-07-21 15:01_

---

_Comment by @IDrokin117 on 2025-07-24 12:56_

@ntBre Could you please review a PR?

---

_Review requested from @ntBre by @ntBre on 2025-07-24 13:04_

---

_Label `bug` added by @ntBre on 2025-07-24 13:04_

---

_Label `fixes` added by @ntBre on 2025-07-24 13:04_

---

_Comment by @ntBre on 2025-07-24 13:04_

Will do! Sorry, I'm a bit behind on my review queue, but this has been on the list.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:103 on 2025-08-07 15:58_

Oh I see, you need `func_stmt` below. I _think_ you can use something like this:

```rust
let Some(func_stmt @ Stmt::FunctionDef(ast::StmtFunctionDef {
    ...
```

in the old version, but this is fine if not.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:207 on 2025-08-07 15:59_

```suggestion

/// Returns `true` if the function contains load references to `__class__` or `super` without
```

tiny nit

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/helpers.rs`:1 on 2025-08-07 16:01_

Does this need to be in the `ruff_python_ast` crate? It usually makes sense to define the visitor next to the rule using it, if it's only used by that rule.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:223 on 2025-08-07 16:04_

Because these are both `any`, I think we can just check these conditions as the visitor visits and avoid needing to collect a `Vec` of expressions. We may even be able to terminate the visitor early in that case.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:224 on 2025-08-07 16:05_

This looks like a cheaper check than visiting a `Stmt`, so we should probably check this first and return early if it's true (?)

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:228 on 2025-08-07 16:06_

I think you can use `match_builtin_expr` for this:

https://github.com/astral-sh/ruff/blob/96ffd8a134d62f5426c9d94d15adb21100d90f01/crates/ruff_python_semantic/src/model.rs#L319

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP008.py`:156 on 2025-08-07 16:11_

I think we could strip these examples down a little bit, just to make it clearer which parts are important. For example, I don't think the parent class even needs to exist actually, and the same with the following calls. Basically I'd just use this:


```suggestion
class ChildD1:
    def f(self):
        if False: __class__ # Python injects __class__ into scope
        builtins.super(ChildD1, self).f()
```

unless I'm missing some other critical part of the example.

Or we could maybe preserve one of the parent classes and the calls, if it's helpful to be able to run the code. I just don't think we need one parent class per example, if they're all the same, at least.

---

_@ntBre reviewed on 2025-08-07 16:23_

Thanks, this looks great! Sorry again for the delay.

I just had a few small suggestions. 

Thanks for the docs links too. I skimmed over a few of those, and I think you've captured everything I understood from them and from the issue too :)

---

_Review comment by @IDrokin117 on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:103 on 2025-08-07 18:55_

Got it. Changed to @ usage

---

_Review comment by @IDrokin117 on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:207 on 2025-08-07 19:00_

Edited

---

_Review comment by @IDrokin117 on `crates/ruff_python_ast/src/helpers.rs`:1 on 2025-08-07 19:09_

I did it just because at least `AwaitVisitor` and `RaiseStatementVisitor` are defined in helpers, but are used only once in rules (`cancel_scope_no_checkpoint` and `raise_without_from_inside_except` respectively) similar to my use case. But yes, it makes sense to move it next to the rule. Done.

---

_Review comment by @IDrokin117 on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:224 on 2025-08-07 19:21_

Good point. Moved it to the beginning.

---

_Review comment by @IDrokin117 on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:228 on 2025-08-07 19:34_

`match_builtin_expr(&call.func, "super")` returns true for both cases: `super()` and `builtins.super()`, but I need to check only the second case.

---

_Review comment by @IDrokin117 on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:223 on 2025-08-07 21:06_

My intention was to make it simple and not necessarily optimized.   
I have reworked it, and now it is implemented according to your recommendation, but I think it is more complex to read. I would appreciate any simplification advice.

BTW, it was intentional to pass conditions due to initialization rather than hardcode them in `visit_expr`. In my opinion, it provides more flexibility for future changes.

---

_Review comment by @IDrokin117 on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP008.py`:156 on 2025-08-07 21:18_

Make sense for me. Removed duplicated parents and instance creation with method calling.

---

_@IDrokin117 reviewed on 2025-08-07 21:21_

---

_Review requested from @ntBre by @IDrokin117 on 2025-08-09 20:08_

---

_Comment by @ntBre on 2025-08-13 18:22_

I just came back to this notification right after reviewing https://github.com/astral-sh/ruff/pull/19783. I somehow hadn't connected these two before, but now I'm wondering if they have the same root cause. I think if we're able to work out a neat, general solution for binding the `__class__` cell it might help here too.

---

_Comment by @ntBre on 2025-08-26 13:54_

We added a new `ScopeKind::DunderClassCell` (and `BindingKind::DunderClassCell`) in https://github.com/astral-sh/ruff/pull/20048. I don't believe that will resolve the issue addressed by this PR, but would you mind seeing if it makes anything easier? I was hoping it might help to avoid the new `Visitor`, but I think we'll still need some rule-specific changes in any case.

Thanks again for your work here :)

---

_Comment by @IDrokin117 on 2025-08-26 18:05_

@ntBre Good news! Here are my thoughts after reviewing the new feature.  
It seems the `__class__` binding is being created in any case now, regardless of whether `__class__` and `super` names are present. Am I right? This is going to be simple and good for some rules, but not for this case. Since the `__class__` binding pretends to always exist within a method, it doesn't provide any new information. I mean, if a `__class__` cell is available, then `builtins.super()` has to work. But that's not true since `__class__` is not always available. I see two options to resolve the issue:
 1. Left the extra visitor to ensure determination of whether `__class__` is really injected at runtime based on specific rules.
 2. Simplify the rule and apply it only if `super()` is encountered, ignoring `builtins.super()`.
 
Since I've already implemented tailored `__class__` determination, I prefer the first option. What do you think?

---

_Comment by @ntBre on 2025-08-29 13:34_

Thanks for looking into it and rebasing the PR!

> It seems the `__class__` binding is being created in any case now, regardless of whether `__class__` and `super` names are present. Am I right? 

Yes that's right, we didn't try to detect if `__class__` or `super` was used, so it makes sense that it may not help here.

> 1. Left the extra visitor to ensure determination of whether `__class__` is really injected at runtime based on specific rules.
> 2. Simplify the rule and apply it only if `super()` is encountered, ignoring `builtins.super()`.
> 
> Since I've already implemented tailored `__class__` determination, I prefer the first option. What do you think?

Makes sense to me, I'll give this another review soon!



---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:225 on 2025-09-09 15:45_

Would it make sense to combine these conditions in one closure? Maybe something like:


```suggestion
        |expr: &Expr| {
            expr.as_name_expr()
                .is_some_and(|name| matches!(name.id.as_str(), "super" | "__class__") && name.ctx.is_load())
        },
```

I think it could even make sense just to hard-code this check into the visitor since we're not really using it generically, as far as I can tell.

I think you might have been addressing this in [this comment](https://github.com/astral-sh/ruff/pull/19424#discussion_r2261395337), but we can always make it more general in the future if we have to. I'm not really foreseeing that at the moment.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:243 on 2025-09-09 15:51_

Would an `Option<&'a Expr>` make sense here? It looks like we only ever check if it's empty or not, which could be `is_none` instead (and `Some(expr)` instead of `insert(0)`). Or just a boolean flag since it looks like we just want to record whether we found it.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:212 on 2025-09-09 15:59_

Based on my reading of the [docs](https://docs.python.org/3/reference/datamodel.html#creating-the-class-object) we found for the other `__class__` PR, I think we may need to check _all_ of the methods in a class definition:

> `__class__` is an implicit closure reference created by the compiler if **any methods** in a class body refer to either `__class__` or `super`.

(emphasis mine)

This could be quite expensive since we'd have to re-visit the whole class body, so it might still make sense to approximate the behavior by only checking each function separately. But we could still add a test and document the limitation, at least.

---

_@ntBre reviewed on 2025-09-09 16:11_

Thanks! Just a couple of minor simplification suggestions and one corner case from the docs.

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:212 on 2025-09-09 16:47_

Each method is considered individually. `__class__` in one method has no effect on another.
```pycon
>>> class C(str):
...     def f1(self):
...         return super().__len__()
...     def f2(self):
...         import builtins
...         return builtins.super().__len__()
... 
>>> C().f1()
0
>>> C().f2()
Traceback (most recent call last):
  File "<python-input-2>", line 1, in <module>
    C().f2()
    ~~~~~~^^
  File "<python-input-0>", line 6, in f2
    return builtins.super().__len__()
           ~~~~~~~~~~~~~~^^
RuntimeError: super(): __class__ cell not found
```

---

_@dscorbett reviewed on 2025-09-09 16:47_

---

_@IDrokin117 reviewed on 2025-09-09 16:50_

---

_Review comment by @IDrokin117 on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:212 on 2025-09-09 16:50_

At least test case `ChildI2` covers it

---

_@ntBre reviewed on 2025-09-09 16:55_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:212 on 2025-09-09 16:55_

Ah okay, thanks. I should have actually tested it instead of reading so much into the phrasing. And I overlooked the existing test :facepalm: thank you both!

---

_@IDrokin117 reviewed on 2025-09-09 18:08_

---

_Review comment by @IDrokin117 on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:225 on 2025-09-09 18:08_

I simplified the Visitor

---

_Review comment by @IDrokin117 on `crates/ruff_linter/src/rules/pyupgrade/rules/super_call_with_parameters.rs`:243 on 2025-09-09 18:09_

Made it boolean

---

_@IDrokin117 reviewed on 2025-09-09 18:09_

---

_Review requested from @ntBre by @IDrokin117 on 2025-09-09 18:09_

---

_@ntBre approved on 2025-09-09 18:58_

Thanks!

---

_Renamed from "[`pyupgrade`]  Apply UP008 only when the __class__ cell exists (`UP008`)" to "[`pyupgrade`]  Apply `UP008` only when the `__class__` cell exists (`UP008`)" by @ntBre on 2025-09-09 18:58_

---

_Renamed from "[`pyupgrade`]  Apply `UP008` only when the `__class__` cell exists (`UP008`)" to "[`pyupgrade`]  Apply `UP008` only when the `__class__` cell exists" by @ntBre on 2025-09-09 18:59_

---

_Merged by @ntBre on 2025-09-09 18:59_

---

_Closed by @ntBre on 2025-09-09 18:59_

---

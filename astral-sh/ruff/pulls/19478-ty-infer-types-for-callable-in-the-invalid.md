```yaml
number: 19478
title: "[ty] Infer types for `Callable` in the invalid arguments case"
type: pull_request
state: closed
author: lipefree
labels:
  - bug
  - server
  - ty
assignees: []
base: main
head: remove-pull-type
created_at: 2025-07-22T07:32:54Z
updated_at: 2025-07-24T11:38:42Z
url: https://github.com/astral-sh/ruff/pull/19478
synced_at: 2026-01-12T15:56:40Z
```

# [ty] Infer types for `Callable` in the invalid arguments case

---

_@lipefree_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes https://github.com/astral-sh/ty/issues/815. 

Previously a snippet of the form : 

```py
# fmt: off

def _(c: Callable[ 
            {1, 2}  
        ]
    ):
    reveal_type(c)  # revealed: (...) -> Unknown
```
or 


caused a panic in mdtests because no type was stored for argument `{1, 2}`. Now we store a type for the single argument case with `CallableType::unknown(..)` if it is a valid first argument to `Callable` (list of types, `ParamSpec`, Concatenate or `...`) or we infer it otherwise. For the tuple case, we make sure to infer the types in case the first argument is not valid.

## Test Plan

<!-- How was it tested? -->

Removed the `pull-types:skip` directive and added an additional error in the relevant test. 


---

_Comment by @github-actions[bot] on 2025-07-22 07:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `server` added by @AlexWaygood on 2025-07-22 07:56_

---

_Label `ty` added by @AlexWaygood on 2025-07-22 07:56_

---

_Renamed from "[ty] Correctly infer types for `Callable` in the single argument case" to "[ty] Infer types for `Callable` in the invalid arguments case" by @lipefree on 2025-07-22 09:08_

---

_Marked ready for review by @lipefree on 2025-07-22 09:50_

---

_Review requested from @carljm by @lipefree on 2025-07-22 09:50_

---

_Review requested from @AlexWaygood by @lipefree on 2025-07-22 09:50_

---

_Review requested from @sharkdp by @lipefree on 2025-07-22 09:50_

---

_Review requested from @dcreager by @lipefree on 2025-07-22 09:50_

---

_Label `bug` added by @AlexWaygood on 2025-07-22 10:42_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/annotations/callable.md`:45 on 2025-07-22 21:03_

I think _ideally_ the first error here is sufficient and we don't need the second one, too. But if that's really difficult, it's not a big deal.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/annotations/callable.md`:86 on 2025-07-22 21:04_

Similarly here, I think the below error sort of covers this already?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:9615 on 2025-07-22 21:32_

This doesn't feel right (and it's the cause of the "unnecessary" extra diagnostics I mentioned in the tests). We know that whatever expression is here, it's not valid in this context. It doesn't make sense to then try to infer it as a type expression (when an arbitrary type expression is not valid here, even if its a valid type expression!). We are only doing this because it's the most convenient way to ensure a type is assigned to every sub-expression. If we had a "walk expression tree and store Unknown for every sub-expression" it would make more sense to use that here -- but we haven't built that utility. And really even that seems wasteful.

I feel like this is pushing me over the edge where I am really ready to just implement a blanket fallback to Unknown (at query time) for any expression we didn't store a type for, and be done with the "no type stored for this expression" panics once and for all. I think it's fine if we occasionally miss an expression, end up with Unknown, and eventually get a bug report. And it can save us so much useless work (here I mean partially runtime work, but also development work) in error cases, where it would just become fine/correct to return early and not bother with storing types.

---

_@carljm reviewed on 2025-07-22 21:33_

Thanks for working on this! I'm not quite happy with this approach, though -- see inline comment. Would like to take this to the team because IMO this is really an argument for just changing our whole approach to missing stored expression types.

---

_@lipefree reviewed on 2025-07-23 06:09_

---

_Review comment by @lipefree on `crates/ty_python_semantic/src/types/infer.rs`:9615 on 2025-07-23 06:09_

I totally understand your opinion on this ! I have learn quite a few tricks in rust playing around with this PR so it is ok if it doesn't get merge at all. I will be happy on working on the refactoring of "no type stored for this expression" if a new mechanism for handling such cases is introduced.

---

_@sharkdp reviewed on 2025-07-23 06:37_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:9615 on 2025-07-23 06:37_

> If we had a "walk expression tree and store Unknown for every sub-expression" it would make more sense to use that here -- but we haven't built that utility.

I [tried that half a year ago](https://github.com/astral-sh/ruff/pull/14629) when I was fed up with patching a lot of these panics :-)

> I feel like this is pushing me over the edge where I am really ready to just implement a blanket fallback to Unknown (at query time) for any expression we didn't store a type for, and be done with the "no type stored for this expression" panics once and for all. I think it's fine if we occasionally miss an expression, end up with Unknown, and eventually get a bug report. And it can save us so much useless work (here I mean partially runtime work, but also development work) in error cases, where it would just become fine/correct to return early and not bother with storing types.

Strong agree (see also my "side remark" in the linked PR). We could maybe still make it a hard error (or at least an error in debug mode) if we fall back to `Unknown` within valid code, if that information is easy to retrieve/generate?

---

_@AlexWaygood reviewed on 2025-07-23 12:21_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:9615 on 2025-07-23 12:21_

I still have concerns about the impact this might have on the ability to use ty as a backend for Ruff in the long-term (https://github.com/astral-sh/ruff/pull/14629#issuecomment-2505930718, https://github.com/astral-sh/ruff/pull/14629#issuecomment-2506271894).

I agree that we've spent a frustrating amount of time chasing down corpus panics for invalid syntax and/or invalid type expressions. But these corpus tests have also caught some genuine bugs where we weren't storing subexpressions for nodes involving _valid_ type expressions -- https://github.com/astral-sh/ruff/pull/18535 and https://github.com/astral-sh/ruff/pull/18536 were examples. I think it would be really confusing for users if they saw `Unknown` when hovering over an AST node inside a valid type expression. It would also be hard to figure out why a lint rule that used ty as a backend wasn't getting the correct result when a bug was caused by `Unknown` being stored for an AST subnode. So I do think the corpus tests are valuable.

We are definitely too panicky in the server right now, though, and we definitely emit too many cascading diagnostics for invalid type expressions. My preferred solution for the first problem would be to keep the strict corpus tests, but to avoid indexing directly into the expression map from the server code -- instead we should do something like `expression_map.get(node).unwrap_or(Type::unknown())`.

---

_@carljm reviewed on 2025-07-23 21:20_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:9615 on 2025-07-23 21:20_

I looked back at that prior discussion, and I am not convinced that future-type-aware-Ruff is sufficient motivation for us to work so hard to infer meaningful types for invalid expressions. Invalid expressions aren't common, issue their own errors, and tend to be fixed -- we don't need type-aware-Ruff to issue a cascading diagnostic for every other possible lint issue inside an invalid annotation. (I don't buy that there are any significant numbers of people who care about Ruff emitting a diagnostic for `Literal[1, 1]` everywhere it is used, but don't care about valid type annotations in the first place.)

It's still not clear to me how you would actually propose to handle the case in this PR. Should we infer it as a type expression, even though it is not in a context where a type expression is either valid or expected? As a value expression, even though it is not a context where a value expression is valid or expected? Neither option makes semantic sense, and both risk unnecessary/confusing cascading errors. I don't know of any other option that's been suggested, other than "explicitly store Unknown for the whole tree." That option is significantly more work, and it still doesn't help the Ruff case.

Yes, it's not ideal if we wrongly reveal `Unknown` on hover in some valid case where we should reveal some other type. If this happens and it's noticeable, it will eventually be reported as a bug and we will fix it like any other bug. ("Failed to infer a type for an expression" is only one way this can happen; enforcing an explicitly-stored type for every expression doesn't prevent it.) I don't think this is a super high priority category of bug, and I think the cost of this particular method of helping us to prevent it is significantly greater than the benefit.

I agree that at minimum we must start using a fallback when we pull types in the server, so we don't panic, but I don't think that's sufficient. If we maintain that the corpus tests must find an explicit type for every expression, that means we still (in principle, though we won't catch the bugs as quickly via playground/LSP panics) require ourselves to always find some way to infer and explicitly store a type for every expression, including in all error cases. And I think that is a bad use of our time, and we should stop doing it.

I think the corpus tests should continue to exist, and should continue to pull a type for every expression, because that helps catch other failures/panics. But the corpus tests should also use a fallback to `Unknown` and no longer panic if they don't find an explicitly-stored type for some expression.

@lipefree 

> I will be happy on working on the refactoring of "no type stored for this expression" if a new mechanism for handling such cases is introduced.

Thank you for your understanding! I think we really already have most of the mechanism, and this is a relatively easy change to `TypeInference::expression_type` -- instead of using `.expect` and panicking if `try_expression_type` returns `None`, it should instead fallback to `Unknown`.

---

_Closed by @carljm on 2025-07-23 21:21_

---

_@carljm reviewed on 2025-07-24 00:08_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:9615 on 2025-07-24 00:08_

I actually just went ahead and put up [a PR for this](https://github.com/astral-sh/ruff/pull/19517), since it was quite simple -- though not quite as simple as I said above, due to https://github.com/astral-sh/ruff/pull/19435. I reused your added test from this PR, so I added you as a co-author.

---

_@AlexWaygood reviewed on 2025-07-24 11:38_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:9615 on 2025-07-24 11:38_

> It's still not clear to me how you would actually propose to handle the case in this PR. Should we infer it as a type expression, even though it is not in a context where a type expression is either valid or expected? As a value expression, even though it is not a context where a value expression is valid or expected? Neither option makes semantic sense, and both risk unnecessary/confusing cascading errors. I don't know of any other option that's been suggested, other than "explicitly store Unknown for the whole tree." That option is significantly more work, and it still doesn't help the Ruff case.

I'd still like to try out the idea I mentioned in https://github.com/astral-sh/ty/issues/727#issuecomment-3019180248, of refactoring `infer_type_expression` to return a `Result` rather than eagerly emitting diagnostics. I think this would have a lot of benefits more broadly. But it wouldn't be an easy refactor; your solution certainly gets us to a no-panic state more quickly.

---

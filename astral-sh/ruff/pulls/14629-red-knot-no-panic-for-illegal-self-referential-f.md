```yaml
number: 14629
title: "[red-knot] No panic for illegal self-referential f-string annotations"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
base: main
head: david/self-referential-fstring-annotations
created_at: 2024-11-27T09:19:57Z
updated_at: 2025-04-28T23:12:56Z
url: https://github.com/astral-sh/ruff/pull/14629
synced_at: 2026-01-12T15:55:48Z
```

# [red-knot] No panic for illegal self-referential f-string annotations

---

_@sharkdp_

## Summary

I went down another rabbit hole trying to fix all sorts of panics related to illegal type expressions. Some highlights from the linter corpus:
```py
x: f"{x}"
x: (y := x)
x: lambda y: y
type X = Annotated[int, lambda: re.compile("x")]
```

This fixes the last two remaining entries in `KNOWN_FAILURES` except for those related to salsa-cycles. But I really don't know if the approach taken here — to infer `Type::Unknown` for sub-expressions of illegal expressions — is the right one. And even if it is, I'm not sure if my implementation of it is fully correct (see for example the exception for expressions with an inner scope). I thought I'd still open a PR so we can discuss the general problem.

Side remark: After having fixed a few of those panics over the last weeks, my impression is that our approach to upholding the invariant "we infer and store a type for each and every expression, even if illegal in this context" is extremely brittle. The only way we enforce it right now is through the corpus tests. But there is an exponentially large number of branches in type inference (or alternatively: an exponentially large number of weird edge cases like the above), which makes me think that we might need another way to enforce this constraint, e.g. at the (Rust) type-system level instead of the test level.

## Test Plan

```rs
cargo nextest run -p red_knot_workspace -- --ignored linter_af linter_gz
```

---

_Label `red-knot` added by @sharkdp on 2024-11-27 09:19_

---

_Review requested from @carljm by @sharkdp on 2024-11-27 09:19_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-27 09:19_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-27 09:19_

---

_Comment by @github-actions[bot] on 2024-11-27 09:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-11-27 10:36_

> Side remark: After having fixed a few of those panics over the last weeks, my impression is that our approach to upholding the invariant "we infer and store a type for each and every expression, even if illegal in this context" is extremely brittle. The only way we enforce it right now is through the corpus tests. But there is an exponentially large number of branches in type inference (or alternatively: an exponentially large number of weird edge cases like the above), which makes me think that we might need another way to enforce this constraint, e.g. at the (Rust) type-system level instead of the test level.

I definitely agree with this, if it's possible. But I haven't thought at all about how we might redesign our infrastructure to make this possible!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/literal/literal.md`:79 on 2024-11-27 10:37_

Hmm, I think this is a regression? We added support for `Literal[...]` annotations in https://github.com/astral-sh/ruff/pull/13874

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/string.md`:128 on 2024-11-27 10:38_

Is this not a regression? I think the existing error was correct here -- this looks like a quoted annotation, and the quoted expression is not a valid type annotation

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/red_knot.rs`:30 on 2024-11-27 10:41_

There are several type annotations on this line, and it's hard to tell which annotation this is referring to without counting the columns. Could we improve the error message so it specifies which variable or annotation the diagnostic is complaining about?

---

_@AlexWaygood reviewed on 2024-11-27 10:42_

---

_@sharkdp reviewed on 2024-11-27 12:01_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/literal/literal.md`:79 on 2024-11-27 12:01_

Note that this is not `typing.Literal`, but `other.Literal`.

---

_@AlexWaygood reviewed on 2024-11-27 12:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/literal/literal.md`:79 on 2024-11-27 12:03_

Oh, thanks! In that case, I think this is a true positive, and we should remove the TODO?

---

_@MichaReiser reviewed on 2024-11-27 12:05_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/red_knot.rs`:30 on 2024-11-27 12:05_

Including arbitrary expressions is something that I'm always worried about because it can lead to very long messages, but including the name might be an option or David can come up with something that avoids this. 

What should help here is when we have rich diagnostics that also show a code frame

---

_@sharkdp reviewed on 2024-11-27 12:07_

---

_Review comment by @sharkdp on `crates/ruff_benchmark/benches/red_knot.rs`:30 on 2024-11-27 12:07_

When I wrote that error message, I was looking for something like a `ast::Expr::human_readable_name()` method that would return "lambda expression" for a `Expr::Lambda` etc., but didn't find anything.

I'm happy to improve this error message though once we agree that this is even the right approach to begin with.

> What should help here is when we have rich diagnostics that also show a code frame

:+1: 

---

_@AlexWaygood reviewed on 2024-11-27 12:08_

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/red_knot.rs`:30 on 2024-11-27 12:08_

> What should help here is when we have rich diagnostics that also show a code frame

That should help for sure, but it's also useful to have important information in the summary line for when a user is using `--output-format=concise`. I found `--output-format=concise` to be very useful when filing PRs to upgrade projects to Ruff 0.8, because there were many new violations on some codebases, and it was very hard to scroll through them all when using the default verbose output.

---

_@sharkdp reviewed on 2024-11-27 12:09_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/literal/literal.md`:79 on 2024-11-27 12:09_

Yes, seems like I was similarly confused when I added the TODO. Thanks!

---

_@sharkdp reviewed on 2024-11-27 12:17_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/annotations/string.md`:128 on 2024-11-27 12:17_

> Is this not a regression?

Hm... maybe?

We do emit diagnostics for wrong string-kinds in annotations (all the other examples here). But in general, we do not emit any diagnostic for invalid expressions *inside correct string annotations*. Something like `x: "lambda y: y"` does not lead to a diagnostic on `main`. So having `x: "b'int'"` *not* emit a diagnostic either seems consistent to me.

If we generally want to show errors for all these cases, we can also do that, I believe.

---

_@AlexWaygood reviewed on 2024-11-27 12:28_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/string.md`:128 on 2024-11-27 12:28_

I see. I think we probably _do_ want errors for these cases? But if this is consistent with what we have currently elsewhere, then it's probably better to leave it as you have it for now, and tackle it in isolation as a followup.

---

_Comment by @AlexWaygood on 2024-11-27 12:37_

> But I really don't know if the approach taken here — to infer `Type::Unknown` for sub-expressions of illegal expressions — is the right one. And even if it is, I'm not sure if my implementation of it is fully correct (see for example the exception for expressions with an inner scope). I thought I'd still open a PR so we can discuss the general problem.

Yeah, I'm not sure about this either. I think there are two reasons why we want to infer types for all sub-expressions:
1. So that you get a useful tool-tip telling you what the type of something is when you hover over a subexpression of a type annotation in a red-knot-powered IDE
2. In the long term, we want to rewrite Ruff to use red-knot as a semantic-inference backend, and we may have rules that want to know the type of subexpressions of annotations even when the overall annotation is invalid. (Some rules might want type information even if the rule itself is not about enforcing valid annotations or types.)

I think inferring `Unknown` for all subexpressions of invalid annotations is probably _okay_ for the first of these (if the annotation is invalid, it's invalid), but probably not great for the second of these.

---

_Comment by @carljm on 2024-11-28 05:36_

> our approach to upholding the invariant "we infer and store a type for each and every expression, even if illegal in this context" is extremely brittle.

Yeah, I agree. I'm having a hard time thinking of how we could enforce this invariant in the type system. (At least without major re-architecting.)

I think I am reaching the conclusion that we should just have a default fallback to `Unknown` for any expression that we've failed to store a type for, and if we discover cases where we have `Unknown` where there should be a more precise type, we treat that as a bug like any other type inference bug, fix it and add a test.

I think this seems like the right answer to me in particular due to invalid syntax, because a) we can't reasonably expect to cover the full space of possible invalid syntax with any testing strategy, and b) I think, in general, `Unknown` is the right answer for invalid syntax cases that we cannot understand otherwise.

> I think inferring Unknown for all subexpressions of invalid annotations is probably okay for the first of these (if the annotation is invalid, it's invalid), but probably not great for the second of these.

I'm not sure it isn't the best option for the second, too. How symbols in type expressions are interpreted is contextual, and if we've departed from a valid context, I think it's reasonable to say that we really don't know what the type of sub-expressions should be.

---

_@carljm approved on 2024-11-28 05:39_

I would be more inclined to switch to a default fallback of Unknown at lookup time (instead of panic), rather than adding all this infrastructure to walk a tree and store an explicit Unknown for everything in it.

But I don't feel strongly about it, and won't object if this PR is merged as-is.

---

_Comment by @AlexWaygood on 2024-11-28 11:47_

> > I think inferring Unknown for all subexpressions of invalid annotations is probably okay for the first of these (if the annotation is invalid, it's invalid), but probably not great for the second of these.
> 
> I'm not sure it isn't the best option for the second, too. How symbols in type expressions are interpreted is contextual, and if we've departed from a valid context, I think it's reasonable to say that we really don't know what the type of sub-expressions should be.

But remember that we won't just be using red-knot from the linter for its type-inference capabilities, but also for its more general "symbol-aware" capabilities.

For example, take [PYI062](https://docs.astral.sh/ruff/rules/duplicate-literal-member/). This rule tries to detect duplicate members in a literal slice. When it's ported to red-knot, it will need to work largely off the AST, as it does now, because in our red-knot representation of a `Literal` annotation we'll eagerly deduplicate `Literal` elements as we add them to a `UnionType`. But the rule still needs to know that the symbol `Literal` is actually the symbol `typing.Literal` or `typing_extensions.Literal` before it starts traversing the AST, and for that it needs red-knot's multifile type-inference capabilities. It doesn't really matter for that rule whether the `Literal` slice occurs in the context of an annotation that is overall invalid; it can still do an accurate analysis of the `Literal` slice inside the invalid annotation. But if we infer `Unknown` for all subexpressions in the annotation, we won't be able to inform that rule whether or not the symbol `Literal` is actually `typing(_extensions).Literal` or if it's some other `Literal` symbol.

---

_Comment by @carljm on 2024-11-28 14:08_

Yes, I understood all of that from your first comment. This is the assumption I'm not sure I agree with:

> It doesn't really matter for that rule whether the Literal slice occurs in the context of an annotation that is overall invalid; it can still do an accurate analysis of the Literal slice inside the invalid annotation.

I'm not sure it's reasonable to expect that rule to fire inside an arbitrary invalid context, because the meaning of these expressions is contextual.

For example, the first "argument" to `Annotated` is not a type expression: should that lint rule fire on a `Literal` slice that occurs there?

---

_Comment by @AlexWaygood on 2024-11-28 14:38_

> For example, the first "argument" to `Annotated` is not a type expression: should that lint rule fire on a `Literal` slice that occurs there?

Yes, I think it should! It's just as much an error to have duplicate elements in a `Literal` slice even if the `Literal` is not part of a type expression, because the typing module at runtime eagerly deduplicates elements:

```pycon
>>> from typing import Literal
>>> Literal[1, 1]
typing.Literal[1]
```

So even if the `Literal` is part of the first argument to `Annotated` for whatever reason, it's always going to be incorrect to pass duplicate elements to the `Literal` slice.

---

_Comment by @carljm on 2024-12-02 19:26_

Yes, I see that it would be nice to correctly infer the type of a reference to something like `typing.Literal` in any context.

I wish we weren't constrained by Salsa query granularity, so we didn't have to do the extra work of type inference on expressions that are useless for type checking, just because a lint rule or an LSP hover might (but probably in many cases never will) ask for it.

---

_Comment by @AlexWaygood on 2024-12-03 12:58_

> I wish we weren't constrained by Salsa query granularity, so we didn't have to do the extra work of type inference on expressions that are useless for type checking, just because a lint rule or an LSP hover might (but probably in many cases never will) ask for it.

Right. Though the consolation here is that invalid annotations should generally be pretty rare in well-written code, so the "extra cost" of inferring sub-expressions in these invalid annotations should be pretty minimal, I'd wager.

---

_Comment by @MichaReiser on 2025-04-28 07:02_

Is this still something we want or should we close this PR?

---

_Comment by @carljm on 2025-04-28 23:12_

I think probably not? I'm not sure what exactly we changed that fixed this, but the original motivating example in the description no longer panics, even when included in the test corpus (so that we try to pull types for all expressions.)

(I do still think we should switch at some point to defaulting to `Unknown` for expressions we failed to store a type for -- and treating this as an ordinary bug when it matters -- rather than panicking. But I don't think this will be an invasive change, so there's no particular rush to do it. We could do it for the alpha?)

---

_Closed by @carljm on 2025-04-28 23:12_

---

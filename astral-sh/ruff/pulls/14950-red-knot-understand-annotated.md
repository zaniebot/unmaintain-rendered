```yaml
number: 14950
title: "[red-knot] Understand `Annotated`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-annotated
created_at: 2024-12-13T01:05:34Z
updated_at: 2024-12-13T17:50:37Z
url: https://github.com/astral-sh/ruff/pull/14950
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] Understand `Annotated`

---

_@InSyncWithFoo_

## Summary

Resolves #14922.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2024-12-13 01:05_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2024-12-13 01:05_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2024-12-13 01:05_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2024-12-13 01:05_

---

_Comment by @InSyncWithFoo on 2024-12-13 03:51_

Alright, I'm out of ideas for now. What am I missing?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:3 on 2024-12-13 05:41_

```suggestion
`Annotated` attaches arbitrary metadata to a given type.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:33 on 2024-12-13 05:48_

I see that pyright just defaults to `Any` and doesn't emit a diagnostic here, but mypy does. I don't see any support for pyright's behavior in the typing spec, so I would suggest that bare Annotated should be an error.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:77 on 2024-12-13 05:52_

If we don't emit a diagnostic on bare `Annotated` in an annotation, it would suggest that this also should be treated implicitly as just inheriting `Any`, with no error. But I don't like that; I prefer erroring here, and always erroring on bare `Annotated`, as mentioned above.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:37 on 2024-12-13 05:57_

How about a test with too many (3+) type parameters?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4831 on 2024-12-13 05:58_

```suggestion
                if elts.len() != 2 {
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4845 on 2024-12-13 06:00_

We currently have a requirement that we store a type for every expression (in order to support hover in an LSP). I suspect the problem you're running into is that you never infer or store a type for the second or subsequent parameters. (You store a type for the overall tuple expression, but not for the individual expressions in second+ positions, or any possible sub-expressions of those, etc.) I think you will need to infer the first as a type expression (as you already do, using `self.infer_type_expression`), infer the rest of the parameters as value expressions (using `self.infer_expression`), and then return the type of the first and ignore the rest.

---

_@carljm reviewed on 2024-12-13 06:01_

Thanks for working on this! Sorry about the bad experience, our expression coverage and the way it manifests in hard-to-debug errors is a known pain point we want to improve.

---

_Label `red-knot` added by @MichaReiser on 2024-12-13 07:03_

---

_@AlexWaygood reviewed on 2024-12-13 08:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:37 on 2024-12-13 08:30_

Annotated accepts an _arbitrary_ number of metadata elements after the first argument, no?

---

_@AlexWaygood reviewed on 2024-12-13 08:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4831 on 2024-12-13 08:31_

I don't think this is correct, per my comment above — you need at least 2 arguments to `Annotated`, but there's otherwise no restrictions. See e.g. the second bullet point at https://docs.python.org/3/library/typing.html#typing.Annotated, under "Details of the syntax"

---

_@AlexWaygood reviewed on 2024-12-13 08:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:37 on 2024-12-13 08:33_

We should add more tests for it though, for sure 

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4825 on 2024-12-13 12:14_

```suggestion
                    // Pyright also does this. Mypy doesn't; it falls back to `Any` instead.
```

---

_@AlexWaygood reviewed on 2024-12-13 12:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4845 on 2024-12-13 12:22_

(FYI, this is the same thing that caused a panic in the first version of https://github.com/astral-sh/ruff/pull/14858/, @InSyncWithFoo)

---

_@AlexWaygood reviewed on 2024-12-13 12:22_

---

_@InSyncWithFoo reviewed on 2024-12-13 12:23_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:33 on 2024-12-13 12:23_

The conversion is done in `Type::in_type_expression()`, but this has access to neither `.diagnostics` nor any `Expr`s. That method itself is called in three places within `TypeInferenceBuilder`, but it is not possible to emit a diagnostic based on the return value alone. Should I put a TODO there for now?

---

_@InSyncWithFoo reviewed on 2024-12-13 12:24_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:77 on 2024-12-13 12:24_

This is a runtime error, so I think it should always be reported, regardless of whether a diagnostic is emitted for `x: Annotated` or not.

---

_@AlexWaygood reviewed on 2024-12-13 12:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:327 on 2024-12-13 12:32_

It would be nice to avoid adding more TODO comments to this file. Can you add a short description of this check, similar to the one here:

https://github.com/astral-sh/ruff/blob/c3a64b44b7ab8742f4fe3630cd9751104d9f78d2/crates/red_knot_python_semantic/src/types/diagnostic.rs#L60-L77

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:26 on 2024-12-13 12:46_

you've added tests for very exotic kinds of metadata here, but I think we're still missing a simple test that's easy to read and understand, which demonstrates that we allow an arbitrary number of metadata elements without emitting a diagnostic or panicking. Just something like this:

```py
def _(x: Annotated[int, "arbitrary", "metadata", "elements", "are", "fine"]):
    reveal_type(x)  # revealed: int
```

---

_@AlexWaygood reviewed on 2024-12-13 12:46_

---

_Comment by @github-actions[bot] on 2024-12-13 12:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-12-13 12:56_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:28 on 2024-12-13 12:56_

Why should we emit an error on this? This works fine at runtime, on Python 3.9 and Python 3.13:

```pycon
>>> from typing import Annotated
>>> def f(x: Annotated[int, (foo := b"SyntaxError: named expression cannot be used within an annotation")]): ...
>>>
```

I'm not sure I understand what the value of this test is. If there's a syntax error that we're not detecting, that's either a bug in the parser or it's related to https://github.com/astral-sh/ruff/issues/11934. Neither is really something we need to test directly here. I think I would just delete this test; it seems confusing to me

---

_@InSyncWithFoo reviewed on 2024-12-13 13:03_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:28 on 2024-12-13 13:03_

I guess I have been living too much with 3.14. Replaced with a better one in which the metadata is also a type expression.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:348 on 2024-12-13 13:16_

It actually checks for invalid _type arguments_ to `Annotated` :-) Special forms _have_ type parameters; they _accept_ type arguments

```suggestion
declare_lint! {
    /// ## What it does
    /// Checks for invalid arguments to `typing.Annotated`.
    ///
    /// ## Why is this bad?
    /// `Annotated` expects at least two arguments.
    /// Otherwise, it will raise a runtime error.
    ///
    /// ## Example
    ///
    /// ```python
    /// x: Annotated[int]
    /// ```
    ///
    /// Use instead:
    ///
    /// ```python
    /// x: Annotated[int, GreaterThan(42)]
    /// ```
    pub(crate) static INVALID_ANNOTATED_ARGUMENTS = {
        summary: "detects invalid arguments to Annotated",
        status: LintStatus::preview("1.0.0"),
        default_level: Level::Error,
    }
}
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4819 on 2024-12-13 13:17_

```suggestion
                let mut report_invalid_arguments = || {
                    self.diagnostics.add_lint(
                        &INVALID_ANNOTATED_ARGUMENTS,
                        subscript.into(),
                        format_args!(
                            "Special form `{}` expected at least 2 arguments (one type and at least one metadata element)",
                            known_instance.repr(self.db)
                        ),
                    );
                };
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4823 on 2024-12-13 13:17_

```suggestion
                    // `Annotated[]` with less than two arguments is an error at runtime.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4843 on 2024-12-13 13:18_

we could use much more descriptive variable names here:

```suggestion
                let [type_expr, metadata @ ..] = &elts[..] else {
                    self.infer_type_expression(parameters);
                    return Type::Unknown;
                };

                for element in metadata {
                    self.infer_expression(element);
                }

                let ty = self.infer_type_expression(type_expr);
```

---

_@AlexWaygood reviewed on 2024-12-13 13:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:48 on 2024-12-13 13:48_

it's easy to miss that there's an assertion here when reading this; it gets a little lost in the multiline descriptive comment:

```suggestion
def _(x: Annotated[int]):  # error: [invalid-annotated-parameter]
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:27 on 2024-12-13 13:49_

this is a _correct_ spelling, but it's [much less common](https://library.bc.edu/answerwall/2019/08/15/is-parameterization-or-parametrization-correct-when-describing-which-parameters-describe-a-statistical-model/) than "parameterize"

```suggestion
It is invalid to parameterize `Annotated` with less than two arguments.
```


---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:54 on 2024-12-13 13:49_

```suggestion
### Correctly parameterized
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:69 on 2024-12-13 13:51_

```suggestion
### Not parameterized
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:330 on 2024-12-13 13:53_

```suggestion
    /// Otherwise, it will raise `TypeError` at runtime.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:342 on 2024-12-13 13:53_

```suggestion
    /// ```python
    /// from typing import Annotated
    ///
    /// x: Annotated[int]
    /// ```
    ///
    /// Use instead:
    ///
    /// ```python
    /// from typing import Annotated
    ///
    /// x: Annotated[int, "important metadata"]
    /// ```
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4838 on 2024-12-13 13:57_

```suggestion
                let ast::Expr::Tuple(ast::ExprTuple { elts: arguments, .. }) = parameters else {
                    report_invalid_arguments();

                    // `Annotated[]` with less than two arguments is an error at runtime.
                    // However, we still treat `Annotated[T]` as `T` here for the purpose of
                    // giving better diagnostics later on.
                    // Pyright also does this. Mypy doesn't; it falls back to `Any` instead.
                    return self.infer_type_expression(parameters);
                };

                if arguments.len() < 2 {
                    report_invalid_arguments();
                }

                let [type_expr, metadata @ ..] = &arguments[..] else {
                    self.infer_type_expression(parameters);
                    return Type::Unknown;
                };
```

Ideally we'd also rename the `let parameters = &*subscript.slice;` assignment on line 4807 to something like `let arguments_slice = &*subscript.slice;`. You don't have to do that in this PR, though, if you don't want to.

---

_@AlexWaygood reviewed on 2024-12-13 13:57_

---

_@carljm reviewed on 2024-12-13 14:08_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4831 on 2024-12-13 14:08_

Whoops, sorry for the noise! Missed that in reading the spec. Shows how much I've actually had occasion to use `Annotated` 😆 

---

_@carljm reviewed on 2024-12-13 14:09_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:37 on 2024-12-13 14:09_

Oops, my mistake.

---

_@InSyncWithFoo reviewed on 2024-12-13 14:13_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:27 on 2024-12-13 14:13_

Done. Also renamed the two in `literal_string.md`.

---

_@AlexWaygood reviewed on 2024-12-13 14:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:27 on 2024-12-13 14:19_

Thank you!

---

_@AlexWaygood reviewed on 2024-12-13 14:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:33 on 2024-12-13 14:32_

There are two ways to solve this TODO. One way would be to change `Type::in_type_expression` so that it returns a `Result`, where the error tells you what kind of diagnostic needs to be emitted if it's not a valid type expression. The other would be to move `Type::in_type_expression` so that it's a method on `TypeInferenceBuilder` rather than a method on `Type`, since the type inference builder has access to diagnostics. Either would be do the job; they both have advantages and disadvantages. Interested in hearing @carljm's thoughts on what he'd prefer.

---

_@carljm reviewed on 2024-12-13 14:52_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:33 on 2024-12-13 14:52_

Moving it to `TypeInferenceBuilder` seems easy, but we currently roughly maintain a split where methods on `TypeInferenceBuilder` accept AST nodes as input, not types; type-level operations go on `Type` and in `types.rs`. I think I'd prefer to maintain that?

I think there's a third option, which might be simplest: have `in_type_expression` take as argument `&mut self.diagnostics` when called from `TypeInferenceBuilder`. This looks like the direction we might be heading with #14956 anyway, and would be easy to convert to that.

---

_@AlexWaygood reviewed on 2024-12-13 14:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:33 on 2024-12-13 14:54_

> I think there's a third option, which might be simplest: have `in_type_expression` take as argument `&self.diagnostics` when called from `TypeInferenceBuilder`.

I wondered about that, but it felt a little ugly to have a method on `Type` mutate the state of an argument passed in like that :/

---

_@carljm reviewed on 2024-12-13 14:58_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:33 on 2024-12-13 14:58_

I think this might just be something we have to accept, that type operations can add diagnostics to the current inference context? I'm not sure it's nicer if every call to `in_type_expression` has to look like `some_type.in_type_expression(db).unwrap_with_diagnostic(&mut self.diagnostics)` instead.

---

_@AlexWaygood reviewed on 2024-12-13 15:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:33 on 2024-12-13 15:01_

Yes, fair enough. @InSyncWithFoo, can you make that change, then? `Type::in_type_expression` needs to accept an additional argument, of type `&mut TypeCheckDiagnostics`, so that it can emit diagnostics if the type isn't valid in a type expression

---

_@carljm reviewed on 2024-12-13 15:02_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:33 on 2024-12-13 15:02_

Oh, I've forgotten a key point, though -- reporting a diagnostic also requires a node (to report it on the right range). Having `Type::in_type_expression` also take an AST node is just as much a boundary-violation as having methods on `TypeInferenceBuilder` take types. So I feel like returning a `Result` is probably best, or at least most in line with our current structure. Though I'm not certain whether holding this boundary will be sustainable long-term.

---

_@AlexWaygood reviewed on 2024-12-13 15:04_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:33 on 2024-12-13 15:04_

In this case it feels complicated enough that I'm okay leaving the TODO here for now and accepting the PR as-is, if you are!

---

_@carljm reviewed on 2024-12-13 15:05_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/annotated.md`:33 on 2024-12-13 15:05_

Yes, I think that's fine. We need to sort out our strategy here, and I'll be working on that as part of call signature checking anyway.

---

_@AlexWaygood approved on 2024-12-13 15:06_

LGTM, thanks!

---

_Review requested from @carljm by @AlexWaygood on 2024-12-13 15:06_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1798 on 2024-12-13 15:10_

```suggestion
            # TODO should emit a diagnostic
            Type::KnownInstance(KnownInstanceType::Annotated) => Type::Unknown,
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:347 on 2024-12-13 15:15_

I'm not sure if we'll really want dedicated separate rules for invalid parameterization of each separate special form? Pyright and mypy both seem to lump these all together, pyright into `reportInvalidTypeForm` and mypy into `valid-type`. That would correspond to our `INVALID_TYPE_FORM`, but we also already have `INVALID_TYPE_PARAMETER` which is more specific. I would be inclined to just use `INVALID_TYPE_PARAMETER` and not create a new rule for this.

I guess we do have the precedent of `INVALID_LITERAL_PARAMETER`, but tbh I'd be inclined to remove that in favor of a more general rule.

@AlexWaygood I haven't reviewed the full discussion history of the PR; is a dedicated rule for this something you asked for in review? What do you think about just using `INVALID_TYPE_PARAMETER` instead?

We can also leave this for a later cleanup/rationalization of rules. (For instance, I like that this correctly uses the term "argument" instead of "parameter", I think we should fix that in other similar rules.)

---

_@carljm approved on 2024-12-13 15:20_

Looks good to me!

---

_@AlexWaygood reviewed on 2024-12-13 15:24_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:347 on 2024-12-13 15:24_

I left review comments on the name of the dedicated rule and the docs for it, but no, I didn't specifically ask for having a dedicated rule. Having a more general rule sounds fine to me. But it should be `INVALID_TYPE_ARGUMENTS` rather than `INVALID_TYPE_PARAMETER`, I think -- the _arguments to the parameters_ are where the invalidity is (as you yourself note!).

---

_@carljm reviewed on 2024-12-13 15:48_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:347 on 2024-12-13 15:48_

Ok, I think at the very least we should roll all three of `INVALID_ANNOTATED_ARGUMENTS`, `INVALID_LITERAL_PARAMETER`, and `INVALID_TYPE_PARAMETER` into a single rule `INVALID_TYPE_ARGUMENTS`.

I think a good case can be made that we should go one step further and just roll all of it into the single rule `INVALID_TYPE_FORM`, like mypy and pyright do. But I don't feel strongly about this.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:347 on 2024-12-13 16:18_

Ok, taking that thumbs-up as not-an-objection to the "step further", I'm going to suggest that that is in fact what we should do (roll everything into `INVALID_TYPE_FORM`). I just have a hard time imagining that someone wants to globally suppress "wrong type parameters" as an error class, but not other invalid type forms. And mypy and pyright's behavior suggests there hasn't been demand for this.

@InSyncWithFoo are you up for doing this in this PR? I'd probably prefer that, just so we don't add a new rule and then immediately remove it in another PR.

---

_@carljm reviewed on 2024-12-13 16:18_

---

_@InSyncWithFoo reviewed on 2024-12-13 16:29_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:347 on 2024-12-13 16:29_

You must be asking the wrong person, because I sure am.

Two additional notes, though:

1. We might want to differentiate diagnostics for runtime errors and for type-checking-only. `Annotated[int]` is a runtime error, but `Literal[object()]` isn't.
2. I think the rules should be ordered alphabetically, if only for human-searchability.

---

_@AlexWaygood approved on 2024-12-13 17:25_

---

_@carljm reviewed on 2024-12-13 17:30_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:347 on 2024-12-13 17:30_

Definitely no objection to ordering rules alphabetically!

Differentiating between diagnostics for things that cause a runtime exception vs things that don't is interesting. In some cases I think that's useful. When it comes to valid vs invalid uses of `typing` module special forms I think it's less useful, because how much "runtime error checking" is done by special forms in `typing` module is very arbitrary and ad-hoc, based mostly on what an implementer felt like doing and was easy to do. Often these behaviors are changed or relaxed if someone can present a good case for wanting to experiment in static analysis with some meaning for some currently-erroring form. So I don't think it's a useful distinction in this case.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:254 on 2024-12-13 17:39_

This is one way to get rid of TODOs!  :)

---

_@carljm approved on 2024-12-13 17:40_

Looks good, thank you!

---

_Merged by @carljm on 2024-12-13 17:41_

---

_Closed by @carljm on 2024-12-13 17:41_

---

_Branch deleted on 2024-12-13 17:50_

---

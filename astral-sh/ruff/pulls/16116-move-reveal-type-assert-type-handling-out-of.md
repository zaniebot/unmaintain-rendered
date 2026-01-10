```yaml
number: 16116
title: "Move `reveal_type`, `assert_type` handling out of `CallOutcome`"
type: pull_request
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
base: main
head: micha/call-outcome-step1
created_at: 2025-02-12T12:37:19Z
updated_at: 2025-02-14T14:41:01Z
url: https://github.com/astral-sh/ruff/pull/16116
synced_at: 2026-01-10T19:57:22Z
```

# Move `reveal_type`, `assert_type` handling out of `CallOutcome`

---

_Pull request opened by @MichaReiser on 2025-02-12 12:37_

## Summary

Let's measure the temperture on how people feel about this ;) 

Something I noticed about `CallOutcome` or more generally about `Type::call` is that we have to differentiate between three use cases:

1. We want to type check a call expression
2. We want to type check a possibly implicit call (e.g. `a += 1` where this calls `__add__`)
3. We are only interested in what the function would return if called 


This PR moves the code only relevant to 1 out of `CallOutcome` and into `infer_call_expression` instead. 
It simplifies the `CallOutcome` struct overall and I, as a reader, find it easier to reason about which logic 
applies where. 

## Test Plan

`cargo test`


---

_Label `red-knot` added by @MichaReiser on 2025-02-12 12:37_

---

_@MichaReiser reviewed on 2025-02-12 12:40_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call.rs`:242 on 2025-02-12 12:40_

I don't know how relevant this is and if it's worth supporting. I think the following would require the union handling:

```py
if coinflip():
	a = reveal_type
else:
	a = something_else

a(a) 
```

We, obviously could still support this by moving the union case handling into `infer_call_expression` too, but I honestly don't think it's worth the complexity (and there's no test)

Also note that we didn't support this for static assertions... so it was sort of inconsistent already

---

_Marked ready for review by @MichaReiser on 2025-02-12 12:45_

---

_Review requested from @carljm by @MichaReiser on 2025-02-12 12:45_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-12 12:45_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-12 12:45_

---

_Comment by @MichaReiser on 2025-02-12 12:46_

I don't plan on merging this immediately even if people think this is a good idea. I mainly want to get feedback on the direction and if, deemed acceptable, continue in the direction of reducing `Type::call` to only handle the 3rd case and see how that goes. 

---

_Comment by @MichaReiser on 2025-02-12 12:49_

To extend a bit on 3. I think it's useful if `Type::call` returns a struct that holds enough information to create a diagnostic from it. But I think we should remove all methods that take an `InferContext` from `CallOutcome` and move them somewhere into `infer_`

The reason I'm leaning in this direction is that I currently find it hard to decide which of the `CallOutcome` methods I have to use and why. 

Will this work, I don't know. 

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3273 on 2025-02-12 13:07_

you could possibly do one more early return here:

```suggestion
        };
        
        let Some(known_function) = function_type.known(self.db()) else {
            return;
        }

        match known_function {
            KnownFunction::RevealType => {
```

---

_@AlexWaygood approved on 2025-02-12 13:08_

This is an excellent simplification. Thank you.

> ```py
> if coinflip():
>     a = reveal_type
> else:
>     a = something_else
> 
> a(a)
> ```

Yeah, I don't think we need to support this. Neither mypy or pyright does, and it doesn't seem worth the extra complexity. There's no plausible use-case.

---

_Comment by @MichaReiser on 2025-02-12 15:13_

I'm looking into moving out `len` next. It's an interesting challenge on how to do that without duplicating all code

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3346 on 2025-02-12 15:19_

```suggestion
                if truthiness.is_always_true() {
                    return call;
                }

                if let Some(message) = message.into_string_literal().map(|s| &**s.value(self.db()))
                {
                    self.context.report_lint(
                        &STATIC_ASSERT_ERROR,
                        call_expression.into(),
                        format_args!("Static assertion error: {message}"),
                    );
                } else if parameter_ty == Type::BooleanLiteral(false) {
                    self.context.report_lint(
                        &STATIC_ASSERT_ERROR,
                        call_expression.into(),
                        format_args!("Static assertion error: argument evaluates to `False`"),
                    );
                } else if truthiness.is_always_false() {
                    self.context.report_lint(
                        &STATIC_ASSERT_ERROR,
                        call_expression.into(),
                        format_args!(
                            "Static assertion error: argument of type `{parameter_ty}` is statically known to be falsy",
                            parameter_ty=parameter_ty.display(self.db())
                        ),
                    );
                } else {
                    self.context.report_lint(
                        &STATIC_ASSERT_ERROR,
                        call_expression.into(),
                        format_args!(
                            "Static assertion error: argument of type `{parameter_ty}` has an ambiguous static truthiness",
                            parameter_ty=parameter_ty.display(self.db())
                        ),
                    );
                }
            }
```

---

_@AlexWaygood reviewed on 2025-02-12 15:19_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3255 on 2025-02-12 18:43_

The name `check_call` doesn't seem right here, as most of the important "call checking" happens outside this method, this method just handles special cases for a few particular known callables. I'm not sure what better name to suggest, which I think raises some question about whether this change adds clarity to the structure of the code. The right name also depends partly on what all we intend to ultimately try to move out here.

---

_@carljm reviewed on 2025-02-12 19:07_

I'm OK with this change as a standalone change (because it pulls some complexity out of `CallOutcome` and `Type::call`, in exchange for losing some precision that doesn't matter at all), but I don't see it as clarifying a broader direction for how we handle call checking or outcomes of type operations, which seems like maybe the opposite of your intent with the PR :)

Strictly speaking, this is a semantic regression, because it means we no longer "recognize" `reveal_type` and friends wherever we might happen to call them, but only if they are directly called, not as part of a union, and not in an implicit call. I don't think this matters at all in practice (there is no reasonable use case either for `reveal_type` to be in a union, or for something like `__bool__ = reveal_type` that would result in an implicit call to it), but it makes me question whether this generalizes at all beyond a few "special" functions where we are OK with being more limited.

I think this semantic "regression" corresponds to the lack of clarity for me about how the code structure is supposed to correlate to the Python semantics, and therefore also what we should name the new method. Before it was IMO more clear: `Type::call` and its return value should encapsulate all of the results, both in terms of needed diagnostics and in terms of return type, of calling any Python object. If we are going to split that up in some way, I don't yet understand the semantics of the intended split.

---

_Comment by @AlexWaygood on 2025-02-12 19:14_

> I'm OK with this change as a standalone change (because it pulls some complexity out of `CallOutcome` and `Type::call`, in exchange for losing some precision that doesn't matter at all), but I don't see it as clarifying a broader direction for how we handle call checking or outcomes of type operations

That's actually why I kinda like the PR. Figuring out a Big Picture for how to solve the overall problem is Hard. Breaking out somewhat isolated simplifications like this where they make sense makes it easier to analyse the rest of the problem and think about it in isolation.

---

_Comment by @MichaReiser on 2025-02-13 09:07_

> I'm OK with this change as a standalone change (because it pulls some complexity out of CallOutcome and Type::call, in exchange for losing some precision that doesn't matter at all), but I don't see it as clarifying a broader direction for how we handle call checking or outcomes of type operations, which seems like maybe the opposite of your intent with the PR :)

I'm also not sure where this goes long term but the second half of your comment already gives me some signal. So I still think this was useful :)

---

_Closed by @MichaReiser on 2025-02-14 14:41_

---

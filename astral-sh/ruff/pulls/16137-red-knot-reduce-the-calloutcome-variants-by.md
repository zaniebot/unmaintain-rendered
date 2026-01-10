```yaml
number: 16137
title: "[red-knot] Reduce the `CallOutcome` variants by removing `RevealedType`, `AssertType` and `StaticAssert`"
type: pull_request
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
base: main
head: micha/reduce-call-outcome-variants
created_at: 2025-02-13T10:17:34Z
updated_at: 2025-02-14T14:40:54Z
url: https://github.com/astral-sh/ruff/pull/16137
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Reduce the `CallOutcome` variants by removing `RevealedType`, `AssertType` and `StaticAssert`

---

_Pull request opened by @MichaReiser on 2025-02-13 10:17_

## Summary

An alternative to https://github.com/astral-sh/ruff/pull/16116

Instead of moving the checks out of `CallOutcome` entirely, move them from `Type::call` to `return_type_result`.

This also allows us to avoid some extra diagnostics in the cases where `reveal_type` (or any other of the known functions) is called incorrectly. 
This matches Pyright's behavior.

## Test Plan

`cargo test`




---

_Review requested from @carljm by @MichaReiser on 2025-02-13 10:17_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-13 10:17_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-13 10:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call.rs`:246 on 2025-02-13 12:55_

You can reduce the nesting quite a bit here:

```suggestion
                if !binding.has_binding_errors() {
                    match binding
                        .callable_type()
                        .into_function_literal()
                        .and_then(|function| function.known(context.db()))
                    {
                        // TODO: Should we skip those diagnostics if there's any binding error?
                        Some(KnownFunction::RevealType) => {
                            if let Some(revealed_type) = binding.one_parameter_type() {
                                context.report_diagnostic(
                                    node,
                                    DiagnosticId::RevealedType,
                                    Severity::Info,
                                    format_args!(
                                        "Revealed type is `{}`",
                                        revealed_type.display(context.db())
                                    ),
                                );
                            }
                        }
                        Some(KnownFunction::AssertType) => {
                            if let [actual_ty, asserted_ty] = binding.parameter_types() {
                                if !actual_ty.is_gradual_equivalent_to(context.db(), *asserted_ty) {
                                    context.report_lint(
                                        &TYPE_ASSERTION_FAILURE,
                                        node,
                                        format_args!(
                                            "Actual type `{}` is not the same as asserted type `{}`",
                                            actual_ty.display(context.db()),
                                            asserted_ty.display(context.db()),
                                        ),
                                    );
                                }
                            }
                        }
                        Some(KnownFunction::StaticAssert) => {
                            if let Some((parameter_ty, message)) = binding.two_parameter_types() {
                                let truthiness = parameter_ty.bool(context.db());

                                if !truthiness.is_always_true() {
                                    if let Some(message) = message
                                        .into_string_literal()
                                        .map(|s| &**s.value(context.db()))
                                    {
                                        context.report_lint(
                                            &STATIC_ASSERT_ERROR,
                                            node,
                                            format_args!("Static assertion error: {message}"),
                                        );
                                    } else if parameter_ty == Type::BooleanLiteral(false) {
                                        context.report_lint(
                                            &STATIC_ASSERT_ERROR,
                                            node,
                                            format_args!("Static assertion error: argument evaluates to `False`"),
                                        );
                                    } else if truthiness.is_always_false() {
                                        context.report_lint(
                                            &STATIC_ASSERT_ERROR,
                                            node,
                                            format_args!(
                                                "Static assertion error: argument of type `{parameter_ty}` is statically known to be falsy",
                                                parameter_ty=parameter_ty.display(context.db())
                                            ),
                                        );
                                    } else {
                                        context.report_lint(
                                            &STATIC_ASSERT_ERROR,
                                            node,
                                            format_args!(
                                                "Static assertion error: argument of type `{parameter_ty}` has an ambiguous static truthiness",
                                                parameter_ty=parameter_ty.display(context.db())
                                            ),
                                        );
                                    }
                                }
                            }
                        }
                        _ => {}
```

---

_@AlexWaygood approved on 2025-02-13 12:55_

---

_Label `red-knot` added by @AlexWaygood on 2025-02-13 18:46_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:168 on 2025-02-14 00:31_

Yes, I think it's fine to do that, would be fine to remove this TODO IMO. It's not always clear what the right reveal/assert diagnostic would even be if we had an error binding the call, and the binding error should tell the user what they need to fix in order to get the desired diagnostic.

---

_@carljm approved on 2025-02-14 00:32_

I like this a lot!!

---

_Closed by @MichaReiser on 2025-02-14 14:40_

---

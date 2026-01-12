```yaml
number: 13559
title: Add unary inference for integer and boolean literals
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/unary
created_at: 2024-09-30T01:49:09Z
updated_at: 2024-09-30T18:11:10Z
url: https://github.com/astral-sh/ruff/pull/13559
synced_at: 2026-01-12T15:55:44Z
```

# Add unary inference for integer and boolean literals

---

_@charliermarsh_

## Summary

Just trying to familiarize myself with the general patterns, testing, etc.

Part of https://github.com/astral-sh/ty/issues/244.


---

_Review requested from @carljm by @charliermarsh on 2024-09-30 01:49_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-09-30 01:49_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-09-30 01:49_

---

_Label `red-knot` added by @charliermarsh on 2024-09-30 01:49_

---

_Comment by @charliermarsh on 2024-09-30 01:49_

A few questions:

1. Is there a way to express type errors right now? E.g., `~None` is a type error.
2. Is there a float type?
3. How do you all test these changes during debug? Do you run the playground, or the CLI, etc.?

---

_Comment by @github-actions[bot] on 2024-09-30 02:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-09-30 10:03_

> ## 1. Is there a way to express type errors right now? E.g., `~None` is a type error.

Yup! Here's how we emit a diagnostic for when you try to iterate over something that's not iterable, for example:

https://github.com/astral-sh/ruff/blob/5118166d21784e7c78e38ea42919ba50bb2a5142/crates/red_knot_python_semantic/src/types/infer.rs#L1241-L1251

(I'll let you grep for places where that method is used ;)

> ## 2. Is there a float type?

There's no type in our semantic model representing _literal_ floats, no. (At least, not yet.) To say "this is an arbitrary instance of `float`", you can do something like this:

https://github.com/astral-sh/ruff/blob/5118166d21784e7c78e38ea42919ba50bb2a5142/crates/red_knot_python_semantic/src/types/infer.rs#L1620

The [typing spec](https://typing.readthedocs.io/en/latest/spec/literal.html#legal-parameters-for-literal-at-type-check-time) (drawing its language from [PEP 586](https://peps.python.org/pep-0586/#legal-parameters-for-literal-at-type-check-time)) states:

> `Literal` may be parameterized with literal ints, byte and unicode strings, bools, Enum values and `None`.

`float` isn't included there, so `Literal[3.14]` isn't valid in a type annotation according to the spec.

Should we go beyond the spec, and track the literal value of `float` variables anyway (where possible)? _Maybe_. Notably, mypy does a similar kind of tracking internally when it comes to float values -- but I think this is mostly so that it can constant-fold things like `X: Final = 3.14 ** 2` in mypyc. We don't have any equivalent of mypyc in red-knot, so I'm not sure what the use case is in red-knot for tracking the value of `float` variables _unless_ we were to allow users to write things like `Literal[3.14]` in type annnotations -- but that really would just flat-out contradict the spec, so I'd be pretty uncomfortable with allowing users to do that.

> ## 3. How do you all test these changes during debug? Do you run the playground, or the CLI, etc.?

Not sure I have a great answer there; I'm still figuring this out myself!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2214 on 2024-09-30 10:20_

Can't we use Rust's bitwise logical `not` operator here, to mirror Python's bitwise logical `not` operator?

```suggestion
            (UnaryOp::Invert, Type::IntLiteral(value)) => Type::IntLiteral(!value),
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2212 on 2024-09-30 10:26_

```suggestion
		let operand_ty = self.infer_expression(operand);

        match (op, operand_ty) {
            (UnaryOp::UAdd, Type::IntLiteral(_)) => operand_ty,
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2222 on 2024-09-30 10:27_

nit: I'd probably just inline these

```suggestion
            (UnaryOp::UAdd, Type::BooleanLiteral(bool)) => Type::IntLiteral(i64::from(bool)),
            (UnaryOp::USub, Type::BooleanLiteral(bool)) => Type::IntLiteral(-i64::from(bool))
            (UnaryOp::Invert, Type::BooleanLiteral(bool)) => Type::IntLiteral(!i64::from(bool))
            (UnaryOp::Not, ty) => ty.bool(self.db).negate().into_type(self.db),
            _ => Type::Unknown, // TODO other unary op types
```

---

_@AlexWaygood approved on 2024-09-30 10:28_

Nice!

---

_@AlexWaygood reviewed on 2024-09-30 10:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2227 on 2024-09-30 10:39_

I'm not sure this needs to be done now (and probably it actually _shouldn't_ be done now), but at some point we'll want to emit some kind of diagnostic here. It's currently deprecated at runtime. Whether it will remain deprecated is an [open question](https://discuss.python.org/t/bool-deprecation/62232), but it's nonetheless true that typeshed [decorates `bool.__invert__` with `@deprecated`](https://github.com/AlexWaygood/typeshed/blob/9f033bf439e064917e4b235b8a09b7530f182516/stdlib/builtins.pyi#L927-L928) in its `builtins` stub, which means that (once we support `@deprecated`) we'll emit a diagnostic when users attempt to invert a non-literal boolean value. For consistency, we'll want to make sure we emit the same diagnostic for literal `bool`s as we do for non-literal `bool`s, but I'm not sure how we want to do that. (Should we check the typeshed `bool` stub for each method when we're doing type inference on literal `bool`s, in case the `bool` method is decorated with `@deprecated` or similar? Or should we copy out the same deprecation message here, and hope that we can manually keep them in sync? Curious for @carljm's thoughts!)

TL;DR: let's add a TODO comment noting that eventually this should probably cause us to emit a diagnostic due to the deprecation!

---

_Merged by @charliermarsh on 2024-09-30 16:29_

---

_Closed by @charliermarsh on 2024-09-30 16:29_

---

_Branch deleted on 2024-09-30 16:29_

---

_@carljm reviewed on 2024-09-30 18:09_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2227 on 2024-09-30 18:09_

I think we'll probably want to get the deprecation message from calling the typeshed method, once we support `@deprecated`.

---

_Comment by @carljm on 2024-09-30 18:11_

> 3\. How do you all test these changes during debug? Do you run the playground, or the CLI, etc.?

I usually do most of my debugging via tests, not via CLI. There is no red-knot playground yet.

---

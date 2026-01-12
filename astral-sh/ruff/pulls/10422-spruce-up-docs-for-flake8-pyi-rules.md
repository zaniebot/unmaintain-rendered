```yaml
number: 10422
title: Spruce up docs for flake8-pyi rules
type: pull_request
state: merged
author: AlexWaygood
labels:
  - documentation
assignees: []
merged: true
base: main
head: flake8-pyi-docs
created_at: 2024-03-15T13:47:34Z
updated_at: 2024-03-20T16:00:32Z
url: https://github.com/astral-sh/ruff/pull/10422
synced_at: 2026-01-12T15:55:32Z
```

# Spruce up docs for flake8-pyi rules

---

_@AlexWaygood_

This started off as an attempt to clarify some of the confusion in https://github.com/astral-sh/ruff/issues/9810#issuecomment-1999339083 and https://github.com/astral-sh/ruff/issues/9810#issuecomment-1999349880, but I figured I would take a more comprehensive look at the docs for the flake8-pyi rules while I was about it.

Summary of the changes:

- Improve clarity over the motivation for some rules
- Improve links to external references. In particular, reduce links to PEPs, as PEPs are generally historical documents rather than pieces of living documentation. Where possible, it's better to link to the [official typing spec](https://typing.readthedocs.io/en/latest/spec/), the other docs at https://typing.readthedocs.io/en/latest/, or the docs at https://docs.python.org/3/library/typing.html.
- Add some missing imports to code examples
- Use more concise language in a few places

---

_Comment by @github-actions[bot] on 2024-03-15 14:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @AlexWaygood on 2024-03-18 12:58_

I haven't reviewed the docs for all the flake8-pyi rules yet -- only up to `redundant_numeric_union.rs` -- but I'll cut scope here and tackle the others in a separate PR, so that this is easier to review.

---

_Marked ready for review by @AlexWaygood on 2024-03-18 12:59_

---

_Label `documentation` added by @MichaReiser on 2024-03-18 16:03_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/any_eq_ne_annotation.rs`:24 on 2024-03-18 16:05_

This is probably on me but I don't understand the relation between "`==` or` !=` should never raise an exception" and why the method should then use `object` rather than `Any`. How does using `object` guards against exceptions (other than that it might catch a few more typing errors)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/collections_named_tuple.rs`:21 on 2024-03-18 16:08_

```suggestion
/// However, using `typing.NamedTuple` allows you to provide a type annotation
/// for each field in the class. This means that type checkers will have
/// more information to work with, and will be able to analyze your code
/// more precisely.
```

Maybe we can rephrase it to say that `namedtuple` types all fields as `Any` (I guess?) and that `typing.NamedTuple` allows you to specify the field types instead.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/complex_assignment_in_stub.rs`:44 on 2024-03-18 16:09_

Noob question. Is this preferred over `type X = int` or is that syntax not allowed in classes/stubs?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/future_annotations_in_stub.rs`:15 on 2024-03-18 16:19_

This might be correct but it's weird that the `.` is inside of the parentheses :sweat_smile: 

```suggestion
/// type checkers). As such, the `from __future__ import annotations` import
```


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/iter_method_return_iterable.rs`:20 on 2024-03-18 16:21_

I would probably remove the second "in Python", assuming that's clear from a python linter.
```suggestion
/// `__iter__` methods are expected to return `Iterator`s. Type
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/iter_method_return_iterable.rs`:39 on 2024-03-18 16:24_

What I understood from the description "by calling `iter` on the list" is that you write `y = x.iter()`. 

Maybe that's because of my German background but "on" sounds like it's a method on list. It might already be sufficient to change the example to `iter(...)` or `iter(x)` or use another word than `on`.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/iter_method_return_iterable.rs`:50 on 2024-03-18 16:24_

Nit: I'm okay with either.

```suggestion
/// on the returned object, violating the contract of the interface.
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/no_return_argument_annotation.rs`:18 on 2024-03-18 16:27_

I think this misses an explanation of why it's more idiomatic (or a preferred stylistic choice) to use  `Never` over `NoReturn`. I assume it's mainly that `NoReturn` is intended for function return types and everything else should use `Never`?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:37 on 2024-03-18 16:30_

Shouldn't this be `str`?

> For example, `Literal["A"] | str` is equivalent to `str`, and

---

_@MichaReiser approved on 2024-03-18 16:31_

Nice improvements. I learned a couple of things by reading through your new documentation.

I left a few Python noob understand comments

---

_@AlexWaygood reviewed on 2024-03-18 16:46_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/iter_method_return_iterable.rs`:50 on 2024-03-18 16:46_

I weakly prefer "expectations of the interface", so I'll stick with my current wording here 👍

---

_@AlexWaygood reviewed on 2024-03-18 16:50_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/any_eq_ne_annotation.rs`:24 on 2024-03-18 16:50_

Basically, for any two objects `x` and `y`, `x == y` should _never_ raise an exception -- calling `x.__eq__(y)` should be safe regardless of what the types are. That means that `object` is the better type here, as it's Python's true "top type" from a type theory perspective.

`Any` should be reserved for situations where you either want to allow unsafe behaviour for some reason, or for situations where your code is safe, but the true type that a function accepts is inexpressible. That should never be the case for `__eq__` or `__ne__` methods -- `object` should do just fine in all cases.

Now, how can I put that more succinctly 😅

---

_@AlexWaygood reviewed on 2024-03-18 16:53_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/complex_assignment_in_stub.rs`:44 on 2024-03-18 16:53_

`type X = int` is allowed in classes/stubs, but it's new syntax that's only available on Python 3.12+ -- I think it's better to use syntax here that all users of Python will be able to use

---

_@AlexWaygood reviewed on 2024-03-18 16:54_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/future_annotations_in_stub.rs`:15 on 2024-03-18 16:54_

The rule is that if it's a complete sentence inside the parentheses, the period goes inside the parentheses, but if it's only part of a sentence inside the parentheses, the period goes outside: https://www.masterclass.com/articles/period-inside-or-outside-parentheses

---

_@AlexWaygood reviewed on 2024-03-18 16:59_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/no_return_argument_annotation.rs`:18 on 2024-03-18 16:59_

It's basically just because it's really confusing to see a type called `NoReturn` in a parameter annotation -- that's the only reason we added the `Never` type in Python 3.11 😄 It has exactly the same semantics as `Noreturn`.

I'll try to flesh this out a little more...

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:37 on 2024-03-18 16:59_

Whoops, good catch. This should be `Literal[b"B"] | str`.

---

_@AlexWaygood reviewed on 2024-03-18 16:59_

---

_@AlexWaygood reviewed on 2024-03-18 17:57_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/collections_named_tuple.rs`:21 on 2024-03-18 17:57_

> Maybe we can rephrase it to say that `namedtuple` types all fields as `Any` (I guess?)

That's correct, but I wonder if we really need to go into that much detail here... I think simply saying that it will make type-checking more accurate is okay for our purposes here, and helps avoid too much information overload

---

_@AlexWaygood reviewed on 2024-03-18 18:01_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/iter_method_return_iterable.rs`:39 on 2024-03-18 18:01_

You're right that this was unclear -- I changed the wording from

```
calling `iter()` on the list
```

to

```
passing the list to `iter()`
````

---

_Merged by @AlexWaygood on 2024-03-18 18:03_

---

_Closed by @AlexWaygood on 2024-03-18 18:03_

---

_Branch deleted on 2024-03-20 16:00_

---

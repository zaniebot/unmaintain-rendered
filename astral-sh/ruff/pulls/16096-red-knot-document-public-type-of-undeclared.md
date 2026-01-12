```yaml
number: 16096
title: "[red-knot] Document 'public type of undeclared symbols' behavior"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/public-type-undeclared
created_at: 2025-02-11T11:27:17Z
updated_at: 2025-02-12T07:52:13Z
url: https://github.com/astral-sh/ruff/pull/16096
synced_at: 2026-01-12T15:55:53Z
```

# [red-knot] Document 'public type of undeclared symbols' behavior

---

_@sharkdp_

## Summary

After I was asked twice within the same day, I thought it would be a good idea to write some *user facing* documentation that explains our reasoning behind inferring `Unknown | T_inferred` for public uses of undeclared symbols. This is a major deviation from the behavior of other type checkers and it seems like a good practice to defend our choice like this. Please let me know if I missed something or if you see a chance to explain this more clearly.


[Rendered version](https://github.com/astral-sh/ruff/blob/david/public-type-undeclared/crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md)

---

_Label `red-knot` added by @sharkdp on 2025-02-11 11:27_

---

_Review requested from @carljm by @sharkdp on 2025-02-11 11:27_

---

_Review requested from @MichaReiser by @sharkdp on 2025-02-11 11:27_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-02-11 11:27_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md`:24 on 2025-02-11 11:30_

nit: "raising an error" to me makes it sound to me like we'll panic; I prefer to talk about "emitting an error"/"issuing a diagnostic".

(This might just be because I'm used to the Python context where there's a `raise` keyword, though

)
```suggestion
Mypy and Pyright both infer a type of `None` for the type of `wrapper.value`. Consequently, both
tools emit an error when trying to assign `1` to `wrapper.value`. But there is nothing wrong with
this program. Emitting an error here violates the [gradual guarantee] which states that *"Removing
type annotations (making the program more dynamic) should not result in additional static type
errors."*: If `value` were annotated with `int | None` here, Mypy and Pyright would not emit any
errors.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md`:43 on 2025-02-11 11:32_

You could also use this as an additional example, which might be slightly more "realistic" since users often leave local variables unannotated:

```suggestion
    y: int = w.value

    z = w.value + 42  # error
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md`:48 on 2025-02-11 11:33_

```suggestion
In the first example, we demonstrated how our behavior prevents false positives. However, it can also
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md`:50 on 2025-02-11 11:33_

```suggestion
To make this a bit more realistic, imagine that `OptionalInt` is imported from an external, untyped
```

---

_@AlexWaygood reviewed on 2025-02-11 11:34_

Thank you, this is great

---

_@sharkdp reviewed on 2025-02-11 11:37_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md`:43 on 2025-02-11 11:37_

That's similar to what I wanted to demonstrate here at first, but unfortunately, we don't detect that `None + 42` is an error, yet.

---

_@AlexWaygood reviewed on 2025-02-11 11:38_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md`:43 on 2025-02-11 11:38_

ah, we should probably fix that 🙃

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md`:102 on 2025-02-11 11:44_

```suggestion
Red-knot models all symbols as having two types: a "public type" and a "local type". The "local type"
is the type the symbol has from the perspective of code that exists in the same scope the symbol
was originally defined in. We try to infer as precise as possible a type for the local type, as we
can statically enumerate all assignments to the symbol within that scope. The "public type" of
a symbol, on the other hand, is the type the symbol has from the perspective of code in external scopes. For example:
```

---

_@AlexWaygood approved on 2025-02-11 11:44_

---

_@sharkdp reviewed on 2025-02-11 12:28_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md`:102 on 2025-02-11 12:28_

> Red-knot models all symbols as having two types: a "public type" and a "local type".

Hm. This might be how it looks like from the perspective of a user, but I'm not sure if that's the most helpful mental model. I think it might be better to think in terms of the inferred type and the declared type, and then to understand how those are used to construct "local" and "public" types (https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/resources/mdtest/boundness_declaredness/public.md)?

I think the suggested description also seems to imply that there are only two types for each symbol. The public and the local type. In reality, it really depends on the *use* of a symbol. Two "local" uses of `x` could see completely different types due to narrowing, for example.

> The "public type" of a symbol, on the other hand, is the type the symbol has from the perspective of code in external scopes.

Here, it might be good to explain *why* we need a (seemingly!) less precise type for this case. This is what I tried to achieve with the "coud have been reassigned" explanation above.

---

_@AlexWaygood reviewed on 2025-02-11 13:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md`:102 on 2025-02-11 13:31_

> Hm. This might be how it looks like from the perspective of a user, but I'm not sure if that's the most helpful mental model. I think it might be better to think in terms of the inferred type and the declared type, and then to understand how those are used to construct "local" and "public" types ([`main`/crates/red_knot_python_semantic/resources/mdtest/boundness_declaredness/public.md](https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/resources/mdtest/boundness_declaredness/public.md?rgh-link-date=2025-02-11T12%3A28%3A07Z))?

Heh. And I think I'll throw that right back at you and say that, while this mental model is the most accurate in terms of our _implementation_ (and therefore probably a better mental model for us to have), I'm not sure it's so helpful for users!

> I think the suggested description also seems to imply that there are only two types for each symbol. The public and the local type. In reality, it really depends on the _use_ of a symbol. Two "local" uses of `x` could see completely different types due to narrowing, for example.

Thanks, this is a great point.

What about something like this?

```suggestion
Red-knot applies different semantics depending on whether a symbol is accessed from the same scope
in which it was originally defined, or whether it is accessed from an external scope. External
scopes will see the symbol's "public type", which has been discussed above. But within the same
scope the symbol was defined in, we use a narrower type of `T_inferred` for undeclared symbols.
This is because, from the perspective of this scope, there is no way that the value of the symbol
could have been reassigned from external scopes. For example:
```

---

_@sharkdp reviewed on 2025-02-11 14:35_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md`:102 on 2025-02-11 14:35_

> Red-knot

I see what you did there. You thought I wouldn't notice.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md`:32 on 2025-02-11 17:36_

```suggestion
knowledge about this type. In the example above, we don't know what values `wrapper.value` could
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md`:43 on 2025-02-11 17:37_

I think it could also be OK to still use the more realistic example, with a TODO for us to add the binop checking.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md`:82 on 2025-02-11 17:51_

This is the weakest section, and I think that's because it is asserting something that kind of isn't true :) The core behavior of the dynamic type is that it avoids false positives but allows false negatives, because it is permissive. So I don't think that inferring dynamic types in unannotated code can really be said to prevent false negatives.

I think a better framing is that it prevents type checkers from inferring confident-but-wrong types from untyped code. Neither we nor mypy nor pyright will catch the bug in this usage of an untyped API. But mypy and pyright will claim to be confident about types that they are simply wrong about, whereas we will accurately reflect our lack of knowledge.

One concrete way this can benefit users is if we also provide tooling to detect the prevalance of dynamic types in your code. In this example, pyright and mypy will tell you that your code is typed, but they'll just be wrong about the types. We will tell you, accurately, that there are dynamic types here, due to the untyped API you are using.

---

_@carljm approved on 2025-02-11 17:51_

This is excellent, thanks for writing it up!

---

_@carljm reviewed on 2025-02-11 17:53_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md`:95 on 2025-02-11 17:53_

Instead of (or in addition to) using a `reveal_type` here, we could explicitly demonstrate an assignment of some incompatible type failing

---

_@sharkdp reviewed on 2025-02-11 22:03_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md`:82 on 2025-02-11 22:03_

Yes, fully agreed; thanks for the suggestion with the dynamic-type-detection mode. I rephrased this section now. Please let me know if I'm using "unsoundness" incorrectly here, or if it sounds overly dramatic.

---

_@sharkdp reviewed on 2025-02-11 22:03_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md`:95 on 2025-02-11 22:03_

Done.

---

_@sharkdp reviewed on 2025-02-11 22:09_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md`:43 on 2025-02-11 22:09_

I really wanted this document to be user-facing, so I would prefer if it were free of TODOs. I now used a `chr(w.value)` call to demonstrate that we would emit a *"`Unknown | None` cannot be assigned to parameter 1 (`i`) of function `chr`; expected type `int`"* diagnostic.

I used `chr` (as opposed to something more prominent) because it is one of very few functions in `builtins` that "just takes an `int`" as a parameter, and not some complicated generic protocol :upside_down_face: 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md`:79 on 2025-02-11 22:11_

```suggestion
`o.value`. Together with a possible future type-checker mode that would detect the prevalence of
```

---

_@carljm approved on 2025-02-11 22:11_

Looks great!

---

_@AlexWaygood reviewed on 2025-02-11 22:36_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md`:43 on 2025-02-11 22:36_

> I used `chr` (as opposed to something more prominent) because it is one of very few functions in `builtins` that "just takes an `int`" as a parameter, and not some complicated generic protocol 🙃

Oh no, that... Seems like it might be a typeshed bug 😆

```pycon
>>> class NotAnInt:
...   def __index__(self): return 42
...   
>>> chr(NotAnInt())
'*'
```

I can keep quiet about it until red-knot's implemented generics and protocols, though 😉

---

_@carljm reviewed on 2025-02-11 22:49_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md`:43 on 2025-02-11 22:49_

Couldn't you just as well define your own function that "just takes an int" instead of needing to use something from builtins?

---

_@dhruvmanila approved on 2025-02-12 05:34_

This is excellent, thank you!

---

_@sharkdp reviewed on 2025-02-12 07:44_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md`:43 on 2025-02-12 07:44_

> Couldn't you just as well define your own function that "just takes an int"

Yes, I did that now. I wanted to keep it short, but I shouldn't trade brevity for clarity.

> Seems like it might be a typeshed bug 😆

Oh :smile:

https://github.com/python/typeshed/pull/13494

---

_Merged by @sharkdp on 2025-02-12 07:52_

---

_Closed by @sharkdp on 2025-02-12 07:52_

---

_Branch deleted on 2025-02-12 07:52_

---

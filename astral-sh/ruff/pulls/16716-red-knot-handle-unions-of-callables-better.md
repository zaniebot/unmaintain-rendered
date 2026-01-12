```yaml
number: 16716
title: "[red-knot] Handle unions of callables better"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/union-callables
created_at: 2025-03-13T20:37:29Z
updated_at: 2025-03-17T14:35:55Z
url: https://github.com/astral-sh/ruff/pull/16716
synced_at: 2026-01-12T15:55:56Z
```

# [red-knot] Handle unions of callables better

---

_@dcreager_

This cleans up how we handle calling unions of types.  astral-sh/ruff#16568 adding a three-level structure for callable signatures (`Signatures`, `CallableSignature`, and `Signature`) to handle unions and overloads.

This PR updates the bindings side to mimic that structure.  What used to be called `CallOutcome` is now `Bindings`, and represents the result of binding actual arguments against a possible union of callables.  `CallableBinding` is the result of binding a single, possibly overloaded, callable type.  `Binding` is the result of binding a single overload.

While we're here, this also cleans up `CallError` greatly.  It was previously extracting error information from the bindings and storing it in the error result.  It is now a simple enum, carrying no data, that's used as a status code to talk about whether the overall binding was successful or not.  We are now more consistent about walking the binding itself to get detailed information about _how_ the binding was unsucessful.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2167 on 2025-03-14 01:24_

This has to be separated out into two match arms since the `Err` outcome is boxed and the `Ok` one is not.

---

_Comment by @github-actions[bot] on 2025-03-14 01:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2365 on 2025-03-14 01:33_

The `signatures` function as a whole is now tracked, instead of tracking each of these individual special case clauses.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:3992 on 2025-03-14 01:41_

These lints are now handled by `Bindings::report_diagnostics`, which means that all call-site-related lints are now in the same place.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/conditional/match.md`:51 on 2025-03-14 01:44_

Without these changes, we infer the type of `__bool__` to be `int | Unknown`. Only one of the union branches is non-callable, which changes the content of the error message below. I decided to add the type annotations instead of updating the expected error messages, since this seems to more accurately reflect the intent of these `NotBoolable` types.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/expression/boolean.md`:138 on 2025-03-14 01:45_

Here `__bool__` is a union, but unlike above, both elements are non-callable, so we treat the union as a whole as non-callable.

---

_@dcreager reviewed on 2025-03-14 01:45_

---

_Marked ready for review by @dcreager on 2025-03-14 01:45_

---

_Review requested from @carljm by @dcreager on 2025-03-14 01:45_

---

_Review requested from @MichaReiser by @dcreager on 2025-03-14 01:45_

---

_Review requested from @AlexWaygood by @dcreager on 2025-03-14 01:45_

---

_Review requested from @sharkdp by @dcreager on 2025-03-14 01:45_

---

_Renamed from "[red-knot] [WIP] Handle unions of callables better" to "[red-knot] Handle unions of callables better" by @dcreager on 2025-03-14 01:46_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2318 on 2025-03-14 07:48_

Does making this a salsa query help with performance? It's one of the cases where salsa has to do one extra interning for the cache key (because the key is a combination of `self` and `Type`. 

Should we instead intern `Signatures`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2636 on 2025-03-14 07:51_

```suggestion
        for binding in bindings.bindings_mut() {
```

This reads very repetitively. Should it be renamed to `iter_mut`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/bind.rs`:91 on 2025-03-14 07:54_

Nit: Would it amke sense to track a boolean flag whether we've seen any binding with errors and only perform the second iteration if the flag is `true`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/bind.rs`:140 on 2025-03-14 07:55_

Nit: `bindings.bindings()` reads very repetitively on the call site. I suggest going with `iter`, `iter_mut` and implementing `IntoIterator for &Bindings`

Oh, I see now that we return a slice. Do we need to return a slice or is an iterator enough (`std::slice::Iter`)?

If it has to be a slice, then I'm leaning towards `as_slice` and `as_slice_mut` and/or implementing `Deref` and `DerefMut`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2633 on 2025-03-14 08:02_

I overall like the change, but I'm a bit concerned about `call` returning `Bindings` directly and relying on the caller's *good behavior* to only call `bindings.return_type` if they're okay ignoring any possible binding error. It somewhat undoes what I tried to accomplish with my `try_` refactor to make all error handling explicit. 

The method name also doesn't fit into the `try_` for faliable operations naming-convention anymore. It should also simplify the call-sites because every call site immediately calls `as_result`. 

I very much like the outcome that bindings now stores the relevant information.
A possible solution here is to change `CallError` to a struct with two fields: `Bindings` and `kind` where kind is what is `CallError` in your PR. 



---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2319 on 2025-03-14 08:11_

Similar to `call`. I think this is going in a slightly different direction to what I started with my `try_` work where error handling is no longer explicit but implicit again. I'm not tied to that approach but what my PR showed is that implicit error handling is very easy to get wrong and it's very tempting to call methods directly on `Signatures` if they're directly available. 


That's why I'm wondering of what your reasoning was to not return a `Result<Signatures, SignaturesErr>` here. I suspect it has to do with the union error handling where the `Result` doesn't compose well? 

---

_@MichaReiser reviewed on 2025-03-14 08:12_

---

_Label `red-knot` added by @MichaReiser on 2025-03-14 08:12_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2633 on 2025-03-14 12:01_

Yeah I was going back on forth on that.  I had thought that some callers would want to inspect the bindings before converting to a result.  But no one is doing that, and even if they did, I can probably add a helper method to make that easier.

If you look a couple of commits back that's sort of how it was...though the lower layer bindings need to return a `CallError` without the attached `Bindings`, so I had it with a parameterized type...  Anyway I think I can pull out a `CallErrorKind` to make this work.  Will try that now

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2319 on 2025-03-14 12:07_

> I suspect it has to do with the union error handlin

Yes that's right!  This originally returned `Option<Signatures>`, since the only "error" condition is that the type isn't callable, and therefore doesn't really have a signature.

But with unions, you can have some element types that are callable and some that are not.  So we already need the signature types to track "callable or not" internally, and once we have that, `Signatures::not_callable` works for that "error" condition just as well as `None`.

So every type now has a `Signatures` object.  And it's perfectly fine to try to create a `Bindings` for it at a call site, even if the type isn't actually callable.  The resulting `Bindings` (and per your above comment, the `Err` that you'll get back from that `try_call` call) will tell you that the type isn't actually callable.  (And if you _want_ to skip the `try_call` for types that are completely non-callable, there's an `is_callable` method that you can call.)

I can add some more documentation about this.

---

_@dcreager reviewed on 2025-03-14 12:07_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2319 on 2025-03-14 12:33_

Makes sense. I also think that returning a `Result` here is less important than for `call` as it's *more internal* and `Signatures` provides less helpful methods (e.g. no `return_type`). So I think it's different enough that it is okay to diverge from the pattern we use for other type operations and documentation should help to make this clear

---

_@MichaReiser reviewed on 2025-03-14 12:33_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2318 on 2025-03-14 14:53_

Before we [were using salsa queries to cache the signatures](https://github.com/astral-sh/ruff/pull/16568/#discussion_r1988118843) we were creating by hand for special-case calls, since the contents of the signature don't depend on the arguments of each call site anymore.  I had done this to try to simplify that caching.

I like your suggestion better, though, so I've done that.  I'm just tracking the signatures, not interning them, since we don't need the faster equality comparison.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2636 on 2025-03-14 14:56_

Changed to `iter_mut`

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:140 on 2025-03-14 14:56_

Ditto other comment, made this `iter`

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:91 on 2025-03-14 14:58_

I don't think it will matter for performance, since we shouldn't have hundreds or thousands of union elements. So I'd rather decide this based on code readability. Do you feel like it reads better with the flag and skipping the second loop if we can?

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2633 on 2025-03-14 14:59_

Done

---

_@dcreager reviewed on 2025-03-14 14:59_

---

_Comment by @codspeed-hq[bot] on 2025-03-14 15:04_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Funion-callables)

### Merging astral-sh/ruff#16716 will **not alter performance**

<sub>Comparing <code>dcreager/union-callables</code> (e460c80) with <code>main</code> (75a562d)</sub>



### Summary

`✅ 32` untouched benchmarks  





---

_@dcreager reviewed on 2025-03-14 15:25_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2318 on 2025-03-14 15:25_

Tracking (and interning, tried that too) `Signatures` adds the performance regression that Codspeed is showing.  Which thinking through it, makes sense, since when tracking the `signatures` function, the cache key is just two u32s (`self` and `Type`).  But tracking the struct means that we have to do a hash lookup based on the content of the more complex `Signatures` type.  (Unless I'm misunderstanding the underlying salsa machinery!)

---

_@MichaReiser reviewed on 2025-03-14 15:43_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/bind.rs`:91 on 2025-03-14 15:43_

> Do you feel like it reads better with the flag and skipping the second loop if we can?

that's very unlikely because it introduces more control flow and not less 😆 

---

_@MichaReiser reviewed on 2025-03-14 15:45_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2318 on 2025-03-14 15:45_

Makes sense. Thanks for trying. The main finding here is that the extra allocations for repeated signatures matter less. 

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2763 on 2025-03-14 17:11_

I would prefer to do inner matches over the value of `function.known()` and `class.known()` in this method, rather than having lots of branches with `if function.is_known()` and `if class.is_known()` guards. The `is_known()` functions each do a Salsa lookup, which can be surprisingly expensive; it's better to just do the lookup once and then have a `match` over it rather than doing it repeatedly in each `match` branch:

```rs
                Type::FunctionLiteral(function) => match function.known(db) {
                    Some(KnownFunction::IsEquivalentTo) => {
                        if let [ty_a, ty_b] = overload.parameter_types() {
                            overload.set_return_type(Type::BooleanLiteral(
                                ty_a.is_equivalent_to(db, *ty_b),
                            ));
                        }
                    }

                    Some(KnownFunction::IsSubtypeOf) => {
                        if let [ty_a, ty_b] = overload.parameter_types() {
                            overload.set_return_type(Type::BooleanLiteral(
                                ty_a.is_subtype_of(db, *ty_b),
                            ));
                        }
                    }

                    Some(KnownFunction::IsAssignableTo) => {
                        if let [ty_a, ty_b] = overload.parameter_types() {
                            overload.set_return_type(Type::BooleanLiteral(
                                ty_a.is_assignable_to(db, *ty_b),
                            ));
                        }
                    }

                    Some(KnownFunction::IsDisjointFrom) => {
                        if let [ty_a, ty_b] = overload.parameter_types() {
                            overload.set_return_type(Type::BooleanLiteral(
                                ty_a.is_disjoint_from(db, *ty_b),
                            ));
                        }
                    }

                    Some(KnownFunction::IsGradualEquivalentTo) => {
                        if let [ty_a, ty_b] = overload.parameter_types() {
                            overload.set_return_type(Type::BooleanLiteral(
                                ty_a.is_gradual_equivalent_to(db, *ty_b),
                            ));
                        }
                    }

                    Some(KnownFunction::IsFullyStatic) => {
                        if let [ty] = overload.parameter_types() {
                            overload.set_return_type(Type::BooleanLiteral(ty.is_fully_static(db)));
                        }
                    }

                    Some(KnownFunction::IsSingleton) => {
                        if let [ty] = overload.parameter_types() {
                            overload.set_return_type(Type::BooleanLiteral(ty.is_singleton(db)));
                        }
                    }

                    Some(KnownFunction::IsSingleValued) => {
                        if let [ty] = overload.parameter_types() {
                            overload.set_return_type(Type::BooleanLiteral(ty.is_single_valued(db)));
                        }
                    }

                    Some(KnownFunction::Len) => {
                        if let [first_arg] = overload.parameter_types() {
                            if let Some(len_ty) = first_arg.len(db) {
                                overload.set_return_type(len_ty);
                            }
                        };
                    }

                    Some(KnownFunction::Repr) => {
                        if let [first_arg] = overload.parameter_types() {
                            overload.set_return_type(first_arg.repr(db));
                        };
                    }

                    Some(KnownFunction::Cast) => {
                        // TODO: Use `.parameter_types()` exclusively when overloads are supported.
                        if let Some(casted_ty) = arguments.first_argument() {
                            if let [_, _] = overload.parameter_types() {
                                overload.set_return_type(casted_ty);
                            }
                        };
                    }

                    Some(KnownFunction::Overload) => {
                        overload.set_return_type(todo_type!("overload(..) return type"));
                    }

                    Some(KnownFunction::GetattrStatic) => {
                        let [instance_ty, attr_name, default] = overload.parameter_types() else {
                            continue;
                        };

                        let Some(attr_name) = attr_name.into_string_literal() else {
                            continue;
                        };

                        let default = if default.is_unknown() {
                            Type::Never
                        } else {
                            *default
                        };

                        let union_with_default = |ty| UnionType::from_elements(db, [ty, default]);

                        // TODO: we could emit a diagnostic here (if default is not set)
                        overload.set_return_type(
                            match instance_ty.static_member(db, attr_name.value(db)) {
                                Symbol::Type(ty, Boundness::Bound) => {
                                    if instance_ty.is_fully_static(db) {
                                        ty
                                    } else {
                                        // Here, we attempt to model the fact that an attribute lookup on
                                        // a non-fully static type could fail. This is an approximation,
                                        // as there are gradual types like `tuple[Any]`, on which a lookup
                                        // of (e.g. of the `index` method) would always succeed.

                                        union_with_default(ty)
                                    }
                                }
                                Symbol::Type(ty, Boundness::PossiblyUnbound) => {
                                    union_with_default(ty)
                                }
                                Symbol::Unbound => default,
                            },
                        );
                    }

                    _ => {}
                },

                Type::ClassLiteral(ClassLiteralType { class }) => match class.known(db) {
                    Some(KnownClass::Bool) => {
                        overload.set_return_type(
                            arguments
                                .first_argument()
                                .map(|arg| arg.bool(db).into_type(db))
                                .unwrap_or(Type::BooleanLiteral(false)),
                        );
                    }

                    Some(KnownClass::Str) if overload_index == 0 => {
                        overload.set_return_type(
                            arguments
                                .first_argument()
                                .map(|arg| arg.str(db))
                                .unwrap_or_else(|| Type::string_literal(db, "")),
                        );
                    }

                    Some(KnownClass::Type) if overload_index == 0 => {
                        if let Some(arg) = arguments.first_argument() {
                            overload.set_return_type(arg.to_meta_type(db));
                        }
                    }
                    _ => {}
                },
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call.rs`:13 on 2025-03-14 17:12_

nit: I think I'd prefer named fields?

```suggestion
pub(super) struct CallError<'db> {
    pub(super) kind: CallErrorKind,
    pub(super) bindings: Box<Bindings<'db>>,
}
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/signatures.rs`:26 on 2025-03-14 17:18_

hmm, what's meant by "directly callable" here? _All_ callable objects in Python have a `__call__` method, even functions; this is core to the definition of what it means for an object to be callable in the language:

```py
>>> def foo():
...     print('hi')
...     
>>> foo.__call__()
hi
```

For _some_ types, like function-literal types, we don't attempt to lookup the `__call__` method in red-knot in order to infer the signature of the callable. But that doesn't mean that the `__call__` method doesn't exist!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call/bind.rs`:72 on 2025-03-14 17:20_

nit 

```suggestion
    pub(crate) const fn is_single(&self) -> bool {
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call/bind.rs`:91 on 2025-03-14 17:21_

> I don't think it will matter for performance, since we shouldn't have hundreds or thousands of union elements. So

you'd be surprised! A lot of pathological edge cases to do with type-checker performance are to do with large unions. I listed some of the ones mypy and pyright have come across in the past in https://github.com/astral-sh/ruff/issues/13549

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call/bind.rs`:238 on 2025-03-14 17:23_

nit

```suggestion
    const fn is_callable(&self) -> bool {
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call/bind.rs`:312 on 2025-03-14 17:23_

nit

```suggestion
    pub(crate) const fn dunder_is_possibly_unbound(&self) -> bool {
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call/bind.rs`:578 on 2025-03-14 17:24_

heh, "descriptor" is quite an overloaded word in red-knot 😆 maybe `CallableDescription`?

```suggestion
pub(crate) struct CallableDescription<'a> {
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/signatures.rs`:92 on 2025-03-14 17:29_

nit: we changed our naming convention in https://github.com/astral-sh/ruff/pull/15617 to use `type` rather than `ty` where possible. That obviously isn't ergonomic for the first field in this struct (as it conflicts with a Rust keyword), but I'd prefer for this to be called `signature_type` (and similar for various other fields on various other structs in this PR)

```suggestion
    pub(crate) signature_type: Type<'db>,
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/signatures.rs`:130 on 2025-03-14 17:30_

Does this still panic if the iterator is empty? It doesn't _look_ to me like it does

---

_@AlexWaygood reviewed on 2025-03-14 17:31_

Not really a full review (sorry!), just a couple of things I spotted while skimming

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2763 on 2025-03-14 20:31_

That's a good callout!  I had done it this way because I like to avoid too must nesting if possible, but I hadn't realized the performance implications.  Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call.rs`:13 on 2025-03-14 20:35_

Normally I agree but this type is only ever consumed in pattern matches, and my thought was that

```rust
Err(CallDunderError::CallError(CallErrorKind::BindingError, bindings)) => ...
```

is easier to read than

```rust
Err(CallDunderError::CallError {
    kind: CallErrorKind::BindingError,
    bindings,
})) => ...
```

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/signatures.rs`:26 on 2025-03-14 20:38_

Ah that's a good clarification!  The idea I'm trying to convey is whether we use type of the object or the type of the `__call__` method in certain error messages.  Is there existing terminology that we can use here?

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:91 on 2025-03-14 20:45_

Good to know!  Added the boolean

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:578 on 2025-03-14 20:46_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/signatures.rs`:130 on 2025-03-14 20:47_

It doesn't!  Good catch

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/signatures.rs`:92 on 2025-03-14 20:50_

Done (and `ty` → `callable_type`)

---

_@dcreager reviewed on 2025-03-14 20:53_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:26 on 2025-03-14 21:49_

I think the names `callable_type` and `signature_type` are reasonable here. Here's what I'd propose for this wording:
```suggestion
    /// The type we'll use for error messages referring to details of the called signature. For calls to functions this
    /// will be the same as `callable_type`; for other callable instances it may be a `__call__` method.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:52 on 2025-03-14 21:54_

Does this method panic on receiving an empty iterator? It doesn't look like it would, it looks like it would just create a `Signatures` with empty `elements`.

Also I think it takes an iterator of `Signatures`s, not `Signature`s :)
```suggestion
    /// Creates a new `Signatures` from an iterator of [`Signatures`].
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:108 on 2025-03-14 21:59_

For vectors that will usually be length 1, I always wonder whether `SmallVec` would be a win (by reducing allocation/fragmentation in the common case). But it may not be. (And sometimes it introduces bogus lifetime variance problems, because the current version of the `smallvec` crate has a lifetime annotations bug.)

Or I guess an enum like you did with `BindingsInner` is another option. Is there a clear principle by which you chose simple vectors for signatures but a more complex representation for bindings?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:98 on 2025-03-14 22:01_

Let's not forget to update this along with the above similar comment.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:136 on 2025-03-14 22:03_

It seems like there's nothing actually requiring the iterator to be non-empty; if it's empty we'll just create a not-callable `CallableSignature`? Which seems fine.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:108 on 2025-03-14 22:06_

Maybe add a comment here noting that an empty `overloads` indicates an object that is not callable?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:30 on 2025-03-14 22:14_

I guess `&'db` lifetime is right because this is a reference returned from a `#[return_ref]` Salsa query, so it really is borrowed from the db?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:105 on 2025-03-14 22:21_

It's not clear to me why this is important. Do you judge it important for the user experience? The only effect of the significant simplification below is that we get `Unknown | int` instead of `int | Unknown` in a few places, which doesn't seem like a problem to me (in fact it seems potentially better, if the ordering of the return types maps directly to the ordering of elements in the original callable union):
```suggestion
                UnionType::from_elements(db, bindings.iter().map(|b| b.return_type()))
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:59 on 2025-03-14 22:25_

So `BindingError` may also represent a union that is possibly not callable, but we prioritize surfacing the binding error over the fact that it may not be callable at all? That may be OK, but I feel like ideally I'd want both problems surfaced at once. (I'm not worried about the details of how that is reflected in diagnostics until we have the new diagnostics system in use, just about whether the representation is capable of carrying the necessary information.)

In principle it seems like `PossiblyNotCallable` and `BindingError` are orthogonal, so to flatten to a single enum we have to choose one of them to prioritize.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:58 on 2025-03-14 22:27_

Is `PossiblyNotCallable` a possible result from a binding of a single `CallableSignature`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:103 on 2025-03-14 22:45_

It seems like in practice this is really just a single bit of information. Either we aren't explicitly representing `__call__` at all (e.g. for a function type), which is equivalent to (and is actually at runtime) "`__call__` is definitely bound", or we have "`__call__` is possibly bound". It doesn't seem like `Unbound` is actually possible, because then we wouldn't have a signature at all.

So would it be simpler to represent this as `dunder_call_may_be_unbound: bool`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:382 on 2025-03-14 22:52_

nit: if we'll rename the type (which I like), let's do the variable too
```suggestion
        let callable_description =
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:742 on 2025-03-14 22:53_

```suggestion
        callable_description: Option<&CallableDescription>,
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call.rs`:21 on 2025-03-14 22:57_

I think it's worth noting here that the type may also be possibly not callable.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2318 on 2025-03-14 23:12_

What are the "extra allocations for repeated Signatures" here? We'll create one for each `(self, callable_ty)` combo, which is I think the minimum we need regardless (barring a different way of handling `callable_ty` entirely).

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2325 on 2025-03-14 23:25_

It feels to me like it should be possible to get rid of this `callable_ty` argument (and thus the Salsa interning) entirely, but it might require a recursive `Signature::set_callable_ty` method; not sure if that's worth it or not.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2330 on 2025-03-14 23:27_

nit: what about a `CallableSignature.with_bound_type()` method that wouldn't require wrapping in `Some` or publicly exposing the `bound_type` field directly?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2658 on 2025-03-14 23:31_

Not sure, but does this break handling of these special known callables when they occur inside a union? Should we be matching on a `callable_type` or `signature_type` from the binding's specific signature, rather than on `self` here?

---

_@carljm approved on 2025-03-14 23:35_

Overall I like this a lot!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2325 on 2025-03-15 12:23_

there's too many comments already on this line of code 😆 but if we _do_ keep this argument, I'd prefer it to be named

```suggestion
    fn signatures(self, db: &'db dyn Db, callable_type: Type<'db>) -> Signatures<'db> {
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2941 on 2025-03-15 12:24_

```suggestion
        callable_type: Option<Type<'db>>,
        name: &str,
    ) -> Option<(&'db Signatures<'db>, Boundness)> {
        match self
            .member_lookup_with_policy(db, name.into(), MemberLookupPolicy::NoInstanceFallback)
            .symbol
        {
            Symbol::Type(dunder_callable, boundness) => {
                let callable_type = callable_type.unwrap_or(dunder_callable);
                Some((dunder_callable.signatures(db, callable_type), boundness))
```

---

_@AlexWaygood reviewed on 2025-03-15 12:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2318 on 2025-03-15 12:29_

I just want to note quickly that there is a 2% incremental benchmark regression being reported for this PR, which _may_ be due to the changes in the Salsa tracking that we're discussing in this thread? I think a 2% regression is probably fine, and we probably shouldn't overfit on `tomllib`[^1] So I'm not necessarily asking for any change here, just wanted to note that there is a _small_ regression being reported by Codspeed.

[^1]: At some point we should add better benchmarks that also include red-knot being run on larger codebases, and on code that's less typed than tomllib, and on code that's generally lower quality than tomllib, and on code that contains bigger unions, etc. etc. etc. Once we've added those benchmarks, we'll have much better data on which exact functions should be Salsa-tracked and what the best granularity of caching is.

---

_@AlexWaygood reviewed on 2025-03-15 12:29_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3864 on 2025-03-15 12:39_

Consider implementing `IntoIterator` for `&Bindings` and `&mut Bindings`; it would mean we could avoid the explicit `.iter()` and `.iter_mut()` calls:

```diff
diff --git a/crates/red_knot_python_semantic/src/types.rs b/crates/red_knot_python_semantic/src/types.rs
index adad3f189..8d238378f 100644
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -2647,7 +2647,7 @@ impl<'db> Type<'db> {
     ) -> Result<Bindings<'db>, CallError<'db>> {
         let signatures = self.signatures(db, self);
         let mut bindings = Bindings::bind(db, signatures, arguments).into_result()?;
-        for binding in bindings.iter_mut() {
+        for binding in &mut bindings {
             // For certain known callables, we have special case logic to determine the return type
             // in a way that isn't directly expressible in the type system. Each special case
             // listed here should have a corresponding clause above in `signatures`.
diff --git a/crates/red_knot_python_semantic/src/types/call.rs b/crates/red_knot_python_semantic/src/types/call.rs
index 7d82d449e..b994c18de 100644
--- a/crates/red_knot_python_semantic/src/types/call.rs
+++ b/crates/red_knot_python_semantic/src/types/call.rs
@@ -10,7 +10,7 @@ pub(super) use bind::Bindings;
 /// Wraps a [`Bindings`] for an unsuccessful call with information about why the call was
 /// unsuccessful.
 #[derive(Debug, Clone, PartialEq, Eq)]
-pub(super) struct CallError<'db>(pub(super) CallErrorKind, pub(super) Box<Bindings<'db>>);
+pub(crate) struct CallError<'db>(pub(super) CallErrorKind, pub(super) Box<Bindings<'db>>);
 
 /// The reason why calling a type failed.
 #[derive(Debug, Clone, Copy, PartialEq, Eq)]
diff --git a/crates/red_knot_python_semantic/src/types/call/bind.rs b/crates/red_knot_python_semantic/src/types/call/bind.rs
index e2b6d24ac..e04e85867 100644
--- a/crates/red_knot_python_semantic/src/types/call/bind.rs
+++ b/crates/red_knot_python_semantic/src/types/call/bind.rs
@@ -128,7 +128,7 @@ impl<'db> Bindings<'db> {
         let mut all_ok = true;
         let mut any_binding_error = false;
         let mut all_not_callable = true;
-        for binding in self.iter() {
+        for binding in &self {
             let result = binding.as_result();
             all_ok &= result.is_ok();
             any_binding_error |= matches!(result, Err(CallErrorKind::BindingError));
@@ -149,14 +149,14 @@ impl<'db> Bindings<'db> {
         }
     }
 
-    pub(crate) fn iter(&self) -> impl Iterator<Item = &CallableBinding<'db>> + '_ {
+    pub(crate) fn iter(&self) -> std::slice::Iter<'_, CallableBinding<'db>> {
         match &self.inner {
             BindingsInner::Single(binding) => std::slice::from_ref(binding).iter(),
             BindingsInner::Union(bindings) => bindings.iter(),
         }
     }
 
-    pub(crate) fn iter_mut(&mut self) -> impl Iterator<Item = &mut CallableBinding<'db>> + '_ {
+    pub(crate) fn iter_mut(&mut self) -> std::slice::IterMut<'_, CallableBinding<'db>> {
         match &mut self.inner {
             BindingsInner::Single(binding) => std::slice::from_mut(binding).iter_mut(),
             BindingsInner::Union(bindings) => bindings.iter_mut(),
@@ -192,6 +192,24 @@ impl<'db> Bindings<'db> {
     }
 }
 
+impl<'a, 'db> IntoIterator for &'a Bindings<'db> {
+    type Item = &'a CallableBinding<'db>;
+    type IntoIter = std::slice::Iter<'a, CallableBinding<'db>>;
+
+    fn into_iter(self) -> Self::IntoIter {
+        self.iter()
+    }
+}
+
+impl<'a, 'db> IntoIterator for &'a mut Bindings<'db> {
+    type Item = &'a mut CallableBinding<'db>;
+    type IntoIter = std::slice::IterMut<'a, CallableBinding<'db>>;
+
+    fn into_iter(self) -> Self::IntoIter {
+        self.iter_mut()
+    }
+}
+
 /// Binding information for a single callable. If the callable is overloaded, there is a separate
 /// [`Binding`] for each overload.
 ///
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index fde224c34..2da8a9df5 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -3861,7 +3861,7 @@ impl<'db> TypeInferenceBuilder<'db> {
         let call_arguments = self.infer_arguments(arguments, parameter_expectations);
         match function_type.try_call(self.db(), &call_arguments) {
             Ok(bindings) => {
-                for binding in bindings.iter() {
+                for binding in &bindings {
                     let Some(known_function) = binding
                         .signature
                         .callable_type
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2642 on 2025-03-15 12:43_

```suggestion
    /// the arguments are not compatible with the formal parameters.
    ///
    /// You get back a [`Bindings`] for both successful and unsuccessful calls.
    /// It contains information about which formal parameters each argument was matched to,
    /// and about any errors matching arguments and parameters.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2651 on 2025-03-15 12:43_

```suggestion
            // For certain known callables, we have special-case logic to determine the return type
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2683 on 2025-03-15 12:48_

you can reduce the indentation a little here:

```diff
diff --git a/crates/red_knot_python_semantic/src/types.rs b/crates/red_knot_python_semantic/src/types.rs
index adad3f189..306455196 100644
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -2669,17 +2669,13 @@ impl<'db> Type<'db> {
                                 BoundMethodType::new(db, function, instance.to_meta_type(db)),
                             )));
                         }
-                    } else {
-                        if let Some(first) = arguments.first_argument() {
-                            if first.is_none(db) {
-                                overload.set_return_type(Type::FunctionLiteral(function));
-                            } else {
-                                overload.set_return_type(Type::Callable(
-                                    CallableType::BoundMethod(BoundMethodType::new(
-                                        db, function, first,
-                                    )),
-                                ));
-                            }
+                    } else if let Some(first) = arguments.first_argument() {
+                        if first.is_none(db) {
+                            overload.set_return_type(Type::FunctionLiteral(function));
+                        } else {
+                            overload.set_return_type(Type::Callable(CallableType::BoundMethod(
+                                BoundMethodType::new(db, function, first),
+                            )));
                         }
                     }
                 }
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2959 on 2025-03-15 12:56_

```suggestion
        let (signature, boundness) = self
            .dunder_signature(db, None, name)
            .ok_or(CallDunderError::MethodNotAvailable)?;
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2605 on 2025-03-15 13:07_

A small suggested tweak to the comment here: if a type has `__call__` set to `None`, it is not callable from the language's perspective: in order for an object to be callable, it has to have a `__call__` method that is itself callable. So it's a _little_ confusing to say here that an object might be "callable via a `__call__` method" but then later in the same paragraph say that attempting to call the `__call__` method might then cause us to emit an error saying that "`__call__` is not callable".

```suggestion
                // Note that for objects that have a (possibly not callable!) `__call__` attribute,
                // we will get the signature of the `__call__` attribute, but will pass in the type
                // of the original object as the "callable type". That ensures that we get errors
                // like "`X` is not callable" instead of "`<type of illegal '__call__'>` is not
                // callable".
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call.rs`:13 on 2025-03-15 13:21_

could you add a comment saying that we use `Box<Bindings>` rather than simply `Bindings` because otherwise we would be passing around very large `Err` variants around on the stack? It wasn't immediately clear to me why we were doing the boxing here

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call/bind.rs`:53 on 2025-03-15 13:25_

It seems like every time we call `Bindings::bind()` we immediately call `.into_result()` on it. And if I understand correctly, I think it would be incorrect to call `Bindings::bind()` and _not_ call `.into_result()` on it (because then you would have constructed a `Bindings` that might represent a successful call _or_ an unsuccessful call). So maybe `Bindings::bind()` should just return `Result<Self, CallError<'db>>` (and we get rid of `Bindings::into_result()`) rather than splitting the logic into two methods?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call/bind.rs`:65 on 2025-03-15 13:26_

you can `.collect()` directly into a boxed slice:

```suggestion
            .collect();
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call/bind.rs`:266 on 2025-03-15 13:28_

```suggestion
            .collect();
```

---

_@AlexWaygood reviewed on 2025-03-15 13:31_

---

_@AlexWaygood approved on 2025-03-15 13:32_

This looks great. Thank you!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/signatures.rs`:52 on 2025-03-15 14:18_

Ah yes! This is from an earlier version where we actually did panic, on the assumption that a union type is never empty. The rest of the code does assume that the `signatures` field is non-empty, so this should either panic as before or construct `non_callable` for an empty union. I'm putting it back to panicking.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/signatures.rs`:108 on 2025-03-15 14:22_

I did have an enum before, actually, and then as part of @MichaReiser suggestion's to track/intern these structs, I moved it to a plain vector since (a) it made the code simpler and (b) the extra allocations for non-union non-overloaded callables mattered less when we were going through the extra work to have salsa track/intern the thing. Signatures are still only created inside of a salsa-tracked method, so (b) still seems to hold.

That said, I do like your `SmallVec<1>` idea as a way to get the best of both.  Will give that a shot.

Update: I'm getting salsa-related lifetime errors when switching to `SmallVec`, due to `Type::signatures` being marked `return_ref`. (Removing `return_ref` eliminates the lifetime errors.)  I'm going to table this for now, address the rest of the comments, and then come back to it.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/signatures.rs`:108 on 2025-03-15 14:25_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/signatures.rs`:98 on 2025-03-15 14:55_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/signatures.rs`:136 on 2025-03-15 14:56_

Updated comment

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:30 on 2025-03-15 14:56_

Yep that's right

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:105 on 2025-03-15 14:58_

Works for me! This was me trying to faithfully mimic the logic that was there before, which also specifically put the errorful types at the end. The code is certainly simpler this way!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:59 on 2025-03-15 15:01_

> In principle it seems like `PossiblyNotCallable` and `BindingError` are orthogonal, so to flatten to a single enum we have to choose one of them to prioritize.

This was me mimicking the previous behavior, where we were already making this prioritization.

> I'm not worried about the details of how that is reflected in diagnostics until we have the new diagnostics system in use, just about whether the representation is capable of carrying the necessary information.

All of the information is definitely there in the bindings types themselves, so I'm not worried about being able to make this change as part of the larger diagnostics reworking. So I'd suggest holding off on that until then.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:58 on 2025-03-15 15:03_

No, for that either there's an overload that matches the argument list, in which case the `CallableSignature` is callable, or there isn't, in which case it's (completely, not possibly) non-callable.  `PossiblyNotCallable` only comes into play with union types.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/signatures.rs`:103 on 2025-03-15 17:42_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:382 on 2025-03-15 17:43_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:742 on 2025-03-15 17:43_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call.rs`:21 on 2025-03-15 17:49_

Reworded the comments for all three variants to hopefully be clearer

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2325 on 2025-03-15 18:10_

I had it before as `replace_type`, which I can bring back. Removing the interning might also help with the salsa lifetime errors I mention in another comment.  (I did not bring back the salsa-cached `overloads` functions that we had before — it's not clear that cloning the result from that salsa query will be faster than constructing a new `Signatures` on the fly for our special case methods.)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2330 on 2025-03-15 18:27_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2658 on 2025-03-15 19:05_

It sure does, great catch!  Fixed and added an mdtest for this

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2325 on 2025-03-15 19:06_

I removed the param but did a (hopefully) more thorough search/replace on this

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2941 on 2025-03-15 19:06_

This function is gone now

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:3864 on 2025-03-15 19:16_

Done.  I did this for `CallableBinding`, too, as well as for the corresponding `Signatures` types

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2683 on 2025-03-15 19:17_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2959 on 2025-03-15 19:18_

I ended up inline the `dunder_signature` function so this is moot

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2605 on 2025-03-15 19:18_

Thanks!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call.rs`:13 on 2025-03-15 19:20_

Done. (As an aside, it offends my sense of fairness that clippy thinks it's okay to pass around a large `Ok` variant! :smile:)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:53 on 2025-03-15 19:22_

Good idea, done.  (I kept `into_result` as a separate method since there are two return sites in `bind` and I wanted to avoid copy-pasting the logic twice.  But I made `into_result` private and called it on behalf of the caller.)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:65 on 2025-03-15 19:24_

Done

---

_@dcreager reviewed on 2025-03-15 19:28_

---

_@dcreager reviewed on 2025-03-15 19:39_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/signatures.rs`:108 on 2025-03-15 19:39_

With the other suggestions that led to us not tracking this function anymore, I got the `SmallVec` update to work.  Using that now for both signatures and bindings

---

_@AlexWaygood reviewed on 2025-03-15 19:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call/bind.rs`:53 on 2025-03-15 19:39_

hmm, did you maybe forget to commit and push that change? It looks to me like `into_result()` is still `pub(crate)` and `Bindings::bind()` still returns `Self`

---

_@dcreager reviewed on 2025-03-15 19:48_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:53 on 2025-03-15 19:48_

Gah, you're right! Not sure where that went. Pushed for real this time — and now with `SmallVec`, there _aren't_ multiple return sites, and so I did into `into_result` into `bind`

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4337 on 2025-03-15 19:53_

the TODO seems done?

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2458 on 2025-03-15 19:55_

we could probably micro-optimize this slightly by doing an inner `match` over `class.known()` again rather than lots of `Type::ClassLiteral` branches with `if class.is_known()` guards

---

_@AlexWaygood approved on 2025-03-15 19:57_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:4337 on 2025-03-15 20:00_

I think it's partly done; we still aren't handling `@overload` decorators, which I think are needed to address the first part of the TODO

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:2458 on 2025-03-15 20:02_

Done

---

_@dcreager reviewed on 2025-03-15 20:02_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4032 on 2025-03-15 20:42_

oops -- the change to the snapshot makes me realise that there was a pre-existing bug here; this should be

```suggestion
                        and its `__getitem__` method (with type `{dunder_getitem_type}`) \
```

---

_@AlexWaygood reviewed on 2025-03-15 20:43_

---

_Closed by @AlexWaygood on 2025-03-17 07:55_

---

_Reopened by @AlexWaygood on 2025-03-17 07:56_

---

_Comment by @github-actions[bot] on 2025-03-17 08:09_

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

_@dcreager reviewed on 2025-03-17 14:10_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types.rs`:4032 on 2025-03-17 14:10_

Thanks!

---

_@dcreager reviewed on 2025-03-17 14:11_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:30 on 2025-03-17 14:11_

(This is no longer relevant now that we're not `return_ref` tracking the `signatures` result)

---

_@AlexWaygood reviewed on 2025-03-17 14:12_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2318 on 2025-03-17 14:12_

On the latest version of the PR, this seems now to be down to only a 1% regression

---

_Merged by @dcreager on 2025-03-17 14:35_

---

_Closed by @dcreager on 2025-03-17 14:35_

---

_Branch deleted on 2025-03-17 14:35_

---

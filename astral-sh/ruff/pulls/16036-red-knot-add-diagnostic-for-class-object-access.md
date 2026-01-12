```yaml
number: 16036
title: "[red-knot] Add diagnostic for class-object access to pure instance variables"
type: pull_request
state: merged
author: mishamsk
labels:
  - ty
assignees: []
merged: true
base: main
head: 15963-fix-diagnostic-for-pure-instance-variables-access-from-class
created_at: 2025-02-08T04:31:32Z
updated_at: 2025-02-27T01:25:56Z
url: https://github.com/astral-sh/ruff/pull/16036
synced_at: 2026-01-12T15:55:53Z
```

# [red-knot] Add diagnostic for class-object access to pure instance variables

---

_@mishamsk_

## Summary

This is at best a band-aid solution for astral-sh/ruff#15963 . See my first comment below for discussion.

As of time of writing:

* Fixes unbound class attribute access. Doesn't return a bound symbol anymore and thus causes a `[unresolved-attribute]` diagnostic
* Intentionally as a separate commit:
  * Changes`Type::ClassLiteral` representation to `type[ClassName]` matching mypy (this was already the case for `Type::SubclassOf`)
  * Changes representation of a union of 2+ types to `types.UnionType[TypeOne, TypeTwo, ...]` also matching mypy

I didn't not add a separate new lint, despite the issue description. I am not sure why `[unresolved-attribute]` is not appropriate. Unbound attributes indeed do not exist at runtime, so the language seems perfectly appropriate.

## Test Plan

cargo nextest run -p red_knot_python_semantic --no-fail-fast


---

_Comment by @mishamsk on 2025-02-08 05:00_

@sharkdp first, sorry for taking so long, I got distracted by work ;-) 

second, sorry for a long read for such a small thing. However, since we can't just talk it over - I'll dump why I think it is a band aid and PR marked as draft. 

Disclaimer: by no means I am implying there is anything wrong with knot code. I totally understand that you are moving crazy fast and this is one huge undertaking, so I totally get it when something is not "perfect" or a temporary solution.

The reason it took me multiple hours to understand what's happening is that I got confused by the fact that knot uses a notion of "Symbol" for both real symbols and unbound declarations, which are not symbols at runtime. They only end up in annotations:

```
In [1]: class C:
   ...:     not_a_symbol: int
   ...:

In [2]: dir(C)
Out[2]:
['__annotations__',
 '__class__',
 '__delattr__',
 '__dict__',
 '__dir__',
 '__doc__',
 '__eq__',
 '__format__',
 '__ge__',
 '__getattribute__',
 '__getstate__',
 '__gt__',
 '__hash__',
 '__init__',
 '__init_subclass__',
 '__le__',
 '__lt__',
 '__module__',
 '__ne__',
 '__new__',
 '__reduce__',
 '__reduce_ex__',
 '__repr__',
 '__setattr__',
 '__sizeof__',
 '__str__',
 '__subclasshook__',
 '__weakref__']

In [3]: C.__annotations__
Out[3]: {'not_a_symbol': int}
```

Even though I quickly found the passage rgd `Declaration` that is used for both types of statements, the fact that they both create a symbol wasn't readily apparent.

Further, confusing the matter was the fact that `red_knot_python_semantic::symbol::Symbol` (not entirely helpfully matching the `red_knot_python_semantic::semantic_index::symbol::Symbol`) returns the enum `Boundness` for both types of "symbols". So pure (unbound) declarations will be `Symbol::Type(ty, Boundness::Bound)`.

## Ideas

### Symbol::Type
Tbh and imho symbols and declarations should be split. At least in this context. But I felt like doing so would cause too many changes in too many places. Something I would only do if you confirm such an idea. There are multiple ways this can be done:

* Very invasive - create something separate from symbols for declarations and match them to bindings / use sites when necessary. Probably too much of a change and disruption 
* Extend `Boundness` and add two more values: `Declared` and `PossiblyDeclared`
* In the same vain - extend `Symbol::Type` tuple by adding some enum with the sub-kind of the symbol. This won't solve the confusing "Boundness::Bound unbound declared symbol" thing though
* Maybe there is even better way

### Split `symbol` function

`red_knot_python_semantic::types::symbol` function is used in multiple places to get the symbol type & boundness. But not all of them, e.g. `Class::own_instance_member` does it's own but similar thing. In the same vain, it was confusing me a bit. I'd split it or at least extract a more narrower common part out of it. 

E.g. why would we need to check for `TYPE_CHECKING` on every invocation, if the only place where it may be triggered from is `known_module_symbol` (which calls `symbol` through `global_symbol`) and inversely why looking up from `global_symbol` would need to check for dunder `__slots__`.

Again, really sorry for a long dump, especially if there are very good reasons for all of this that I just failed to identify.

---

_Comment by @mishamsk on 2025-02-08 05:09_

well. here is one false-positive from this change: https://github.com/astral-sh/ruff/actions/runs/13212332102/job/36887424507?pr=16036#step:7:94

ClassVar in stubs/ABCs needs to be handled as bound... I'll look into it later

---

_Label `red-knot` added by @AlexWaygood on 2025-02-08 11:03_

---

_Comment by @sharkdp on 2025-02-10 14:56_

As mentioned on Discord, I'll split my reply into a few comments

> * Intentionally as a separate commit:
>   * Changes`Type::ClassLiteral` representation to `type[ClassName]` matching mypy (this was already the case for `Type::SubclassOf`)
>   * Changes representation of a union of 2+ types to `types.UnionType[TypeOne, TypeTwo, ...]` also matching mypy

Commenting on this first. This should definitely be discussed elsewhere (ideally as a ticket first), but I don't think that we want either of these changes. 

Unlike mypy, we deliberately distinguish `Literal[C]` from `type[C]`, as they denote different types. We don't know yet if we really want to use `Literal[C]` as the spelling of that type, but I don't think we want to mix up `Literal[C]` and `type[C]`.

The difference between the two is:

* `Literal[C]` is the type of the class object `C`, for a `class C: …`. It is a singleton type with one single inhabitant: the class object `C`.
* `type[C]` is the type of all class objects `S`, where `S` is a subclass of `C`, or `C` itself.

More concretely, both `C` and `D` are of type `type[C]`, but only `C` is of type `Literal[C]`:

```py
class C: ...
class D(C): ...
```


The union-type representation change would be more stylistic, but I don't see any argument for choosing `types.UnionType[TypeOne, TypeTwo]` over `TypeOne | TypeTwo`?

---

_Comment by @mishamsk on 2025-02-10 15:20_

> but I don't think that we want either of these changes

Yeah, that's why put them in a separate commit. I had a hunch ;-) I just saw comments like "we want to improve the message". I'll drop this commit, no prob.

> The difference between the two

yes, I do understand that and I saw that you use `type[]` only for `Type::SubclassOf`. I just thought that `Literal` is a bit unconventional coming from mypy/pyright. If you are planning to stray far from conventions, maybe changing the outer message would be an interesting approach. Instead of 
```
Type `Literal[A]` has no attribute `non_existent`
```

generate 
```
Class `A` has no attribute `non_existent`
```

> The union-type representation change would be more stylistic, but I don't see any argument for choosing types.UnionType[TypeOne, TypeTwo] over TypeOne | TypeTwo?

same here. I went "conventional" way (mypy). If you keep `Literal`, it would be `Literal[TypeOne, TypeTwo]` I guess


---

_Comment by @sharkdp on 2025-02-10 15:44_

> I just saw comments like "we want to improve the message".

> Class `A` has no attribute `non_existent`

I should have described this better in the TODO comment:

https://github.com/astral-sh/ruff/blob/f7819e553f0dbb22afcf31ea77938d529fe52818/crates/red_knot_python_semantic/resources/mdtest/attributes.md?plain=1#L53-L56

I didn't specifically want to talk about the `Literal[C]` part here, but I agree that this could *also* be improved. I was rather thinking about the following: This specific diagnostic here is raised if someone tries to access a *pure instance variable* on the class object (not for a non-existent attribute). So I was thinking of giving them a hint and explaining that in more detail: "The attribute `inferred_from_value` can only be accessed on instances of class `C`, but not on the class object itself."

---

_Comment by @sharkdp on 2025-02-10 15:52_

> Disclaimer: by no means I am implying there is anything wrong with knot code. I totally understand that you are moving crazy fast and this is one huge undertaking, so I totally get it when something is not "perfect" or a temporary solution.

> The reason it took me multiple hours to understand what's happening is that I got confused by the fact that knot uses a notion of "Symbol" for both real symbols and unbound declarations, which are not symbols at runtime. They only end up in annotations:

> Further, confusing the matter was the fact that `red_knot_python_semantic::symbol::Symbol` (not entirely helpfully matching the `red_knot_python_semantic::semantic_index::symbol::Symbol`) returns the enum `Boundness` for both types of "symbols". So pure (unbound) declarations will be `Symbol::Type(ty, Boundness::Bound)`.

> Even though I quickly found the passage rgd `Declaration` that is used for both types of statements, the fact that they both create a symbol wasn't readily apparent.

It's completely fine to call this out. We know that the `Symbol` API can be improved, and we had a long discussion about the name — and we are aware that the clash with the term "symbol" in the semantic index builder is unfortunate. We also know that our handling of "boundness" and "declaredness" needs to be improved. For more details, see [this issue](https://github.com/astral-sh/ty/issues/229) and the explanations and comments in (this test suite](https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/resources/mdtest/boundness_declaredness/public.md).

That said, I would appreciate if we could move this discussion into a new ticket, to discuss it separately from the change in question here.

---

_Comment by @sharkdp on 2025-02-10 19:24_

I now had a more detailed look at the ideas you proposed and they do look very interesting to me. They definitely go in the right direction. What I would prefer is if we could decouple any refactoring from this functional change here. Your "bandaid" solution doesn't look too bad to me, given our current API restrictions.

It would be great if you could remove the second commit from this branch, then we could focus on the functional change here and try to understand the errors in stub files better.

---

_Comment by @codspeed-hq[bot] on 2025-02-11 00:21_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mishamsk%3A15963-fix-diagnostic-for-pure-instance-variables-access-from-class)

### Merging astral-sh/ruff#16036 will **not alter performance**

<sub>Comparing <code>mishamsk:15963-fix-diagnostic-for-pure-instance-variables-access-from-class</code> (005fd6d) with <code>main</code> (e7a6c19)</sub>



### Summary

`✅ 32` untouched benchmarks  





---

_Comment by @mishamsk on 2025-02-11 00:24_

@sharkdp first rdg the PR:

- reverted class literal display code
- added ABC, ABCMeta to known classes
- added handling of stubs & classvars to `own_class_member` with some tests
  - had to extract the code from `symbol` as I needed to get qualifiers, not just type... this is possibly a performance hit, since `symbol` used salsa query internally... something had to give (either big changes across the board, or this). To my defense `own_instance_member` is also re-creating the logic instead of using a shared query

Am I right, that you also want me to create a separate diagnostic for this specific access patter with the message you suggested? Didn't have time for this yet.

Also, I didn't do extensive validation that ABC's are properly caught in all possible scenarios. But at least based on two tests they do.

Now back to the general design. Or should I move this to astral-sh/ty#229? 

I saw astral-sh/ty#229 and related comments, but not https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/resources/mdtest/boundness_declaredness/public.md - thanks for the link. I think the table is awesome, it literally suggest the perfect way to model this.

I'd refactor symbol to:
```rust
#[derive(Debug, Clone, Copy, PartialEq, Eq)]
pub(crate) enum BindingStatus<'db> {
    Bound(Type<'db>),
    PossiblyUnbound(Type<'db>),
    Unbound,
}

#[derive(Debug, Clone, Copy, PartialEq, Eq)]
pub(crate) enum DeclarationStatus<'db> {
    Declared(TypeAndQualifiers<'db>),
    PossiblyDeclared(TypeAndQualifiers<'db>),
    Undeclared,
}

#[derive(Debug, Clone, PartialEq, Eq)]
pub(crate) enum Symbol<'db> {
    Known(BindingStatus<'db>, DeclarationStatus<'db>),
    /// Or Make Symbol a tuple since Unknown == BindingStatus::Unbound & DeclarationStatus::Undeclared
    Unknown,
}
```

return this from `symbol` query and then let the caller apply whatever logic is appropriate. `Symbol` can have methods that resolve types between declarations & bindings based on your table from that file too.

---

_@sharkdp reviewed on 2025-02-11 13:25_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:4185 on 2025-02-11 13:25_

Instead of designing a special case for stubs and abstract base classes, shouldn't we design a special case for `ClassVar`s? The false positive you were getting from stubs was that `Literal[timezone]` has no attribute `utc`. But `utc` is explicitly [annotated as a `ClassVar`](https://github.com/python/typeshed/blob/24c78b9e0dff457275092025a14387dac206f5d6/stdlib/datetime.pyi#L29).

The idea of [the ticket](https://github.com/astral-sh/ruff/issues/15963) was to detect class-object access to *instance variables*. If we see a `utc: ClassVar[…]` annotation (in a stub or not), we should treat that as a pure class variable instead and _allow_ access on class objects.

It would be great if we would not have to special-case stubs and ABCs.

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:4185 on 2025-02-13 04:45_

sorry. got carried away I guess... this is fixed. I'll list the full list of changes in a new thread below

---

_@mishamsk reviewed on 2025-02-13 04:45_

---

_Comment by @mishamsk on 2025-02-13 04:55_

- remove ABC detection
- remove special casing for class member access in stubs
- improve error message for invalid pure instance attr access from class in load context 
- added error message for invalid pure instance attr access from class in store context 
- switched the symbol lookup implementation inside class member function into salsa query, similar to the not-so-shared `symbol` function

---

_Marked ready for review by @mishamsk on 2025-02-13 04:56_

---

_Review requested from @carljm by @mishamsk on 2025-02-13 04:56_

---

_Review requested from @MichaReiser by @mishamsk on 2025-02-13 04:56_

---

_Review requested from @AlexWaygood by @mishamsk on 2025-02-13 04:56_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4160 on 2025-02-13 11:45_

unfortunately Salsa-tracked methods with lots of parameters are not very performant. Ideally we'd find a way to reduce the number of parameters here. Doing something like this might help cut down the performance regression:

```diff
diff --git a/crates/red_knot_python_semantic/src/types.rs b/crates/red_knot_python_semantic/src/types.rs
index 7f9179aab..5be0c6535 100644
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -4155,7 +4155,6 @@ impl<'db> Class<'db> {
         fn class_symbol_by_id<'db>(
             db: &'db dyn Db,
             scope: ScopeId<'db>,
-            is_dunder_slots: bool,
             symbol_id: ScopedSymbolId,
         ) -> Symbol<'db> {
             let use_def = use_def_map(db, scope);
@@ -4217,6 +4216,13 @@ impl<'db> Class<'db> {
                     }
                     // Symbol is undeclared, return the union of `Unknown` with the inferred type
                     Ok(SymbolAndQualifiers(Symbol::Unbound, _)) => {
+                        let table = symbol_table(db, scope);
+                        // `__slots__` is a symbol with special behavior in Python's runtime. It can be
+                        // modified externally, but those changes do not take effect. We therefore issue
+                        // a diagnostic if we see it being modified externally. In type inference, we
+                        // can assign a "narrow" type to it even if it is not *declared*. This means, we
+                        // do not have to call [`widen_type_for_undeclared_public_symbol`].
+                        let is_dunder_slots = table.symbol(symbol_id).name() == "__slots__";
                         widen_type_for_undeclared_public_symbol(db, inferred, is_dunder_slots)
                     }
                 },
@@ -4224,20 +4230,10 @@ impl<'db> Class<'db> {
         }
 
         let body_scope = self.body_scope(db);
-        let table = symbol_table(db, body_scope);
-
-        // `__slots__` is a symbol with special behavior in Python's runtime. It can be
-        // modified externally, but those changes do not take effect. We therefore issue
-        // a diagnostic if we see it being modified externally. In type inference, we
-        // can assign a "narrow" type to it even if it is not *declared*. This means, we
-        // do not have to call [`widen_type_for_undeclared_public_symbol`].
-        let is_dunder_slots = name == "__slots__";
-
-        if let Some(symbol_id) = table.symbol_id_by_name(name) {
-            class_symbol_by_id(db, body_scope, is_dunder_slots, symbol_id)
-        } else {
-            Symbol::Unbound
-        }
+        symbol_table(db, body_scope)
+            .symbol_id_by_name(name)
+            .map(|symbol_id| class_symbol_by_id(db, body_scope, symbol_id))
+            .unwrap_or(Symbol::Unbound)
     }
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4180 on 2025-02-13 11:47_

```suggestion
                            Symbol::bound(declared_ty)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4177 on 2025-02-13 11:47_

```suggestion
                            // if the symbol is possibly unbound. This requires a bit of
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4189 on 2025-02-13 11:48_

```suggestion
                            Symbol::bound(declared_ty.inner_type())
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4209 on 2025-02-13 11:48_

```suggestion
                        UnionType::from_elements(db, [inferred_ty, declared_ty]),
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4216 on 2025-02-13 11:48_

```suggestion
                        Symbol::bound(ty.inner_type())
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3464 on 2025-02-13 12:01_

nit: this could be a bit shorter

```suggestion
                                "Attribute `{}` can only be accessed on instances of type `{}`, not on the class object itself.",
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3523 on 2025-02-13 12:03_

I think we can omit the word "pure" here -- assigning to _any_ instance attribute from the class object is illegal

```suggestion
                                "Cannot assign to instance attribute `{attr}` from the class object `{ty}`",
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3726 on 2025-02-13 12:08_

This logic doesn't look correct to me. This means that the error message now says "it can only be accessed on instances" for _all_ invalid attribute accesses on class objects, even when there isn't a corresponding instance attribute. E.g. if I run red-knot on this file with your branch:

```py
class Foo: ...

print(Foo.bar)
```

red-knot reports:

```
error: lint:unresolved-attribute
 --> /Users/alexw/dev/experiment/baz.py:3:7
  |
1 | class Foo: ...
2 |
3 | print(Foo.bar)
  |       ^^^^^^^ The attribute `bar` can only be accessed on instances of type `Literal[Foo]`, but not on the class object itself.
  |
```

But that message is inaccurate, because we'd also complain about accessing the attribute `bar` on instances of `Foo`. `Foo` doesn't have a ClassVar attribute `bar` _or_ an instance attribute `bar`

---

_@AlexWaygood reviewed on 2025-02-13 12:08_

Nice! I haven't done a full review, but I spotted a few things looking over quickly

---

_@mishamsk reviewed on 2025-02-13 12:19_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:4160 on 2025-02-13 12:19_

thanks. tbh I am not sure there is a point in query at all. My first version didn't have a query, and the codsped showed the same hit to performance. I intentionally matched the existing `symbol` function and it's internal query syntax to simplify merging/unifying them later.

I can apply your suggestion, but I'd just drop query. LMK

---

_@mishamsk reviewed on 2025-02-13 12:23_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types/infer.rs`:3523 on 2025-02-13 12:23_

THe word "pure" comes from @sharkdp and afaik the intended target of this PR/issue. 

Do you mean that in red_knot you want to restrict this as well:

```python
class C:
    attr: str = "class_value"

    def __init__(self):
        self.attr = "instance value"

C.attr = "other class value"
```

if so, I think it is a deviation from what the issue said, so want to double-check that's the intent here.

---

_@AlexWaygood reviewed on 2025-02-13 12:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3523 on 2025-02-13 12:25_

> Do you mean that in red_knot you want to restrict this as well:

Yes. We should allow instance attributes like that to be _accessed_ from the class object but not to be _assigned_ on the class object. On the class object, they should be readable but not writable

---

_@mishamsk reviewed on 2025-02-13 12:27_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types/infer.rs`:3726 on 2025-02-13 12:27_

The idea was to get a nice helpful message in a common case of accidentally trying to assign existing instance var via class. But you are right, I didn't think about entirely non-existent attrs.

I suggest to simplify/unify the message with the Instance arm above. To make a nice message that will distinguish this two cases would lead to doing a second symbol search through instances, probably unreasonable overhead.

@sharkdp are you ok with that?

---

_@AlexWaygood reviewed on 2025-02-13 12:27_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3523 on 2025-02-13 12:27_

And for "pure" instance attributes -- those which do not have a default value set in the class body -- they should be neither readable nor writable on the class object :-)

---

_@AlexWaygood reviewed on 2025-02-13 12:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3726 on 2025-02-13 12:30_

Helpful error messages are important when it comes to creating a tool that users enjoy running on their code! And performance doesn't matter quite so much in this specific location. Here we already know we're about to emit a diagnostic, so we only pay the cost of doing something slow here if there's a typing error in the user's code. That's hopefully pretty rare; most of the time most users' code won't have typing errors in it.

---

_@sharkdp reviewed on 2025-02-13 12:34_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3726 on 2025-02-13 12:34_

> To make a nice message that will distinguish this two cases would lead to doing a second symbol search through instances, probably unreasonable overhead.

I think I would be fine with a possible performance hit for this, especially if we can perform that lookup only if we would emit a diagnostic anyway, i.e. if we would not have to pay a cost for this if everything is fine.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:4160 on 2025-02-13 12:40_

I don't think we can drop the query. The query is necessary for better incremental computation because `symbol_from_declarations` calls into the visibility constraint evaluation that calls `node_ref` on some nodes that don't necessarily belong to the same module as the module calling `Type::member`. 

---

_@MichaReiser reviewed on 2025-02-13 12:40_

---

_@AlexWaygood reviewed on 2025-02-13 12:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4160 on 2025-02-13 12:42_

> tbh I am not sure there is a point in query at all.

I'm not totally sure TBH. It's important to use queries when we have code that might be "reaching across the module boundary" -- this helps make sure that red-knot is incremental in its type-checking, and only recomputes the things it absolutely needs to when something changes in a file. I'm not confident on whether that applies here or not. @MichaReiser is our expert on these matters.

---

_@AlexWaygood reviewed on 2025-02-13 12:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:4160 on 2025-02-13 12:42_

Ah and @MichaReiser got there before I even finished writing that message 😄

---

_@mishamsk reviewed on 2025-02-13 15:02_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:4160 on 2025-02-13 15:02_

@MichaReiser thanks, very helpful explanation. I think I now understand how you choose where to use query. At least the case of cross-boundary vs. same-file analysis.

I'll apply the suggested change here, but just FYI - `own_instance_member` should be changed too. Going back to my initial suggestion to refactor the symbol business (most recent [comment here](https://github.com/astral-sh/ruff/pull/16036#issuecomment-2649552081)). Not as part of this PR, but I'd be happy to do that if the team allows.

---

_@mishamsk reviewed on 2025-02-13 15:03_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:4180 on 2025-02-13 15:03_

ah. that's new API if I am not mistaken. Thanks for the tip

---

_@mishamsk reviewed on 2025-02-13 15:04_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types/infer.rs`:3726 on 2025-02-13 15:04_

ok. we'll do later today

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:594 on 2025-02-13 22:16_

```suggestion
# and the assignment is properly attributed to the class method.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4160 on 2025-02-13 22:52_

Great catch on `own_instance_member` needing to be a query as well, that looks right to me. (I'm also happy for you to work on improving our handling of "boundness" vs "declaredness", I think there are some issues there.)

---

_@mishamsk reviewed on 2025-02-13 22:58_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types/infer.rs`:3523 on 2025-02-13 22:58_

@AlexWaygood I applied naming changes per your suggestions. 

I did not implement the new case:

```python
class C:
    attr: str = "class_value"

    def __init__(self):
        self.attr = "instance value"

C.attr = "other class value"
```

for two reasons:

- outside of the scope of original issue
- more importantly: this would need changing member or onw_class_member API to return qualifiers. I think this will collide with what @sharkdp is doing with descriptors now. It would be much easier to add this one last case to tests & emit diagnostic when API's are changed

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:54 on 2025-02-13 23:04_

Since we are talking about the instance type, I think it would be less confusing if this error message used the instance type name (aka just the class name):

```suggestion
# error: [unresolved-attribute] "Attribute `inferred_from_value` can only be accessed on instances of type `C`, not on the class object itself."
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/expression/attribute.md`:32 on 2025-02-13 23:05_

This error message change looks like a regression that should be reversed. The new error message is misleading because it implies that accessing `non_existent` on the instance type would work, when it would not, because the attribute doesn't exist at all. (I think maybe this is also discussed in another comment thread below?)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4155 on 2025-02-13 23:09_

How different is this from `symbol_by_id`? It looks like a lot of it is copy-pasted. I'm a bit hesitant to copy-paste such a significant chunk of code, with known TODOs and all. (I realize `own_instance_member` also implements its own lookup logic, but I think that logic is much more significantly different from `symbol_by_id`.

For context, `symbol_by_id` used to be a standalone function before it was moved inside `symbol`. And it already has multiple arguments, so already pays the cost of Salsa argument interning. I don't see much reason not to move it back outside of `symbol`, and add an argument to it would allow it to serve this use case, too. Or an interesting alternative approach would be to have it return a `SymbolAndQualifiers` which might allow the logic here to be implemented externally?

At the very least, it would be easier to review and understand the differences in behavior!

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:4160 on 2025-02-13 23:12_

fixed

---

_@mishamsk reviewed on 2025-02-13 23:12_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types/infer.rs`:3726 on 2025-02-13 23:13_

this should be fixed now

---

_@mishamsk reviewed on 2025-02-13 23:13_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3743 on 2025-02-13 23:17_

I think it would be pretty easy to refactor this to avoid duplicating this default error message. Put the entire `match value_ty` inside `let bound_on_instance = match value_ty { ... }` where the fallback case assigns `false`, and then follow that with `if bound_on_instance { ... <access on instance diagnostic> ... } else { ... <default diagnostic> ... }`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3523 on 2025-02-13 23:28_

> Yes. We should allow instance attributes like that to be _accessed_ from the class object but not to be _assigned_ on the class object. On the class object, they should be readable but not writable

What's the rationale for this? It's legal at runtime, both pyright and mypy allow it, I'm not sure I see why it must be disallowed.

---

_@carljm reviewed on 2025-02-13 23:34_

Thank you!

---

_@carljm reviewed on 2025-02-13 23:34_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/expression/attribute.md`:32 on 2025-02-13 23:34_

Oops ignore, commented before you made the update.

---

_Comment by @carljm on 2025-02-13 23:36_

I suspect the performance regression on the incremental benchmark here is not avoidable. We just need more cached query results now that we distinguish more ways to resolve symbols, and more cached query results means more re-validation cost in Salsa.

---

_Comment by @AlexWaygood on 2025-02-13 23:43_

> I suspect the performance regression on the incremental benchmark here is not avoidable. We just need more cached query results now that we distinguish more ways to resolve symbols, and more cached query results means more re-validation cost in Salsa.

Note that we haven't got up to date benchmark numbers on this PR right now, as the assertion in the benchmark regarding the number of diagnostics has been failing for the last few commits, so codspeed has not been running 

---

_Comment by @carljm on 2025-02-13 23:52_

> I suspect the performance regression on the incremental benchmark here is not avoidable. We just need more cached query results now that we distinguish more ways to resolve symbols, and more cached query results means more re-validation cost in Salsa.

I'm also questioning this conclusion -- I think if we were able to make `symbol` return a `SymbolAndQualifiers` and that were enough to let us check for classvars externally, that might help address the perf impact here, since it would mean we could do this without increasing the number of cached memos (instead we'd just be slightly increasing the data stored in each one, which is not as expensive in Salsa incremental re-validation).

---

_@mishamsk reviewed on 2025-02-14 11:05_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:54 on 2025-02-14 11:05_

@carljm I totally agree. But this is a shared display logic. I originally included another change in this PR related to displaying classes & unions, which touched a lot of mdtest, but reverted it per @sharkdp request. Now I am hesitant to include anything like that in this PR. I doubt that special-casing for this single instance would be welcomed either => I'd rather leave message improvements to another PR and look at them more holistically

---

_@mishamsk reviewed on 2025-02-14 11:07_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:4155 on 2025-02-14 11:07_

Wholeheartedly agree. I suggested refactoring the whole symbol thing. I also think it should return `SymbolAndQualifiers` and that the `Symbol` enum should be changed to push the logic of combining declared and inferred types to the caller. Does everyone want me to do this in this PR though? @AlexWaygood @sharkdp 

---

_@mishamsk reviewed on 2025-02-14 11:08_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types/infer.rs`:3523 on 2025-02-14 11:08_

btw. I didn't argue with @AlexWaygood but I am with @carljm - IMHO there is no reason to forbid that. It was not and is not apparent to me

---

_@AlexWaygood reviewed on 2025-02-14 13:13_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3523 on 2025-02-14 13:13_

> What's the rationale for this? It's legal at runtime, both pyright and mypy allow it, I'm not sure I see why it must be disallowed.

It's something I'd want a type checker to flag if I'm running a type checker on my Python code. Sure, it's legal at runtime, but it seems like it's probably a user mistake to me if the variable has been explicitly declared as an instance attribute but it's being modified on the class object rather than an instance. Having said all this, your pushback makes me think that the error should probably be disabled by default, since you're right that it isn't really _unsafe_ for an instance attribute with a class-level default to be assigned on the class object.

---

_@mishamsk reviewed on 2025-02-14 13:56_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types/infer.rs`:3523 on 2025-02-14 13:56_

As of current PR it will not emit a diagnostic. Do you suggest adding a configuration and implement it under a flag? Seems like a reasonable compromise. 

I haven't researched knot configuration capabilities yet. Maybe a follow up?

---

_@AlexWaygood reviewed on 2025-02-14 13:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3523 on 2025-02-14 13:58_

@mishamsk you don't need to worry about it for now: it's not implemented on `main`, and it shouldn't be implemented as part of this PR :-) I was just responding to @carljm's (very reasonable) question about my earlier assertion. It could absolutely be a followup, but first we need to reach agreement on whether it's something we do indeed want ;-) and it might not be a priority for us to implement in any case

---

_Comment by @MichaReiser on 2025-02-14 15:00_

I haven't reviewed this PR yet because there are already a lot of folks involved. Feel free to ping me if or when a review from my side would be good.

---

_@sharkdp reviewed on 2025-02-14 18:38_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:54 on 2025-02-14 18:38_

> but reverted it per @sharkdp request.

This previous change affected how we displayed class literal types in general (`type[C]` instead of `Literal[C]`), and I pushed back against this change.

But here, we don't want to change how class literal types are displayed, we rather want some some special handling that turns the `Type::ClassLiteral(class)` type into a `Type::Instance(class)` type before displaying. I am not sure if that is an easy change, though, since we might also need to handle unions etc.?

If it turns out to be more complicated, we could also work around this for now by rephrasing that sentence a bit, e.g. "can only be accessed on instances, not on the class object `Literal[C]` itself."?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:54 on 2025-02-14 22:46_

Tweaking the wording of a single diagnostic message, which is newly introduced in this PR, is quite different from wholesale changes to the display of some kinds of types. I am suggesting the former, not the latter.

I think the change I suggested is pretty easy to implement (and doesn't even require a general type transform using `to_instance()`) because it is already the case that we only emit this specialized diagnostic if the receiver type is exactly a `ClassLiteral` or `SubclassOf` type (not e.g. a union). So we have access to the `class` and could just use its name.

But that said, I like @sharkdp 's suggestion even better, to just reword the message so we mention the actual type where we are discussing "the class object itself" rather than where we are discussing instances. This rewording makes sense, because it is the class type, not the instance type, that we have at hand. It's also the way the "assignment" version of this same diagnostic is already worded.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4155 on 2025-02-14 22:48_

Yes, I think it would make sense to pursue that refactor in this PR; I don't think there is any particular urgency to quickly land a copy-paste version of the functionality. I think it's safe to interpret the thumbs-ups above as team agreement :)

---

_@carljm reviewed on 2025-02-14 22:48_

---

_@carljm reviewed on 2025-02-14 22:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4160 on 2025-02-14 22:57_

https://github.com/astral-sh/ruff/issues/16172 to track the `own_instance_member` issue

---

_@mishamsk reviewed on 2025-02-14 23:02_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:4155 on 2025-02-14 23:02_

Ok. I have a short holiday this weekend, we'll get back to it Monday 

---

_Renamed from "[red-knot] Add diagnostic for class-object access to pure instance variables and class literal repr in diagnostics" to "[red-knot] Add diagnostic for class-object access to pure instance variables" by @AlexWaygood on 2025-02-16 11:14_

---

_@mishamsk reviewed on 2025-02-19 01:59_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types/infer.rs`:3743 on 2025-02-19 01:59_

yes, indeed, thanks. applied this and merged with changes from #16160

---

_@mishamsk reviewed on 2025-02-19 02:04_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:54 on 2025-02-19 02:04_

applied the suggested wording

---

_@mishamsk reviewed on 2025-02-19 02:05_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:594 on 2025-02-19 02:05_

fixed

---

_@mishamsk reviewed on 2025-02-19 04:13_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:4155 on 2025-02-19 04:13_

@carljm I spent some time today, trying to find a suitable unification between 3 different symbol search queries, and haven't landed on a design yet. I am not sure it makes sense, but I'll do another pass tomorrow before a final verdict.

One issue with keeping this refractor and #16172 in scope of this PR is that main is moving forward quite fast. E.g. some of the code I've used moved to symbol.rs mod as a private function after #16152 and the symbol implementation itself was changed.

Not sure how many other conflicting PRs are in the pipe, I'll do my best to quickly create my version of refactoring in hopes that no new divergence occurs.

---

_@carljm reviewed on 2025-02-19 22:31_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4155 on 2025-02-19 22:31_

Oh I don't think #16172 needs to be handled in this PR, sorry if I mis-communicated there. It's a pre-existing problem, and I filed it as an issue specifically because I don't think it needs to be fixed in this PR.

It's true that main can move fast and sometimes we have to handle conflicts in a PR, sometimes multiple times; it's something we've all had to do.

I'm not sure what you're referring to when you say "unification between 3 different symbol search queries" -- I thought I was only suggesting unification of two (the new class-symbol one you've added here, and the main `symbol_by_id` used for public types in general). Those are the two that look mostly like copy-paste of each other. I called out `own_instance_member` above as being quite different from those two, and thus _not_ likely a candidate for unification, certainly not in this PR.

---

_@mishamsk reviewed on 2025-02-21 03:51_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types.rs`:4155 on 2025-02-21 03:51_

@carljm I've unified the two queries. And then made `symbol_by_id` a regular function in a separate commit (easy to revert) after reading #16268 and given that `symbol_by_id` now had to return a bigger structure and thus query performance would be reduced.

tbh. it feels a bit wrong and suboptimal due to the whole Boundness not always meaning boundness. But since redesigning of this is out of scope and you asked me to unify the two logics - I did what I think is the best intermediary solutions.

If you think it looks good, I would re-request review from the wider group

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/symbol.rs`:213 on 2025-02-21 17:55_

I see what you mean about how this is sub-optimal until we make `Symbol` reflect both boundness and declaredness separately; ideally we would not have to re-query bindings here, but would have all the necessary information in `Symbol`.

I'm open to any of several options here:
1) Add a TODO comment here (basically reflecting the above paragraph), go ahead and land this, and then pursue clarification of boundness vs declaredness as a separate PR.
2) Leave this PR on ice for now, go work on boundness vs declaredness, then come back to this after that is landed. This is perhaps the "cleanest" option, but it definitely does risk more rebase-conflicts in this PR.
3) Do it all in this PR. (I think this is my least favorite option, as it risks really metastasizing this PR with too many concerns. But I'm not closed to it.)

Which option do you prefer?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3666 on 2025-02-21 18:12_

I think we could even be a little stricter here and mark this branch `unreachable!`? Because `type[Any]` has all attributes, so we can never get `Unbound` back from getting a member off it.

The main advantage would be that in some sense neither `true` nor `false` seems right here, so it might make the code a bit clearer, and it would mean we fail fast if we violate this invariant. The disadvantage would be that if we somehow ship a very subtle violation of the invariant, it could cause a panic for a user.

I don't feel strongly about using `unreachable!`, open to whatever you prefer. But if we don't use `unreachable!`, I feel like `true` makes slightly more sense here than `false`, since `Any` also has all members.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3747 on 2025-02-21 18:12_

This all looks under-indented by one level? I think rustfmt struggles with the size of `infer.rs`, we need to break it up :/

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3749 on 2025-02-21 18:14_

If we use `unreachable!` above, we could do so here as well.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/symbol.rs`:410 on 2025-02-21 18:37_

It looks like we lost the `#[salsa::tracked]` annotation on this function. It's possible that we don't need it; we should see what the benchmarks say. If it is still a win to track this function, it also requires adding some trait derives on `Symbol` and `SymbolAndQualifiers`:

```diff
diff --git a/crates/red_knot_python_semantic/src/symbol.rs b/crates/red_knot_python_semantic/src/symbol.rs
index 63fd67203..d141c230a 100644
--- a/crates/red_knot_python_semantic/src/symbol.rs
+++ b/crates/red_knot_python_semantic/src/symbol.rs
@@ -37,7 +37,7 @@ pub(crate) enum Boundness {
 /// possibly_unbound:  Symbol::Type(Type::IntLiteral(2), Boundness::PossiblyUnbound),
 /// non_existent:      Symbol::Unbound,
 /// ```
-#[derive(Debug, Clone, PartialEq, Eq)]
+#[derive(Debug, Clone, PartialEq, Eq, salsa::Update)]
 pub(crate) enum Symbol<'db> {
     Type(Type<'db>, Boundness),
     Unbound,
@@ -365,7 +365,7 @@ pub(crate) type SymbolFromDeclarationsResult<'db> =
 /// that this comes with a [`CLASS_VAR`] type qualifier.
 ///
 /// [`CLASS_VAR`]: crate::types::TypeQualifiers::CLASS_VAR
-#[derive(Debug)]
+#[derive(Clone, Debug, Eq, PartialEq, salsa::Update)]
 pub(crate) struct SymbolAndQualifiers<'db>(pub(crate) Symbol<'db>, pub(crate) TypeQualifiers);

 impl SymbolAndQualifiers<'_> {
@@ -395,6 +395,7 @@ impl<'db> From<Symbol<'db>> for SymbolAndQualifiers<'db> {
     }
 }

+#[salsa::tracked]
 fn symbol_by_id<'db>(
     db: &'db dyn Db,
     scope: ScopeId<'db>,
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/symbol.rs`:187 on 2025-02-21 18:45_

We aren't getting any benchmark updates on this PR; the benchmark is failing to run because our assertion of the diagnostic output no longer matches. It looks like there is one new false positive on the line `TOMLDecodeError.__module__ = __name__`, where we think `__module__` is an instance attribute and therefore can't be set on the class. The attribute is defined in typeshed in `builtins.pyi` (on `object`) as just `__module__: str`. It seems to me like this should probably be defined in typeshed as a `ClassVar`? But maybe in a stub file we just need to consider all annotated assignments as "bound" even if they aren't (because the RHS is typically omitted in a stub file)? (This would dovetail with https://github.com/astral-sh/ruff/issues/16264 which is the same thing, but at module level instead of in class bodies; maybe we should just do it everywhere). Curious what you think about this, @AlexWaygood.

I'm OK with adding this false positive to the benchmark expected output for now and dealing with it as a separate follow-up. Here's the diff needed to get the benchmark running again (you can test locally with `cargo bench --bench red_knot`):

```diff
diff --git a/crates/ruff_benchmark/benches/red_knot.rs b/crates/ruff_benchmark/benches/red_knot.rs
index 11ec12577..beed5bd96 100644
--- a/crates/ruff_benchmark/benches/red_knot.rs
+++ b/crates/ruff_benchmark/benches/red_knot.rs
@@ -96,6 +96,13 @@ static EXPECTED_DIAGNOSTICS: &[KeyDiagnosticFields] = &[
         Cow::Borrowed("Unused blanket `type: ignore` directive"),
         Severity::Warning,
     ),
+    (
+        DiagnosticId::lint("invalid-attribute-access"),
+        Some("/src/tomllib/__init__.py"),
+        Some(270..296),
+        Cow::Borrowed("Cannot assign to instance attribute `__module__` from the class object `Literal[TOMLDecodeError]`"),
+        Severity::Error,
+    ),
 ];

 fn tomllib_path(file: &TestFile) -> SystemPathBuf {
```

---

_@carljm reviewed on 2025-02-21 18:46_

This looks pretty good to me! I see your point about how it's sub-optimal until we address bound vs declared in `Symbol`, left an inline comment about that.

I think we need to get the benchmark running so we can see the perf impact of this, and evaluate the impact of tracking or not tracking `symbol_by_id`. But otherwise this looks good.

Sorry about the rebase conflicts; this was a particularly fast-moving week! Thanks for working on this!


---

_@mishamsk reviewed on 2025-02-21 19:31_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/symbol.rs`:213 on 2025-02-21 19:31_

Would be great to merge this with a TODO and I can take a stab at boundness/declaredness refactor with the team permission.

This PR already has 70+ comments, I am afraid it will become harder and harder to review if we keep it open.

---

_@mishamsk reviewed on 2025-02-21 19:39_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types/infer.rs`:3747 on 2025-02-21 19:39_

yeah, unfortunately rustfmt is not as good as ruff formatter for python! ;-)

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types/infer.rs`:3666 on 2025-02-21 20:00_

checked the codebase, seems like unreachable is being used in a ocuple of other places and also verified this invariant (mostly for myself). I think it is reasonable to use unreachable here. Some tests will fail quickly if it stops holding true.

---

_@mishamsk reviewed on 2025-02-21 20:00_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/symbol.rs`:187 on 2025-02-21 20:08_

@carljm ran benches locally.

current version (no query):
![CleanShot 2025-02-21 at 15 07 45@2x](https://github.com/user-attachments/assets/5edfdd9f-56e5-476b-a047-6cd53e073ff9)

With query:
![CleanShot 2025-02-21 at 15 07 01@2x](https://github.com/user-attachments/assets/5e66eb2c-7805-4df6-abfd-535256f46078)

so I'll push non-query version as it seems either the same or slightly faster

---

_@mishamsk reviewed on 2025-02-21 20:08_

---

_@mishamsk reviewed on 2025-02-21 20:08_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/symbol.rs`:410 on 2025-02-21 20:08_

see comment below, closing this thread

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/symbol.rs`:187 on 2025-02-21 20:28_

That seems fine to me. I think this is safe to do now that we have https://github.com/astral-sh/ruff/pull/16268. @MichaReiser do you have any reason that we should keep `symbol_by_id` tracked, if it doesn't seem to be a perf win to track it?

---

_@carljm reviewed on 2025-02-21 20:28_

---

_Comment by @github-actions[bot] on 2025-02-21 20:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @carljm on 2025-02-21 20:46_

[CodSpeed](https://codspeed.io/astral-sh/ruff/branches/mishamsk%3A15963-fix-diagnostic-for-pure-instance-variables-access-from-class) says this is a 1% cold and 3% incremental regression. That's improved from 5.5% incremental regression in the prior version. I think the current regression is acceptable and within the reasonable range for the added functionality here. It may be that we get some improvement from resolving the TODO around re-resolving bindings.

---

_@AlexWaygood reviewed on 2025-02-21 20:49_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:187 on 2025-02-21 20:49_

I believe Micha experimented with removing the `#[tracked]` attribute on `symbol_by_id` in #16268, but Codspeed indicated that incremental performance was still much better if it was `#[tracked]`. Take a look at the individual commits in that PR and https://github.com/astral-sh/ruff/pull/16268#issuecomment-2672155862

---

_@carljm reviewed on 2025-02-21 20:52_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/symbol.rs`:187 on 2025-02-21 20:52_

Hmm, it may be worth actually pushing up a commit that re-adds this and see what CodSpeed says (vs local run of the benchmark).

But it's also possible that perf characteristics have changed since Micha's experiment, due to other changes (or the changes in this PR.)

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/symbol.rs`:187 on 2025-02-21 21:14_

you got it. just pushed. let's see what we get

---

_@mishamsk reviewed on 2025-02-21 21:14_

---

_@mishamsk reviewed on 2025-02-21 21:20_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/symbol.rs`:187 on 2025-02-21 21:20_

technically slightly faster, although I'd say these number are probably well within margin of error. I'll keep the commit and wait for a final verdict from @MichaReiser 

---

_@AlexWaygood reviewed on 2025-02-21 21:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:187 on 2025-02-21 21:20_

It looks like re-adding it _might_ have reduced the perf regression on the incremental benchmark from 3% to 2%? (Though that's in the zone where it could just be noise!)

---

_@sharkdp reviewed on 2025-02-24 10:21_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/symbol.rs`:187 on 2025-02-24 10:21_

That would be consistent with what @MichaReiser and I found last week: Having `symbol_by_id` be a query helps with performance.

---

_@mishamsk reviewed on 2025-02-24 10:56_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/symbol.rs`:187 on 2025-02-24 10:56_

so this one is good to be merged then? the current commit has `symbol_by_id` as a query

---

_@sharkdp reviewed on 2025-02-24 12:58_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/symbol.rs`:438 on 2025-02-24 12:58_

What happened to the `is_final` check here? Maybe we don't have any tests for this, but I think we currently handled `Final` symbols correctly, by not unioning with `Unknown`, because they can not be assigned to from a different scope.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:95 on 2025-02-24 13:17_

it would be useful to retain a comment here that mypy and pyright do not show an error in this case, but we deliberately deviate from them

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:99 on 2025-02-24 13:18_

same here, it would be good to keep some version of this comment even if it's no longer a TODO

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:120 on 2025-02-24 13:18_

and here

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/expression/attribute.md`:32 on 2025-02-24 13:18_

spurious change here. Best to leave this file alone

```suggestion
    # error: [unresolved-attribute] "Type `Literal[A]` has no attribute `non_existent`"
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3712 on 2025-02-24 13:20_

there's a `let db = self.db();` assignment a few lines higher so that this method doesn't have to have lots of verbose `self.db()` calls everywhere

```suggestion
                        !class.instance_member(db, attr).0.is_unbound()
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3717 on 2025-02-24 13:20_

```suggestion
                                !class.instance_member(db, attr).0.is_unbound()
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3719 on 2025-02-24 13:24_

a couple of nits:

- Lookup on a dynamic type won't always return a bound symbol -- e.g. `tuple[Any, Any]` is a dynamic type, but looking up the attribute "foo" on `tuple[Any, Any]` won't return a bound symbol. This is specifically true for dynamic `SubclassOf` types, but not true in general for _all_ dynamic types.
- It won't necessarily return a bound symbol of the same _type_. That might be what we have right now, but in the long term, for example, `str & Any` might be a better answer for the type of the `__module__` attribute on an object with type `type[Any]`.

```suggestion
                            ClassBase::Dynamic(_) => unreachable!("Attribute lookup on a dynamic `SubclassOf` type should always return a bound symbol"),
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3734 on 2025-02-24 13:25_

some overindentation here

```suggestion
                        &UNRESOLVED_ATTRIBUTE,
                        attribute,
                        format_args!(
                            "Attribute `{}` can only be accessed on instances, not on the class object `{}` itself.",
                            attr.id,
                            value_type.display(db)
                        ),
                    );
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3741 on 2025-02-24 13:25_

spurious change

```suggestion
                            value_type.display(db),
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3788 on 2025-02-24 13:26_

some underindentation here

```suggestion
                                &INVALID_ATTRIBUTE_ACCESS,
                                attribute,
                                format_args!(
                                    "Cannot assign to ClassVar `{attr}` from an instance of type `{ty}`",
                                    ty = value_ty.display(self.db()),
                                ),
                            );
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3802 on 2025-02-24 13:27_

```suggestion
                                        ClassBase::Dynamic(_) => unreachable!("Attribute lookup on a dynamic `SubclassOf` type should always return a bound symbol"),
```

---

_@AlexWaygood reviewed on 2025-02-24 13:27_

thanks again, and sorry we keep creating merge conflicts for you here!

---

_@AlexWaygood reviewed on 2025-02-24 13:28_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:187 on 2025-02-24 13:28_

yes, I think this thread can be resolved now :-)

---

_Comment by @sharkdp on 2025-02-24 13:55_

@mishamsk  I can look into making the required changes to get this merged — thank you very much for your work!

---

_@mishamsk reviewed on 2025-02-24 14:05_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/symbol.rs`:438 on 2025-02-24 14:05_

@sharkdp I removed it because it didn't make sense. It was impossible for final qualifier to be set in the branch where it was used

---

_@sharkdp reviewed on 2025-02-24 14:05_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/symbol.rs`:438 on 2025-02-24 14:05_

I think it's okay to remove this for now. I am struggling to write a meaningful test for this. Let's just re-introduce this when we support final. I'm also removing the `SymbolAndQualifiers::is_final` method in the meantime.

---

_Comment by @mishamsk on 2025-02-24 14:07_

> @mishamsk I can look into making the required changes to get this merged — thank you very much for your work!

thanks. I was going to say that I am happy to apply the requested changes myself, but looks like it is too late ;-)

---

_@sharkdp approved on 2025-02-24 14:14_

Thank you very much. The final benchmarks show a -1% regression on the incremental setup.

---

_Merged by @sharkdp on 2025-02-24 14:17_

---

_Closed by @sharkdp on 2025-02-24 14:17_

---

_Comment by @mishamsk on 2025-02-24 17:35_

@sharkdp thanks for pushing this over the edge. YaY 

---

_Branch deleted on 2025-02-27 01:25_

---

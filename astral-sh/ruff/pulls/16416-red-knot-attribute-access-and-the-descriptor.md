```yaml
number: 16416
title: "[red-knot] Attribute access and the descriptor protocol"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/descriptor-protocol-pt2
created_at: 2025-02-27T15:29:03Z
updated_at: 2025-03-12T10:03:18Z
url: https://github.com/astral-sh/ruff/pull/16416
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Attribute access and the descriptor protocol

---

_Pull request opened by @sharkdp on 2025-02-27 15:29_

## Summary

* Attributes/method are now properly looked up on metaclasses, when called on class objects
* We properly distinguish between data descriptors and non-data descriptors (but we do not yet support them in store-context, i.e. `obj.data_descr = …`)
* The descriptor protocol is now implemented in a single unified place for instances, classes and dunder-calls. Unions and possibly-unbound symbols are supported in all possible stages of the process by creating union types as results.
* In general, the handling of "possibly-unbound" symbols has been improved in a lot of places: meta-class attributes, attributes, descriptors with possibly-unbound `__get__` methods, instance attributes, …
* We keep track of type qualifiers in a lot more places. I anticipate that this will be useful if we import e.g. `Final` symbols from other modules (see relevant change to typing spec: https://github.com/python/typing/pull/1937).
* Detection and special-casing of the `typing.Protocol` special form in order to avoid lots of changes in the test suite due to new `@Todo` types when looking up attributes on builtin types which have `Protocol` in their MRO. We previously 
looked up attributes in a wrong way, which is why this didn't come up before.

closes #16367
closes #15966

## Context

The way attribute lookup in `Type::member` worked before was simply wrong (mostly my own fault). The whole instance-attribute lookup should probably never have been integrated into `Type::member`. And the `Type::static_member` function that I introduced in my last descriptor PR was the wrong abstraction. It's kind of fascinating how far this approach took us, but I am pretty confident that the new approach proposed here is what we need to model this correctly.

There are three key pieces that are required to implement attribute lookups:

- **`Type::class_member`**/**`Type::find_in_mro`**: The `Type::find_in_mro` method that can look up attributes on class bodies (and corresponding bases). This is a partial function on types, as it can not be called on instance types like`Type::Instance(…)` or `Type::IntLiteral(…)`. For this reason, we usually call it through `Type::class_member`, which is essentially just `type.to_meta_type().find_in_mro(…)` plus union/intersection handling.
- **`Type::instance_member`**: This new function is basically the type-level equivalent to `obj.__dict__[name]` when called on `Type::Instance(…)`. We use this to discover instance attributes such as those that we see as declarations on class bodies or as (annotated) assignments to `self.attr` in methods of a class.
- The implementation of the descriptor protocol. It works slightly different for instances and for class objects, but it can be described by the general framework:
  - Call `type.class_member("attribute")` to look up "attribute" in the MRO of the meta type of `type`. Call the resulting `Symbol` `meta_attr` (even if it's unbound).
  - Use `meta_attr.class_member("__get__")` to look up `__get__` on the *meta type* of `meta_attr`. Call it with `__get__(meta_attr, self, self.to_meta_type())`. If this fails (either the lookup or the call), just proceed with `meta_attr`. Otherwise, replace `meta_attr` in the following with the return type of `__get__`. In this step, we also probe if a `__set__` or `__delete__` method exists and store it in `meta_attr_kind` (can be either "data descriptor" or "normal attribute or non-data descriptor").
  - Compute a `fallback` type.
    - For instances, we use `self.instance_member("attribute")`
    - For class objects, we use `class_attr = self.find_in_mro("attribute")`, and then try to invoke the descriptor protocol on `class_attr`, i.e. we look up `__get__` on the meta type of `class_attr` and call it with `__get__(class_attr, None, self)`. This additional invocation of the descriptor protocol on the fallback type is one major asymmetry in the otherwise universal descriptor protocol implementation.
  - Finally, we look at `meta_attr`, `meta_attr_kind` and `fallback`, and handle various cases of (possible) unboundness of these symbols.
    - If `meta_attr` is bound and a data descriptor, just return `meta_attr`
    - If `meta_attr` is not a data descriptor, and `fallback` is bound, just return `fallback`
    - If `meta_attr` is not a data descriptor, and `fallback` is unbound, return `meta_attr`
    - Return unions of these three possibilities for partially-bound symbols.

This allows us to handle class objects and instances within the same framework. There is a minor additional detail where for instances, we do not allow the fallback type (the instance attribute) to completely shadow the non-data descriptor. We do this because we (currently) don't want to pretend that we can statically infer that an instance attribute is always set.

Dunder method calls can also be embedded into this framework. The only thing that changes is that *there is no fallback type*. If a dunder method is called on an instance, we do not fall back to instance variables. If a dunder method is called on a class object, we only look it up on the meta class, never on the class itself.

## Test Plan

New Markdown tests.

---

_Label `red-knot` added by @sharkdp on 2025-02-27 15:29_

---

_Comment by @codspeed-hq[bot] on 2025-02-28 12:58_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fdescriptor-protocol-pt2)

### Merging #16416 will **degrade performances by 4.29%**

<sub>Comparing <code>david/descriptor-protocol-pt2</code> (689053e) with <code>main</code> (a18d8bf)</sub>



### Summary

`❌ 1 (👁 1)` regressions  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` red_knot_check_file[incremental] `` | 5.2 ms | 5.5 ms | -4.29% |


---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/annotations/stdlib_typing_aliases.md`:73 on 2025-03-03 10:33_

The other MROs below are not updated, as e.g. `dict` has a `MutableMapping` base which is defined using a `_alias = _SpecialGenericAlias`:
```pyi
MutableMapping = _alias(collections.abc.MutableMapping, 2)
```

---

_@sharkdp reviewed on 2025-03-03 10:33_

---

_@sharkdp reviewed on 2025-03-03 10:38_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/getattr_static.md`:62 on 2025-03-03 10:38_

Would be easy to fix this (special case `int.real` similar to how we do in `Type::member`), but since we don't really depend on `static_member` / `getattr_static` anymore, I didn't think it would be particularly important.

---

_@sharkdp reviewed on 2025-03-03 10:40_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:154 on 2025-03-03 10:40_

This was required to avoid a salsa cycle. See related discussion https://github.com/astral-sh/ruff/pull/16428#discussion_r1975823169

---

_@sharkdp reviewed on 2025-03-03 10:41_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/suppressions/type_ignore.md`:49 on 2025-03-03 10:41_

@MichaReiser I had to reformulate this test a bit, but I hope it (still) tests the right thing.

---

_@MichaReiser reviewed on 2025-03-03 11:00_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/suppressions/type_ignore.md`:49 on 2025-03-03 11:00_

I don't think it does because it no longer asserts that the diagnostic can be suppressed by a suppression comment on the line the expression start/ends (the `2 + "test"` expression now starts/ends at the same line).

Can we keep the binary expression over two lines?

```suggestion
y = (
    # error: [unsupported-operator]
    cast(
        int,
        2 + 
        	"test",  # type: ignore
    )
    + "other"
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/suppressions/type_ignore.md`:49 on 2025-03-03 11:01_

To verify if your test works as expected, comment out line 401

https://github.com/astral-sh/ruff/blob/87668e24b1e0e20c784f3728cfd528a78f671e04/crates/red_knot_python_semantic/src/suppression.rs#L400-L401

---

_@MichaReiser reviewed on 2025-03-03 11:01_

---

_@MichaReiser reviewed on 2025-03-03 11:09_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/suppressions/type_ignore.md`:49 on 2025-03-03 11:09_

What's important (and I think you got that right) is that the inner `type: ignore` shouldn't suppress the error from adding `int` and `"other"`

---

_Renamed from "[red-knot] Descriptor protocol: Data descriptors" to "[red-knot] Attribute access and the Descriptor protocol" by @sharkdp on 2025-03-03 13:50_

---

_@AlexWaygood reviewed on 2025-03-03 15:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/stdlib_typing_aliases.md`:73 on 2025-03-03 15:23_

I think that's what the runtime definition is but IIRC the typeshed definition of `MutableMapping` is pretty different?

---

_@sharkdp reviewed on 2025-03-03 15:40_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/annotations/stdlib_typing_aliases.md`:73 on 2025-03-03 15:40_

Oh, yes — of course. Tricked myself by using Pylance go-to-definition :upside_down_face:. Then it's probably related to `MutableMapping`/`Mapping` having `Generic` in their MRO.

---

_Renamed from "[red-knot] Attribute access and the Descriptor protocol" to "[red-knot] Attribute access and the descriptor protocol" by @sharkdp on 2025-03-04 12:43_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:103 on 2025-03-04 14:19_

unrelated to any changes here, this was just an oversight.

---

_@sharkdp reviewed on 2025-03-04 14:19_

---

_@AlexWaygood reviewed on 2025-03-05 21:46_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/getattr_static.md`:62 on 2025-03-05 21:46_

This actually seems like it makes our inference more accurate, since `int.real` is a descriptor:

```pycon
Python 3.11.4 (main, Sep 30 2023, 10:54:38) [GCC 11.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import inspect
>>> inspect.getattr_static(1, "real")
<attribute 'real' of 'int' objects>
>>>
```

---

_@sharkdp reviewed on 2025-03-06 08:57_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/dunder.md`:96 on 2025-03-06 08:57_

This is more of a regression test for something that was broken previously, but I can also remove it again if we don't think it adds any value.

---

_@sharkdp reviewed on 2025-03-06 12:20_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/suppressions/type_ignore.md`:49 on 2025-03-06 12:20_

After talking about this, we concluded that the modification here is fine. The other property ("can be suppressed by a suppression comment on the line the expression start/ends") is already tested above.

---

_@sharkdp reviewed on 2025-03-06 13:18_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/getattr_static.md`:62 on 2025-03-06 13:18_

You're right!

---

_Marked ready for review by @sharkdp on 2025-03-06 16:29_

---

_Review requested from @carljm by @sharkdp on 2025-03-06 16:29_

---

_@sharkdp reviewed on 2025-03-06 16:31_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3653 on 2025-03-06 16:31_

The changes here are a bit unfortunate. I have moved `or_fall_back_to` from `Symbol` to `SymbolAndQualifiers`, as that was required for most of the call sites. But here, and below, just using `Symbol` would be enough. There are no qualifiers, as we're dealing with types from bindings here. I'm happy to look into this — either here or in a follow up.

---

_@sharkdp reviewed on 2025-03-06 16:32_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1606 on 2025-03-06 16:32_

This is something that's potentially worth discussing.

---

_@sharkdp reviewed on 2025-03-06 16:34_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1718 on 2025-03-06 16:34_

Iterating over the union elements twice here just to be able to use `map_with_boundness` is not great. I think we might need a more generic `map`/`fold` function on unions and intersections. We often have to "union" other meta data as well, not just boundness, but also type qualifiers or above: whether or not a symbol is considered a data descriptor.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/stdlib_typing_aliases.md`:73 on 2025-03-06 18:56_

If I'm understanding this TODO correctly, I think we can remove it, I don't think we would ever want to do that. Precise inference of `__mro__` is really mostly for our own testing purposes, not because real code needs it. We need our MRO to be accurate for attribute/method resolution. In the case of typeshed, it should be accurate to what typeshed says, not to runtime. Put otherwise: when typeshed lies to us, we should just believe it :)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:992 on 2025-03-07 00:22_

The `Unknown` here comes from it being possibly undeclared?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/dunder.md`:96 on 2025-03-07 00:26_

Seems useful to me.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:267 on 2025-03-07 00:44_

The type annotation `type[C | D | E]` is a bit odd here, and the tests here don't appear to depend on it? Conventional practice is to leave the `cls` / `self` arg unannotated, I would probably do that unless we are specifically testing something about the behavior of this annotation.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/scoping.md`:235 on 2025-03-07 00:47_

```suggestion
    # TODO: error for reuse of typevar
    class Bad1[T]: ...
    # TODO: no non-subscriptable error, error for reuse of typevar
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/subscript/tuple.md`:121 on 2025-03-07 00:48_

Similar to above, not sure we need this todo?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:38 on 2025-03-07 00:57_

The error here appears to be precisely the one the TODO comment says we want, which is a bit strange. But I guess we are currently getting that error for the wrong reason (we are looking at return type of `__get__`, not argument type of `__set__`)?

I still think we can remove the TODO comment as there is nothing wrong with the test.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:640 on 2025-03-07 01:15_

😵 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1423 on 2025-03-07 02:00_

```suggestion
                    // return type otherwise, and since `find_name_in_mro` is usually called via `class_member`, this is not
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1502 on 2025-03-07 02:07_

Why is this clause necessary/correct? Doesn't it bypass all attributes/methods defined on `builtins.type` in typeshed?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1545 on 2025-03-07 04:48_

Is this right? I would have thought `c` in this example should be an "instance attribute with class fallback" and should be discovered by this method. Only one with `ClassVar` would not be?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1679 on 2025-03-07 05:17_

There seems to be an assumption made in this method that whatever qualifiers apply to the symbol `attribute` should also apply to its `__get__` method. I'm not surprised if this makes no difference currently due to our limited use of qualifiers, but it doesn't seem correct to me.

It also makes me wonder if this really should take `SymbolAndQualifiers`, or should just take `&self` (which is the type of attribute) and let the caller manage qualifiers.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4753 on 2025-03-07 13:13_

I'm not sure this has much impact yet since our use of intersections is limited, but the boundness logic here doesn't seem right to me. Let's say we have a type `X & Y` and we are mapping over an attribute access `.foo`. If the `foo` attribute is definitely bound on `X` and unbound on `Y`, it is definitely bound on `X & Y`. In general the boundness of an operation on an intersection should be the "maximum" boundness of the operation on the elements.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:341 on 2025-03-07 13:15_

Nit: we don't "skip protocols", we "skip Protocol" -- it is the base type `Protocol` itself that we skip, not "protocols" (which is all classes inheriting `Protocol`).

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:416 on 2025-03-07 13:19_

```suggestion
                    // TODO: We currently skip Protocol when looking up instance members, in order to
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:587 on 2025-03-07 13:23_

I'm not sure about this case. If it's not explicitly marked as `ClassVar`, shouldn't we consider this a possible instance attribute, which _might_ be found in `__dict__`? (We would allow assignment of it on an instance.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1863 on 2025-03-07 13:25_

I'm curious about the switch from taking `&str` to taking a `Name`; is this motivated by performance? It makes a number of callsites more verbose, and it seems like it might require more string cloning?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3653 on 2025-03-07 13:27_

It seems like it would be good to be able to do this operation on a `Symbol` without being required to provide qualifiers -- but I think it's fine to address this in a follow-up.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3903 on 2025-03-07 13:28_

The extra `clone` in places like this seems unfortunate, and makes me wonder why we changed `member` to take `Name` instead of `&str`.

---

_@carljm approved on 2025-03-07 13:29_

This is excellent! Left some comments but IMO nothing that can't be left as a TODO and addressed as follow up; probably better to land this sooner rather than later.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:712 on 2025-03-07 13:36_

Nit: "Metaclass and meta-type" are spelled somewhat inconsistently throughout this PR. I'd always spell it as "metaclass" rather than "meta class" and "meta-type" rather than "meta type"

```suggestion
object first, i.e. on the metaclass:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:791 on 2025-03-07 13:43_

```suggestion
If the (meta)class is a union type or if the attribute on the (meta)class has a union type, we
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/getattr_static.md`:75 on 2025-03-07 13:49_

Do we have any tests covering the fact that attributes on a class's metaclass *cannot* be accessed when proving *instances* of the class (they can only be accessed when probing the class object itself)?

```pycon
Python 3.11.4 (main, Sep 30 2023, 10:54:38) [GCC 11.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> class Meta(type):
...     attr = 42
...
>>> class Foo(metaclass=Meta): ...
...
>>> f = Foo()
>>> Foo.attr
42
>>> f.attr
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Foo' object has no attribute 'attr'
>>>
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/methods.md`:284 on 2025-03-07 13:50_

I would use the term "method on the class" rather than "class method" here and below, so as to avoid confusion with classmethods


---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:514 on 2025-03-07 13:55_

```suggestion
class TailoredForMetaclassAccess:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:519 on 2025-03-07 13:55_

```suggestion
    metaclass_access: TailoredForMetaclassAccess = TailoredForMetaclassAccess()
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:527 on 2025-03-07 13:56_

```suggestion
reveal_type(C.metaclass_access)  # revealed: bytes
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:605 on 2025-03-07 13:57_

```suggestion
descriptor protocol on the callable's `__call__` method:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:10 on 2025-03-07 14:07_

```suggestion
use ruff_python_ast as ast;
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1421 on 2025-03-07 14:09_

```suggestion
                    // If some elements are classes, and some are not, we simply fall back to `Unbound` for the non-class
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1431 on 2025-03-07 14:12_

We could consider implementing `Default` for `SymbolAndQualifiers` (which would return `Symbol::Unbound` with no qualifiers). Then this, and similar branches, could be:

```suggestion
                        .unwrap_or_default()
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1443 on 2025-03-07 14:19_

It might be more performant to have a single `Type::ClassLiteral` branch that does an inner `match` over the value of `class.known()` rather than having multiple branches that each have an `if class.is_known()` guard. I think the `Class::is_known()` calls can be surprisingly expensive because they do a Salsa lookups under the hood

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1939 on 2025-03-07 14:32_

Similarly here, it might be a little more performant to have a single `Type::instance()` branch with an inner match over `class.known()` rather than having multiple branches with `if class.is_known()` guards

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1991 on 2025-03-07 14:33_

```suggestion
                    "Calling `find_name_in_mro` on class literals and subclass-of types should always return `Some`",
```

---

_@AlexWaygood reviewed on 2025-03-07 15:07_

Not a proper review as I'm on my phone, but a couple of minor things I spotted skimming through the diff. Nothing blocking!

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3653 on 2025-03-07 18:31_

> It seems like it would be good to be able to do this operation on a `Symbol` without being required to provide qualifiers

Yes, sorry, that was what I tried to express. I will note this down as a follow up.

---

_@sharkdp reviewed on 2025-03-07 18:31_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/getattr_static.md`:75 on 2025-03-07 18:42_

No — added! Thanks

---

_@sharkdp reviewed on 2025-03-07 18:42_

---

_@sharkdp reviewed on 2025-03-07 18:47_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1991 on 2025-03-07 18:47_

No love for my word plays :cry: 

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1718 on 2025-03-07 18:49_

I will also leave this for a follow up

---

_@sharkdp reviewed on 2025-03-07 18:49_

---

_@carljm reviewed on 2025-03-07 18:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3653 on 2025-03-07 18:57_

No, you did express it effectively :) I was just restating the rationale in my comment.

---

_@sharkdp reviewed on 2025-03-07 19:03_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1863 on 2025-03-07 19:03_

> I'm curious about the switch from taking `&str` to taking a `Name`; is this motivated by performance?

Yes. I followed a suggestion by @MichaReiser when I originally made `member` a salsa query, because those can not take reference arguments. And it lead to a rather drastic performance improvement despite the string-clones. In the meantime, I have moved the `salsa::tracked` annotation to `member_lookup_with_policy`, so I now reverted this change (but kept it for some other functions which are still query). This makes the `member` call sites nicer again, but the cloning is still there … inside `member` where it calls into `member_lookup_with_policy`.

Note that `Name` contains a `CompactString` which can store up to 24 bytes on the stack, which should be ~~more than enough for any Python attribute name~~ relatively cheap to clone, even for `really_really_long_names`.

---

_@sharkdp reviewed on 2025-03-07 19:06_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1443 on 2025-03-07 19:06_

Thanks, I'll note this down for a follow-up.

---

_@sharkdp reviewed on 2025-03-07 19:06_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1939 on 2025-03-07 19:06_

Same here, I'll address this in a follow-up.

---

_@sharkdp reviewed on 2025-03-07 19:32_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1502 on 2025-03-07 19:32_

I believe this clause it both necessary and correct. But I should have written a comment, because it is definitely confusing. When I looked at it again, I also thought it was clearly wrong at first.

We eagerly normalize `type[object]`, i.e. `Type::SubclassOf("object")` to `type`, i.e. `Type::Instance("type")`. So looking up a name in the MRO of an *instance* of `type` is equivalent to looking up the name in the MRO of the *class* `object`.

When this clause is removed and we return `None`, we run into problems when e.g. looking up attributes on instances of `object`: `object().does_this_exist`.

I'll add a comment.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:992 on 2025-03-07 19:43_

Hm, interesting question. I saw the new `| Unknown` and initially thought it would make sense, but now that I think about it more carefully, I'm not so sure.

The `if flag` test in the method is more for "visualization purposes". We don't analyze control flow in methods for the purposes of attribute assignments.

We really union the possibly-undeclared `int` with a definitely bound `Unknown | int` here, meaning that the `Unknown` comes from the undeclared attribute assignment. Do you think it would make more sense to infer `int` here?

---

_@sharkdp reviewed on 2025-03-07 19:43_

---

_@carljm reviewed on 2025-03-07 19:52_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1502 on 2025-03-07 19:52_

Oh, that's subtle! But yes, makes sense.

---

_@sharkdp reviewed on 2025-03-07 19:59_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1679 on 2025-03-07 19:59_

> There seems to be an assumption made in this method that whatever qualifiers apply to the symbol `attribute` should also apply to its `__get__` method.

You're right. This is just wrong. For now, I replaced the `.with_qualifiers(…)` call on the return type of `__get__` with `.into()`, which I believe makes this semantically correct.

The suggested refactoring makes sense to me, I'll do this in a follow up.

---

_@carljm reviewed on 2025-03-07 19:59_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:992 on 2025-03-07 19:59_

Yes, your description of what is happening is exactly what I meant by "comes from it being possibly undeclared." That is, the declared type is possibly-not-declared, so we union it with the implicit attribute type which includes `Unknown`.

That seems like a reasonable outcome to me. I do think it's important that if the `x: int` here were unconditional, then we would just infer `int`. And that is indeed the case.

---

_@sharkdp reviewed on 2025-03-07 20:09_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:4753 on 2025-03-07 20:09_

Oh — If the unbound handling is wrong here, it's a pre-existing issue in `map_with_boundness` as well. I'd like to understand this in detail and write a few test cases, so I hope it's fine if I take this as a follow up as well, even though it's wrong.

---

_@carljm reviewed on 2025-03-07 20:21_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4753 on 2025-03-07 20:21_

Of course, no problem

---

_@sharkdp reviewed on 2025-03-07 20:28_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/class.rs`:587 on 2025-03-07 20:28_

So this is the case I talked about yesterday. It's definitely not straightforward to just return `declared.with_qualifiers(qualifiers)` here. If we do so, we treat every function on a class (which is declared and bound) into an instance attribute. Which does not just feel wrong, but also produces a lot of test failures. Instance attributes take precedence over non-data descriptors, so that would mean that we do not generate bound methods for something like `C().some_method`, but a normal `Literal` function type.

---

_@sharkdp reviewed on 2025-03-07 20:28_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1545 on 2025-03-07 20:28_

This is the same question as below I think, so let's continue the discussion there.

---

_Merged by @sharkdp on 2025-03-07 21:03_

---

_Closed by @sharkdp on 2025-03-07 21:03_

---

_Branch deleted on 2025-03-07 21:03_

---

_@sharkdp reviewed on 2025-03-07 21:06_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/class.rs`:587 on 2025-03-07 21:06_

I decided to merge this PR before signing off for the weekend, but curious to hear your opinion here. I think what I implemented here (w.r.t. this case) mostly keeps the behavior as it was before, but that doesn't necessarily mean that it's the best possible behavior. It's certainly somewhat inconsistent with how we treat declared-but-unbound attributes on the class body (for which we also see no attribute assignments). Another way to make it consistent would be to make *those* also return `Unbound` (maybe except for stubs), unless we see at least one attribute binding in a method(?).

---

_@MichaReiser reviewed on 2025-03-07 21:21_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3903 on 2025-03-07 21:21_

Sorry, I'm a bit late to the party. I think what we could do here instead is:

* add an internal `member_query(db: &'db dyn Db, name: InternedName)` function
* Add a new salsa interned `InternedName` struct that has a single field, a `Name` (we may need a newtype wrapper). Implement `salsa::Lookup` (if necessary) for `Name`

Salsa allows us to lookup the `InternedName` by reference and only clone when the name isn't already interned. Calling `member_query` is then a regular salsa query without any allocations


Oh, I see that the query takes two arguments, the type AND the member. I suspect that this approach could be slightly slower because Salsa still requires an intermediate salsa-interned struct that holds the name and the member, unless you rename `InternedName` to `MemberLookup` and add two fields: `type` and `name` 

---

_@carljm reviewed on 2025-03-08 00:26_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:587 on 2025-03-08 00:26_

The inconsistency that I was thinking about here is that if we see e.g. `foo: int = 1` in a class body (without a `ClassVar` annotation), we generally consider that to be an instance attribute with class fallback value. This means we will allow it to be set on an instance, which means it _could_ exist in the instance dictionary, which means it _could_ shadow a non-data descriptor. But I think you're right that we have to assume it does not, otherwise non-data descriptors would never work at all. It's not just methods, it's anytime you do `d: NonDataDescriptor = NonDataDescriptor()` on a class, we would think `C().d` might give you back `NonDataDescriptor`, because there might be one of those set in the instance dict. This is definitely not useful behavior.

So I think what you've done here is right! We will see if any issues come up on real codebases and can tweak accordingly.

---

_@sharkdp reviewed on 2025-03-12 09:36_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1679 on 2025-03-12 09:36_

> It also makes me wonder if this really should take `SymbolAndQualifiers`, or should just take `&self` (which is the type of attribute) and let the caller manage qualifiers.

Decided not to do this refactoring after all, because the qualifiers are now modified in case we see a descriptor (in which case the class var are replaced by an empty set of qualifiers). So handling this at the call site would lead to code duplication.

---

_@sharkdp reviewed on 2025-03-12 10:03_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3653 on 2025-03-12 10:03_

I attempted to solve this, but I think this would not just require us to duplicate all of these methods (`or_fall_back_to`, `unwrap_with_diagnostic`, …), but also `LookupError`/`LookupResult`. These new functions would have only very few call sites. And I'm not 100% sure I understand the motivation behind the `Symbol`/`LookupResult` split. This is also low priority, so I suggest that I discuss this with @AlexWaygood in one of our next 1:1s.

---

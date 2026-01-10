```yaml
number: 14128
title: "[red-knot] Add narrowing for `issubclass` checks"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/issubclass-narrowing
created_at: 2024-11-06T10:35:58Z
updated_at: 2024-11-07T13:56:02Z
url: https://github.com/astral-sh/ruff/pull/14128
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Add narrowing for `issubclass` checks

---

_Pull request opened by @sharkdp on 2024-11-06 10:35_

## Summary

- Adds basic support for `type[C]` as a red knot `Type`. Some things might not be supported yet, like `type[Any]`.
- Adds type narrowing for `issubclass` checks.

closes astral-sh/ruff#14117 

## Test Plan

New Markdown-based tests

---

_Label `red-knot` added by @sharkdp on 2024-11-06 10:35_

---

_Comment by @github-actions[bot] on 2024-11-06 10:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-11-06 11:28_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/narrow.rs`:451 on 2024-11-06 11:28_

Nit: How about `constraint.negate_if(self.db, is_positive)`

---

_Marked ready for review by @sharkdp on 2024-11-06 14:57_

---

_Review requested from @carljm by @sharkdp on 2024-11-06 14:57_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-06 14:57_

---

_@sharkdp reviewed on 2024-11-06 15:03_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1312 on 2024-11-06 15:03_

There's also a TODO comment below, aksing if `Type::StringLiteral(_) | Type::LiteralString` should result in `type[LiteralString]`. I'm not sure if that's a valid type, given that `LiteralString` is not a class?

---

_@sharkdp reviewed on 2024-11-06 15:04_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1324 on 2024-11-06 15:04_

This is too meta for me. Should this be `Type::Type('type')`?

---

_@sharkdp reviewed on 2024-11-06 15:11_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1091 on 2024-11-06 15:11_

I just copied what we have for `Type::Instance`. Not sure if that's correct.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:329 on 2024-11-06 15:12_

Can we create a `TypeType` struct, that wraps a `Class`, rather than reusing the `ClassLiteralType` struct for this variant? Conceptually `TypeType` and `ClassLiteralType` are quite distinct things (though related, of course).

I guess the reason not to do this would be having to unpack it as `Type::Type(TypeType { class})` 😆 yet another reason to call it something else ;)

---

_@sharkdp reviewed on 2024-11-06 15:15_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1291 on 2024-11-06 15:15_

I didn't include it in the block above, as the comment would not be correct for `Type::Type`. But I also don't know how one would turn `type[C]` into an instance of some kind.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:329 on 2024-11-06 15:17_

Other sensible, maybe less confusing names might be
* `Type::TypeOf` because `type[…]` resembles "type of …"
* `Type::MetaType`
* …

---

_@sharkdp reviewed on 2024-11-06 15:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1091 on 2024-11-06 15:18_

This is fine for now, but you could possibly add a TODO comment similar to the one in the `Type::ClassLiteral()` branch

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1324 on 2024-11-06 15:20_

> Should this be `Type::Type('type')`?

yes, I think so

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1312 on 2024-11-06 15:27_

> I'm not sure if that's a valid type, given that `LiteralString` is not a class?

...I don't know. There are many things which are not classes which are actually allowed inside `type[]` annotations. There are also significant gaps in the spec here. See https://discuss.python.org/t/clarifications-to-the-typing-spec-regarding-type/54596 -- which I need to get back to at some point!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2573 on 2024-11-06 15:28_

```suggestion
        // BuiltinClassLiteral("str") corresponds to the builtin `str` class object itself
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1291 on 2024-11-06 15:31_

You can call a runtime object with type `type[C]` at runtime to create a runtime object of type `C`: https://mypy-play.net/?mypy=latest&python=3.12&gist=8961e1a985e66a0022bd68baf514db9d

I think this should be the same as the `ClassLiteral` branch here, i.e.

```suggestion
            Type::Type(ClassLiteralType { class }) => Type::anonymous_instance(*class),
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2670 on 2024-11-06 15:33_

```suggestion
        // However, type[A] is not disjoint from type[B], as there could be
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2672 on 2024-11-06 15:34_

These are excellent comments, thank you!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:363 on 2024-11-06 15:46_

I think you could avoid the Clippy supppression and the `unreachable!()` here if you did something like this:

<details>
<summary>Suggested revision</summary>

```rs
        // TODO: add support for PEP 604 union types on the right hand side of `isinstance`
        // and `issubclass`, for example `isinstance(x, str | (int | float))`.
        let constraint_function = inference
            .expression_ty(expr_call.func.scoped_ast_id(self.db, scope))
            .into_function_literal()
            .and_then(|f| f.known(self.db))
            .and_then(KnownConstraintFunction::try_from_known_function)?;

        if expr_call.arguments.keywords.is_empty() {
            if let [ast::Expr::Name(ast::ExprName { id, .. }), class_info] =
                &*expr_call.arguments.args
            {
                let symbol = self.symbols().symbol_id_by_name(id).unwrap();

                let class_info_ty =
                    inference.expression_ty(class_info.scoped_ast_id(self.db, scope));

                let to_constraint = match constraint_function {
                    KnownConstraintFunction::IsInstance => {
                        |class_literal: ClassLiteralType<'db>| {
                            Type::anonymous_instance(class_literal.class)
                        }
                    }
                    KnownConstraintFunction::IsSubclass => Type::Type,
                };

                generate_classinfo_constraint(self.db, &class_info_ty, to_constraint).map(
                    |constraint| {
                        let mut constraints = NarrowingConstraints::default();
                        constraints.insert(symbol, constraint.negate_if(self.db, !is_positive));
                        constraints
                    },
                )
            } else {
                None
            }
        } else {
            None
        }
```

</details>

Where `KnownConstraintFunction` is an enum defined like this:

<details>

```rs
#[derive(Debug, Clone, Copy, PartialEq, Eq)]
enum KnownConstraintFunction {
    IsInstance,
    IsSubclass,
}

impl KnownConstraintFunction {
    const fn try_from_known_function(function: KnownFunction) -> Option<Self> {
        match function {
            KnownFunction::IsSubclass => Some(Self::IsSubclass),
            KnownFunction::IsInstance => Some(Self::IsInstance),
            KnownFunction::RevealType => None,
        }
    }
}
```

</details>

---

_@AlexWaygood reviewed on 2024-11-06 15:48_

Very cool!

---

_@AlexWaygood reviewed on 2024-11-06 16:00_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/issubclass.md`:95 on 2024-11-06 16:00_

Interestingly, this is an area where mypy and pyright differ. Pyright does the narrowing that you do here, but mypy actually refuses to do any type narrowing here as a point of principle. This is because (unlike for `isinstance()`) arbitrary objects are not acceptable as the first argument to `issubclass()`, only instances of `builtins.type`. Since `t` has type `object` here (which is not a subtype of `builtins.type`), mypy doesn't know whether or not the call will even succeed, so it doesn't:

```pycon
>>> issubclass(3, str)
Traceback (most recent call last):
  File "<python-input-0>", line 1, in <module>
    issubclass(3, str)
    ~~~~~~~~~~^^^^^^^^
TypeError: issubclass() arg 1 must be a class
```

To be clear, both type checkers issue a diagnostic on the `if issubclass(t, A)` call warning that it's not okay to pass an arbitrary object as the first argument to `issubclass()`. But pyright then proceeds to narrow the type anyway, even though it knows the call could fail, whereas mypy refuses to.

I think I like mypy's behaviour better here? But not sure.

- https://mypy-play.net/?mypy=latest&python=3.12&gist=1db65d922577e1c2fe52bd043070d9df
- https://pyright-play.net/?strict=true&code=MYGwhgzhAECCBc0B0KBQpIwEKJU1qAJgKYBm0p4A5gBQCU0AtAHzQBGA9hyLmieVWIAXAPoYo9Jqw5sAVsWBDe%2BVEOgBeaINHiI9AgEtyBqAFc2umkIA0cOvFTQn0AE7EAbsTAgRQgJ4ADsRWDNAAxK4eXiDEhIj%2BQQDasAC6js5G0CYQ5pY20Fj26c7Obp7evoHBQqERZdGx8VXJKdAAZNAJxIlYacQgEMQOJfUVXSFOdVHejdAy8ort0AB%2BXS1AA

Mypy's behaviour means that it _additionally_ complains about the second `issubclass()` call, whereas pyright does not.

---

_@carljm reviewed on 2024-11-06 19:31_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1312 on 2024-11-06 19:31_

I think `type[LiteralString]` is not valid. The invariant about `type[...]` is that it always represents a class object (or union of class objects) and all its/their subclasses. If you consider all the "other" things mentioned in your post that are valid subscripts for `type[]`, some of them (stringified forward reference, `Annotated[cls, ...]`, are just different syntactic forms that simplify out to a normal `type[cls]`. `type[Optional[cls]]` and `type[cls1 | cls2]` are just syntactic sugar for `type[None] | type[cls]` and `type[cls1] | type[cls2]`. `Self` works like a bounded typevar, so `type[Self]` and `type[T]` are the same; ultimately the typevar must be substituted by a class type.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1312 on 2024-11-06 19:33_

That's a great way of explaining it, thanks! In this case, we should clearly get rid of the TODO.

---

_@AlexWaygood reviewed on 2024-11-06 19:33_

---

_@carljm reviewed on 2024-11-06 19:46_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/issubclass.md`:95 on 2024-11-06 19:46_

I prefer pyright's behavior here. I think in cases where an expression may fail, but _if_ it does not fail, we can be confident of a type, it is better to emit the diagnostic and infer the "if it didn't fail" type, rather than inferring `Unknown`. This is the parallel case, but for narrowing. If the `isinstance` call fails, the code inside the `if` won't run anyway -- so why not infer the type we know must be the case if that code runs?

But perhaps more important here -- if we are going to write a test that should emit a diagnostic for invalid argument to `issubclass`, I want a TODO comment for that diagnostic. (Though here I would like it even better if we used `type[object]` instead of `object` to avoid that diagnostic, and wrote a separate test with TODO specifically for the invalid-argument case. But using `type[object]` would require enhancing `infer_type_expression` to understand the `type[T]` syntactic form.)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/issubclass.md`:95 on 2024-11-06 19:55_

> I think in cases where an expression may fail, but _if_ it does not fail, we can be confident of a type, it is better to emit the diagnostic and infer the "if it didn't fail" type, rather than inferring `Unknown`.

Okay, that makes sense for `builtins.object`, since obviously an instance of `object` _could_ be an instance of `type`, and therefore a valid first argument to `issubclass()` -- just don't _know_ that it is.

What about types that are entirely disjoint with `Instance(builtins.type)`, however -- where we _know_ that the runtime value cannot simultaneously inhabit that type and also be a valid first argument to `issubclass()`? Should we still narrow there? E.g. pyright has some "interesting" inference here...

```py
class A: ...
class B: ...

def flag() -> bool: ...
t = 3

if issubclass(t, A):  # Argument of type "Literal[3]" cannot be assigned to parameter "cls" of type "type" in function "issubclass"
    reveal_type(t)  # Type of "t" is "type[<subclass of Literal[3] and A>]"
    if issubclass(t, B):
        reveal_type(t)  # Type of "t" is "type[<subclass of <subclass of Literal[3] and A> and B>]"
else:
    reveal_type(t)  # Type of "t" is "type[Literal[3]]"
```

https://pyright-play.net/?strict=true&code=MYGwhgzhAECCBc0B0KBQpIwEKJU1qAJgKYBm0p4A5gBQCU0AtAHzQBGA9hyLmgC7QAvNADMBAJblxUAK5sMUGnwA0cOvFTQt0AE7EAbsTAgA%2BnwCeAB2JKG0AMS6DRkMUKIL1gNqwAupu1JaGkIOQUIJVUsdQDtbT1DYzMrGz47RwSXNw8Un19oADJoT2IvLH9iEAhiDTjMpJLbLQznY2zoDjYAK2JgASKAPxK8oA

"Narrowing" to types that we know cannot _ever_ exist makes little sense to me.

---

_@AlexWaygood reviewed on 2024-11-06 19:55_

---

_@carljm reviewed on 2024-11-06 19:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:329 on 2024-11-06 19:57_

If we don't feel that we need to stay tied to a name that's close to the syntactic form used in annotations, I think a good name for what this type actually means would be `Type::SubclassOf`. I think I would be happy with prioritizing semantic clarity, and relying on the doc comment to clarify that this represents `type[]`.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:329 on 2024-11-06 19:59_

`SubclassOf` is a decent name, I'm happy to go with that. I also like `MetaType`.

---

_@AlexWaygood reviewed on 2024-11-06 19:59_

---

_@AlexWaygood reviewed on 2024-11-06 20:00_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/issubclass.md`:95 on 2024-11-06 20:00_

Maybe we already do better than pyright on that point, though? Since intersections are a first-class concept for us when they're not for pyright?

Some tests for types that are entirely disjoint from `Instance(builtins.type)` might be great to clarify this!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:329 on 2024-11-06 20:01_

I agree that we probably shouldn't use `ClassLiteralType` here. We could have it wrap `Class` directly, but if we want to stay consistent with using wrapper structs, even when they have only one element (so that we can have type safety around the different meaning, even where we've unwrapped from the `Type` enum), then I agree we should create a new one, and it should be named `XType`, where `X` is the name we choose for the `Type::` variant itself. (So for example, `Type::SubclassOf(SubclassOfType)`.)

---

_@carljm reviewed on 2024-11-06 20:01_

---

_@carljm reviewed on 2024-11-06 20:04_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/issubclass.md`:95 on 2024-11-06 20:04_

Yes, I think in those cases we would just infer `Never`, which is perfectly sensible, because our intersection handling is better.

There's a case to be made that `Never` (rather than `Unknown`) is in general a better "forgiving" type to infer from failure cases. I wrote this up in more detail over at https://github.com/astral-sh/ruff/issues/13932 It's certainly not a bad inference to make when we are confident the code is not reachable (in this case because `issubclass` will always throw.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:539 on 2024-11-06 20:21_

I think we can add a TODO comment here (or rebase on top of https://github.com/astral-sh/ruff/pull/14120 and do it in this PR): we can improve this to ask instead whether the metaclass of the `Type::Type` class is a subclass of the `Type::Instance` class.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:707 on 2024-11-06 20:22_

Maybe a TODO here about final classes, which we will be able to establish disjointness of.

Although it may be that instead of adding special handling for final `Type::Type` in various places, we'll want instead to ensure at type construction that `Type::Type(SomeFinalClass)` is always turned into `Type::ClassLiteral(SomeFinalClass)`, since they are equivalent. (I guess this would require adding a `TypeTypeBuilder`, or generalizing the concept of a "type builder" to cover all types.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:709 on 2024-11-06 20:23_

I think we are missing a case to establish that `Type::Type(_)` is disjoint from all literal types that are not `ClassLiteral`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:896 on 2024-11-06 20:25_

And in this case `type[C]` is equivalent to `Literal[C]` -- not sure if that's worth a TODO comment in `is_equivalent_to`, or a mention in this comment?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1091 on 2024-11-06 20:31_

I had a momentary thought that that TODO isn't valid here, because a subclass could have a subclass of the metaclass. But our LSP checks (once we have them) should ensure that if `__bool__` or `__len__` are defined on the metaclass with a known-truthiness return type, a subclass can't override it with a wider return type. So I think the TODO does apply! And we are close to being able to resolve it, too, once Charlie's metaclass PR lands.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1291 on 2024-11-06 20:47_

This is a point of some controversy (at least in my head!), because LSP is not enforced for `__init__` or `__new__` methods, so having `type[C]` be callable is not sound -- you have no idea if the subclasses will share the same signature.

Personally I would rather have `type[C]` not be callable, and intersect it with a callable type with a given signature in order to make a callable version of it (which would not include subclasses with a different `__init__` signature.

But that would be a spec change, and there is certainly code in the wild relying on callable `type[]`, so for now probably the best thing to do is what Alex says. Later when we add call signature checking we'll add a comment here noting that our call signature checking here can't be reliable or sound.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1324 on 2024-11-06 20:50_

For now, sure. But with a TODO comment. Because with Charlie's metaclass PR, it should become something like:
```suggestion
            Type::Type(class) => Type::Type(class.class.metaclass(db).expect_literal_class())
```

but with more proper handling of odd cases and less expect

---

_@carljm reviewed on 2024-11-06 20:50_

(Haven't finished review yet but need to run to lunch and an errand, will complete review when I get back.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2740 on 2024-11-07 00:08_

Another test I think might be worth having (since it's easy to get confused over) is that `BuiltinClassLiteral("int")` is not a subtype of `BuiltinClassLiteral("object")`. Though admittedly it's orthogonal to this PR, I just thought of it since you are adding subtype tests around class literal types :)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:873 on 2024-11-07 00:12_

At some point now that we've reached three items, I think it would make sense to move this logic into `KnownFunction`, more like we do for `KnownClass` and `KnownInstance`.

---

_@carljm approved on 2024-11-07 00:16_

This is very nice!

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:329 on 2024-11-07 10:11_

And it's nice that `issubclass(cls, classinfo)` imposes a `Type::SubclassOf(classinfo)` constraint on `cls`.

---

_@sharkdp reviewed on 2024-11-07 10:11_

---

_@sharkdp reviewed on 2024-11-07 10:27_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1312 on 2024-11-07 10:27_

Thanks. Removed the TODO comment.

---

_@sharkdp reviewed on 2024-11-07 10:27_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:329 on 2024-11-07 10:27_

Done.

---

_@sharkdp reviewed on 2024-11-07 11:05_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1324 on 2024-11-07 11:05_

I tried something, but I'm not sure what kind of "odd cases" would need to be handled.  Let me know if something should be changed here (post-merge).

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:707 on 2024-11-07 11:06_

Added a detailed TODO comment.

---

_@sharkdp reviewed on 2024-11-07 11:06_

---

_@sharkdp reviewed on 2024-11-07 11:07_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:896 on 2024-11-07 11:07_

> a TODO comment in `is_equivalent_to`

:+1: 

---

_@sharkdp reviewed on 2024-11-07 11:08_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1091 on 2024-11-07 11:08_

Ok, I didn't resolve the TODO for now. I'll note it down as a potential follow-up.

---

_@AlexWaygood reviewed on 2024-11-07 11:13_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1091 on 2024-11-07 11:13_

Resolving the truthiness of instance types, class-literal types and `SubclassOf` types is _slightly_ more complicated than it seems, because we need to account for the fact that `bool(x)` (where `x` is a member of one of those kinds of types) could fail. We need to emit a diagnostic for things like this:

```pycon
>>> class Bad:
...     __bool__ = None
...     
>>> bool(Bad())
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    bool(Bad())
    ~~~~^^^^^^^
TypeError: 'NoneType' object is not callable
>>> class BadMetaclass(type):
...     __bool__ = None
...     
>>> class Foo(metaclass=BadMetaclass): pass
... 
>>> bool(Foo)
Traceback (most recent call last):
  File "<python-input-4>", line 1, in <module>
    bool(Foo)
    ~~~~^^^^^
TypeError: 'NoneType' object is not callable
```

That's why we haven't resolved the TODO for the `Type::Instance()` branch here yet either.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/narrow.rs`:363 on 2024-11-07 11:52_

I modelled it slightly different using:
```rs
pub enum KnownFunction {
    ConstraintFunction(KnownConstraintFunction),
    RevealType,
}
```

---

_@sharkdp reviewed on 2024-11-07 11:52_

---

_@AlexWaygood approved on 2024-11-07 12:05_

---

_@sharkdp reviewed on 2024-11-07 12:34_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/issubclass.md`:95 on 2024-11-07 12:34_

> But perhaps more important here -- if we are going to write a test that should emit a diagnostic for invalid argument to `issubclass`, I want a TODO comment for that diagnostic.

Done.

> Though here I would like it even better if we used `type[object]` instead of `object` to avoid that diagnostic

Ha! I had `type` here initially, but was annoyed by how it looked in the revealed types (`type & ~type[A]`), so I changed it to object without thinking about the fact that it is too wide of a type for the first argument of `issubclasss`. Changed to `type[object]` now.



> and wrote a separate test with TODO specifically for the invalid-argument case

Done, added two new tests.

> But using `type[object]` would require enhancing `infer_type_expression` to understand the `type[T]` syntactic form.

Implemented an initial version of this.

> What about types that are entirely disjoint with `Instance(builtins.type)`, however

Added a test for this specific case as well.

> I think in those cases we would just infer `Never`, which is perfectly sensible, because our intersection handling is better.

Correct:
```py
t = 1

# TODO: we should emit a diagnostic here
if issubclass(t, int):
    reveal_type(t)  # revealed: Never
```

---

_@AlexWaygood reviewed on 2024-11-07 12:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3889 on 2024-11-07 12:35_

```suggestion
                match value_ty
                    .into_class_literal()
                    .and_then(|class_ty| class_ty.class.known(self.db))
                {
                    Some(KnownClass::Tuple) => self.infer_tuple_type_expression(slice),
                    Some(KnownClass::Type) => self.infer_subclass_of_type_expression(slice),
                    _ => self.infer_subscript_type_expression(subscript, value_ty),
                }
```

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3889 on 2024-11-07 12:38_

I thought about it. But eventually, we're also going to have to understand annotations like `MyGenericClass[T]` here, so there will be cases where `value_ty` is not a known class, right?

---

_@sharkdp reviewed on 2024-11-07 12:38_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/issubclass.md`:157 on 2024-11-07 12:39_

A short description here might be helpful to a reader of the test:

```suggestion
#### Wrong

`Literal[1]` and `type` are entirely disjoint, so the inferred type of `Literal[1] & type[int]` is eagerly simplified to `Never` as a result of the type narrowing in the `if issubclass(t, int)` branch:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4073 on 2024-11-07 12:40_

```suggestion
            // TODO: attributes, unions, subscripts, etc.
            _ => {
                self.infer_type_expression(slice);
                Type::Todo
            }
```

---

_@AlexWaygood approved on 2024-11-07 12:40_

Great work!

---

_@AlexWaygood reviewed on 2024-11-07 12:46_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3889 on 2024-11-07 12:46_

That makes sense. What about a nested `match`, in that case? It feels slightly cleaner (and possibly more efficient?) than all these `if class.is_known()` checks in each branch:

```suggestion
                match value_ty {
                    Type::ClassLiteral(class_literal_ty) => {
                        match class_literal_ty.class.known(self.db) {
                            Some(KnownClass::Tuple) => self.infer_tuple_type_expression(slice),
                            Some(KnownClass::Type) => self.infer_subclass_of_type_expression(slice),
                            _ => self.infer_subscript_type_expression(subscript, value_ty),
                        }
                    }
                    _ => self.infer_subscript_type_expression(subscript, value_ty),
                }
```

---

_@sharkdp reviewed on 2024-11-07 12:46_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:4073 on 2024-11-07 12:46_

I always see a `Type::Todo` as something that we can't miss eventually. But we can add a TODO comment just to be sure.

---

_@AlexWaygood reviewed on 2024-11-07 12:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4073 on 2024-11-07 12:48_

I agree -- I just thought it would be useful to indicate to the reader what exactly is still to be done here 😄

I've wondered about making the `Todo` variant `Type::Todo(&'static str)`, to enforce that we _have_ to explicitly say what is still to be done when using the type... but it might just make things more complicated for little benefit :-)

---

_@sharkdp reviewed on 2024-11-07 12:50_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3889 on 2024-11-07 12:50_

Ok. I don't like that it duplicates the fallback branch, but it looks cleaner, I agree.

---

_Merged by @sharkdp on 2024-11-07 13:15_

---

_Closed by @sharkdp on 2024-11-07 13:15_

---

_Branch deleted on 2024-11-07 13:15_

---

_@carljm reviewed on 2024-11-07 13:53_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:553 on 2024-11-07 13:53_

Looks good! My comment was vague because I hadn't thought through yet the handling of a non-class-literal metaclass here. But I think the only options here are class literal or dynamic type (Any/Unknown/Todo), and since this is `is_subtype_of`, ignoring dynamic type is correct.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1375 on 2024-11-07 13:56_

This looks great, too! Falling back to `Type::SubclassOf(builtins.type)` in the case of any non-specific metaclass is the right answer here, I think.

---

_@carljm reviewed on 2024-11-07 13:56_

---

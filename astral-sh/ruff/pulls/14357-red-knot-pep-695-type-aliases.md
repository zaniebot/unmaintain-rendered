```yaml
number: 14357
title: "[red-knot] PEP 695 type aliases"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/type-aliases
created_at: 2024-11-15T10:55:18Z
updated_at: 2024-11-22T07:47:17Z
url: https://github.com/astral-sh/ruff/pull/14357
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] PEP 695 type aliases

---

_@sharkdp_

## Summary

Add support for (non-generic) type aliases. The main motivation behind this was to get rid of panics involving expressions in (generic) type aliases. But it turned out the best way to fix it was to implement (partial) support for type aliases.

```py
type IntOrStr = int | str

reveal_type(IntOrStr)  # revealed: typing.TypeAliasType
reveal_type(IntOrStr.__name__)  # revealed: Literal["IntOrStr"]

x: IntOrStr = 1

reveal_type(x)  # revealed: Literal[1]

def f() -> None:
    reveal_type(x)  # revealed: int | str
```

## Test Plan

- Updated corpus test allow list to reflect that we don't panic anymore.
- Added Markdown-based test for type aliases (`type_alias.md`)

---

_Label `red-knot` added by @sharkdp on 2024-11-15 10:55_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:609 on 2024-11-16 00:56_

In the CPython bytecode compiler, a type alias is more similar to a function or class than what you've implemented here. The RHS of the type alias is _always_ in its own nested scope (this is to support its deferred evaluation), and if it has type parameters, there are two nested scopes: one which just defines the type parameters, and then nested within that, another one in which the RHS is evaluated.

We can see/verify this using the `symtable` module:

<details>

```pyrepl
>>> s = symtable.symtable("type T = int", "test.py", "exec")
>>> s
<SymbolTable for module test.py>
>>> s.get_identifiers()
dict_keys(['T'])
>>> s.get_children()
[<SymbolTable for T in test.py>]
>>> s.get_children()[0]
<SymbolTable for T in test.py>
>>> s.get_children()[0].get_children()
[]
>>> s.get_children()[0].get_identifiers()
dict_keys(['int'])

>>> t = symtable.symtable("type T[U] = list[U]", "test.py", "exec")
>>> t.get_identifiers()
dict_keys(['T'])
>>> t.get_children()
[<SymbolTable for T in test.py>]
>>> t.get_children()[0]
<SymbolTable for T in test.py>
>>> t.get_children()[0].get_children()
[<SymbolTable for T in test.py>]
>>> t.get_children()[0].get_identifiers()
dict_keys(['U'])
>>> t.get_children()[0].get_children()
[<SymbolTable for T in test.py>]
>>> t.get_children()[0].get_children()[0]
<SymbolTable for T in test.py>
>>> t.get_children()[0].get_children()[0].get_identifiers()
dict_keys(['list', 'U'])
```

</details>

Since in the type alias case, there is no user code evaluated directly in the type parameters scope (unlike in the function case, where function annotations are evaluated in the type params scope, or the class case, where class bases are) it is _possible_ that we could flatten this such that there is always exactly one nested scope.

But I don't think we should; I think it will be easier to get right if we keep it in line with the bytecode compiler and in line with functions and classes. Which means we do need to be pushing and popping an inner scope in this closure, just like for functions and classes. And it means we need both `TypeAliasTypeParameters` and `TypeAlias` scope kinds, as well as `NodeWithScopeRef` etc.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:207 on 2024-11-16 02:30_

A `TypeAlias` scope should also be included in `ScopeId::is_function_like()` above (see https://github.com/python/cpython/blob/main/Python/symtable.c#L568 ).

Note though that a `TypeAliasTypeParameters` scope should just be `ScopeKind::Annotation` like the other type-param scopes; the inner scope for the RHS of the alias should be `ScopeKind::TypeAlias`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1143 on 2024-11-16 02:44_

We should never cross scopes in a single type inference builder. This is why you are currently hitting a wrong-scope debug assertion in this PR. Since both the type params and the RHS are in their own scope, we shouldn't be trying to infer them here.

If/when we want to actually infer a type for the type alias, this will involve a new `Type` variant and a new interned `TypeAliasType` struct to represent the alias. But the actual type value of the alias RHS should not be inferred eagerly here, it should be inferred lazily via a method on the `TypeAliasType` struct, which finds the inner scope (probably this should be preserved as an attribute on `TypeAliasType`, much like `body_scope` is for classes and functions) and then performs type inference on that scope and pull out the inferred type for the top-level expression in that scope.

We don't ever need to explicitly infer the type-params scope, if there is one. This will happen naturally when we infer the RHS scope, if there are any references there to type vars -- `lookup_name` will walk up the scopes, find the definition of that name in the enclosing type-params scope, and infer the typevar definition in the type-params scope; this will all happen without us needing to do anything about it explicitly here.

So mostly I think you can fix this PR by simply doing less here :)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1160 on 2024-11-16 02:46_

Yeah it should be `_with_binding` (as you currently have it), because the type statement defines a value for the type alias name at runtime.

The more questionable part is whether it should be a declaration, or just a binding. But I think it should be a declaration; we should complain if you reassign a type alias name to a different value.

---

_@carljm reviewed on 2024-11-16 02:47_

Happy to do a VC call next week if anything remains unclear about how this is all supposed to work!

---

_Comment by @github-actions[bot] on 2024-11-20 15:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @sharkdp on 2024-11-20 21:24_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-20 21:24_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-20 21:24_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:598 on 2024-11-21 00:55_

I tried a bunch of different things in the ruff playground, and anytime I made it not a name, the parser no longer spit out a `TypeAlias` node at all. So I think given the current parser implementation, this is safe.

I'm not sure I would call that a _guarantee_, though. We want to be robust (not panic) against invalid syntax, and I think it's preferable if we can avoid depending on details of how the parser chooses to represent invalid syntax. Here, it seems like it would be pretty simple to instead just fallback on some synthetic name like `"<unknown>"`? (If I'm wrong, and it's actually hard/problematic to do that, then I think we can just go with the `.expect()` as you have it.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:123 on 2024-11-21 01:11_

On closer look here I realize that `NodeWithScopeKind::TypeAliasTypeParameters` also belongs here, just like `ClassTypeParameters` and `FunctionTypeParameters`.

Though it seems like we could also simplify this to match on `self.node(db).kind()` instead, and then including `ScopeKind::Annotation` would cover all three of the type-param scopes.

Either option seems fine to me.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:407 on 2024-11-21 01:26_

This should be `NodeWithScopeKey::TypeAlias`, not `NodeWithScopeKey::TypeAliasTypeParameters`. (Will also require adding the `NodeWithScopeKey::TypeAlias` variant below.) Otherwise, both the type alias' type params scope and its inner scope will try to use the same key in the scopes-by-node mapping.

It apparently works anyway because we never actually try to look up the type parameters scope, and we insert the inner scope into the map second, so it replaces the type-params scope, and we always end up getting the inner scope back from a lookup.

We should add a debug assertion in `SemanticIndexBuilder::push_scope_with_parent` that our `self.scopes_by_node.insert(...)` call always returns `None` (that is, it never stomps on an existing scope recorded for the same node key.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:358 on 2024-11-21 01:27_

Should clarify what kind, since there are also legacy type aliases.
```suggestion
    /// A PEP 695 type alias, with a name and corresponding type
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:359 on 2024-11-21 01:29_

I'm realizing now (contrary to my comment in the last review) that we don't actually need or want an entirely new `Type` variant for this. The type of `x` after `type x = int` is a singleton type representing a single known instance of `typing.TypeAliasType`. So I think we can (and should) just use `Type::KnownInstance` for this, and a new variant of `KnownInstanceType` that holds the `TypeAliasType` (much like I did for type variables). And that will give us a bunch of singleton-known-instance behaviors for free. (This will probably also require a `KnownClass` for `typing.TypeAliasType`.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:729 on 2024-11-21 01:38_

This is confusing the type represented by the type alias when we see it used in an annotation expression (that is, the type spelled by the type expression on the RHS of the alias), with the type of the alias object itself (which is just a singleton known instance of `typing.TypeAliasType`). So the "de-reference" to the alias' value that you are implementing here should happen only when we encounter a type alias in `infer_type_expression` (that is, in `in_type_expression` below), we don't want to implement it throughout the type system.

Since a type alias object is a singleton instance, it's always disjoint -- but if we use `KnownInstance`, we'll get that for free.

(I won't comment on all the other places below where you're matching on `TypeAlias` and implementing this de-reference to its value type, but the same comment applies to all of them.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2819 on 2024-11-21 01:40_

This is exactly right!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:870 on 2024-11-21 01:44_

It should be `Deferred`, because the RHS of a type statement is lazily evaluated at runtime, meaning we should use end-of-scope name resolution rather than flow-sensitive name resolution for names in the expression.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/mro.rs`:355 on 2024-11-21 01:47_

The spec doesn't really clarify, but mypy and pyright both agree that a type alias isn't valid as a base class, and it won't work at runtime, either. So we don't need to support it in this way, it can go below in the long list of things that return `None`. (Though it will probably go inside the `KnownInstance` match instead.)

---

_Review comment by @carljm on `crates/red_knot_workspace/tests/check.rs`:272 on 2024-11-21 01:51_

This is interesting. I suspect it might go away if we stop trying to infer the type of `.value()` on a type alias anytime we encounter it. If not I'll want to take a closer look.

---

_@carljm reviewed on 2024-11-21 01:56_

The updated type inference structure looks great!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_alias.md`:8 on 2024-11-21 01:57_

I think we'll get this for free too with using `KnownInstance`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1491 on 2024-11-21 02:06_

Note: this is the one method where we will want to map the type alias to its value.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:729 on 2024-11-21 02:09_

It might be good to add a few tests that would have failed based on this -- e.g. showing that we understand that two different type alias singleton types are disjoint and distinct types, even if they are an alias to the same type.

---

_@carljm reviewed on 2024-11-21 02:09_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:2737 on 2024-11-21 06:01_

nit: `value_ty` ?

---

_@dhruvmanila reviewed on 2024-11-21 06:01_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:2737 on 2024-11-21 07:10_

This should be a `salsa::tracked` because it uses `scope.node` or we should document that this function should never be called from a query that's from another file than the current query.

```suggestion
#[salsa::tracked]
impl TypeAliasType<'_> {
    #[salsa::tracked]
    pub fn value(self, db: &dyn Db) -> Type {
```

---

_@MichaReiser approved on 2024-11-21 07:10_

---

_@sharkdp reviewed on 2024-11-21 09:04_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:598 on 2024-11-21 09:04_

Sorry, that TODO comment was really meant for myself (to either check if it's okay or to change the code)

> I tried a bunch of different things in the ruff playground, and anytime I made it not a name, the parser no longer spit out a `TypeAlias` node at all. So I think given the current parser implementation, this is safe.

That's what I did as well :smile:. 

> I think it's preferable if we can avoid depending on details of how the parser chooses to represent invalid syntax

Agreed.

> Here, it seems like it would be pretty simple to instead just fallback on some synthetic name like `"<unknown>"`? (If I'm wrong, and it's actually hard/problematic to do that, then I think we can just go with the `.expect()` as you have it.)

I'll look into it.

---

_@sharkdp reviewed on 2024-11-21 09:25_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:123 on 2024-11-21 09:25_

> Though it seems like we could also simplify this to match on `self.node(db).kind()` instead, and then including `ScopeKind::Annotation` would cover all three of the type-param scopes.

If I understand correctly, you would want to change it to:
```rs
matches!(
    self.node(db).scope_kind(),
    ScopeKind::Annotation | ScopeKind::Function | ScopeKind::TypeAlias | ScopeKind::Comprehension
)
```
This would match everything that was matched previously, but it would additionally iunclude `NodeWithScopeKind::Lambda(_)`. I wasn't sure if that's correct, so I just added `NodeWithScopeKind::TypeAliasTypeParameters` for now.

---

_@sharkdp reviewed on 2024-11-21 09:34_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:407 on 2024-11-21 09:34_

Oops. Added the assertion so this won't happen again (made sure it triggers on my previous version).

---

_@sharkdp reviewed on 2024-11-21 12:03_

---

_Review comment by @sharkdp on `crates/red_knot_workspace/tests/check.rs`:272 on 2024-11-21 12:03_

It happens for self-referential type aliases like `type NestedInt = int | list[NestedInt]`. We form a genuine cycle when inferring the annotation type on the RHS and look up the name `NestedInt`.

---

_Renamed from "[red-knot] Preliminary support for type aliases" to "[red-knot] PEP 695 type aliases" by @sharkdp on 2024-11-21 12:46_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_alias.md`:10 on 2024-11-21 22:15_

Heh, I thought yesterday about recommending a test of the `__value__` attribute, and then I didn't because it's actually pretty tricky. But I should have gone ahead and mentioned why it's tricky, to save you some time.

Internally the `value` type we care about for a type alias is the type of the RHS expression _as a type expression_. And that type here is the type `int | str`. But the type of the actual `__value__` attribute at runtime is the type of the RHS _as a runtime value expression_. Which in this case would be an instance of `types.UnionType`, the result at runtime of evaluating the expression `int | str`. That's a very different type from the type `int | str`.

If we want to do this precisely, it's _possible_, but I think it would require inferring the type of the RHS twice, once as a value expression and once as a type expression. And that gets into some difficulties with our expectation that a given expression has only one inferred type.

So the question is, how valuable is it to have precise typing of the `__value__` attribute of a type alias? Tbh I'm not sure; it's possible that with the increasing use of runtime-typing libraries, like Pydantic, we may get user requests for this precision. But existing type checkers don't bother; they just go with the typeshed definition of `TypeAliasType`, which says the type of `__value__` is `Any`: https://github.com/python/typeshed/blob/main/stdlib/typing.pyi#L1045

For TypeVars, I did try to do precise typing of `__bounds__` and `__constraints__`, mostly so I could write tests like this one, because unlike with a type alias, I didn't even have any way to write a test like the one you wrote down on line 17 here, which correctly asserts for the type `int | str`. But the way I did it (using `to_meta_type` to try to convert "type as type expression" to "type as value expression") is hacky and wrong, as I've realized more clearly since. It works only in simple cases (if `A` is a class, the meta type of `A` is `Literal[A]`, and the latter is also the runtime type of the expression `A`), but it doesn't even work for a union case like this. The meta type of `A | B` is `Literal[A] | Literal[B]`, but that's not the type of the runtime value expression `A | B`; the latter is an instance of `types.UnionType`.

All that is to say: I think for this PR we should not implement any special handling for the `__value__` attribute of a type alias type, and just let it fallback to typeshed. (And, separately, we should probably remove the special handling I added for `__bounds__` and `__constraints__` on a TypeVar. I'll push a PR for this.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_alias.md`:35 on 2024-11-21 22:36_

We should use the more verbose "annotate a name as `IntOrStrOrBytes` and check its revealed type from a different scope" approach here, for the reasons discussed above; the actual type of `IntOrStrOrBytes.__value__` here would be `types.UnionType`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_alias.md`:45 on 2024-11-21 22:37_

And similarly this assertion is not right.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:123 on 2024-11-21 22:57_

Yes, a lambda scope is a function scope, so that would actually be a bugfix.

(For reference, https://github.com/python/cpython/blob/main/Python/symtable.c#L563 is the upstream equivalent of our `is_function_like`, and it operates on a CPython enum which is precisely parallel to our `ScopeKind` enum. And we can see at https://github.com/python/cpython/blob/main/Python/symtable.c#L2379 that Lambda nodes do get a `FunctionBlock` scope.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:453 on 2024-11-21 22:59_

very minor code organization nit: maybe let's not split the two Function scopes (function and lambda) apart here? We could even group them into a single `|` pattern, like with the annotation and comprehension scopes below

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1972 on 2024-11-21 23:03_

As discussed above, I think we should remove this.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1975 on 2024-11-21 23:04_

I'm also not sure we need a match arm and a Todo here, when it would be correct (and match the behavior of other type checkers) to just allow it to fall back to the typeshed definition of the attribute. We can potentially add more precise per-instance typing for it in the future if there are compelling use cases, but we don't need to consider it a todo.

---

_@carljm approved on 2024-11-21 23:14_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_alias.md`:10 on 2024-11-22 07:20_

Thanks!

---

_@sharkdp reviewed on 2024-11-22 07:20_

---

_Merged by @sharkdp on 2024-11-22 07:47_

---

_Closed by @sharkdp on 2024-11-22 07:47_

---

_Branch deleted on 2024-11-22 07:47_

---

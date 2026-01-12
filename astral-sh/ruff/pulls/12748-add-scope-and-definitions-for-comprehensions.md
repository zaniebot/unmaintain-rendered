```yaml
number: 12748
title: Add scope and definitions for comprehensions
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/comprehension-red-knot
created_at: 2024-08-08T10:40:13Z
updated_at: 2024-08-13T01:35:57Z
url: https://github.com/astral-sh/ruff/pull/12748
synced_at: 2026-01-12T15:55:42Z
```

# Add scope and definitions for comprehensions

---

_@dhruvmanila_

## Summary

This PR adds scope and definition for comprehension nodes. This includes the following nodes:
* List comprehension
* Dictionary comprehension
* Set comprehension 
* Generator expression

### Scope

Each expression here adds it's own scope with one caveat - the `iter` expression of the first generator is part of the parent scope. For example, in the following code snippet the `iter1` variable is evaluated in the outer scope.

```py
[x for x in iter1]
```

> The iterable expression in the leftmost for clause is evaluated directly in the enclosing scope and then passed as an argument to the implicitly nested scope. 
>
> Reference: https://docs.python.org/3/reference/expressions.html#displays-for-lists-sets-and-dictionaries

There's another special case for assignment expressions:

> There is one special case: an assignment expression occurring in a list, set or dict comprehension or in a generator expression (below collectively referred to as “comprehensions”) binds the target in the containing scope, honoring a nonlocal or global declaration for the target in that scope, if one exists.
>
> Reference: https://peps.python.org/pep-0572/#scope-of-the-target

For example, in the following code snippet, the variables `a` and `b` are available after the comprehension while `x` isn't:
```py
[a := 1 for x in range(2) if (b := 2)]
```

### Definition

Each comprehension node adds a single definition, the "target" variable (`[_ for target in iter]`). This has been accounted for and a new variant has been added to `DefinitionKind`.

### Type Inference

Currently, type inference is limited to a single scope. It doesn't _enter_ in another scope to infer the types of the remaining expressions of a node. To accommodate this, the type inference for a **scope** requires new methods which _doesn't_ infer the type of the `iter` expression of the leftmost outer generator (that's defined in the enclosing scope).

The type inference for the scope region is split into two parts:
* `infer_generator_expression` (similarly for comprehensions) infers the type of the `iter` expression of the leftmost outer generator
* `infer_generator_expression_scope` (similarly for comprehension) infers the type of the remaining expressions except for the one mentioned in the previous point

The type inference for the **definition** also needs to account for this special case of leftmost generator. This is done by defining a `first` boolean parameter which indicates whether this comprehension definition occurs first in the enclosing expression.

## Test Plan

New test cases were added to validate multiple scenarios. Refer to the documentation for each test case which explains what is being tested.


---

_Label `red-knot` added by @dhruvmanila on 2024-08-08 10:40_

---

_Comment by @github-actions[bot] on 2024-08-09 10:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @dhruvmanila on 2024-08-09 10:48_

---

_Review requested from @carljm by @dhruvmanila on 2024-08-09 10:48_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-09 10:48_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-09 10:48_

---

_Comment by @dhruvmanila on 2024-08-09 15:03_

(I'm planning to look at the diagnostics difference in the benchmark today)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:533 on 2024-08-09 15:57_

```suggestion
    /// Test case to validate that the generator scope is correctly identified and that the target
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:661 on 2024-08-09 16:08_

I think the cleaner way to handle assignment expressions inside a comprehension will be to still record the name and the definition inside the comprehension scope, but mark the name as nonlocal (as if there were implicitly a `nonlocal a` statement in that scope.) I think this is also necessary for correctness in cases where the assigned name is also used within the comprehension.

This will really just become a `TODO` as currently we don't yet have handling in general for the `nonlocal` statement or nonlocal name references.

So for now I would probably just remove this test.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:28 on 2024-08-09 16:10_

I would merge this with the existing `use crate::semantic_index::definition::{...}` statement above.

Perhaps we should generally use `super::` for imports from `crate::semantic_index` instead (I wouldn't mind that change) but within a given file I'd rather stay consistent one way or another and keep the imports together.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:189 on 2024-08-09 16:11_

As mentioned in the other comment, I think we should remove this logic.

We could add a TODO for handling assignment expressions inside comprehensions as nonlocal names. Maybe somewhere down with the comprehension-visit methods?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:177 on 2024-08-09 16:12_

Hmm, why is this annotation now required?

---

_Comment by @dhruvmanila on 2024-08-09 17:19_

I added the filename/line/column information to diagnostics (still a string), and all checks out.

The following two diagnostics are removed (valid):

<pre>
<a href="https://github.com/python/cpython/blob/591f6754b56cb7f6c31fce8c22528bdf0a99556c/Lib/tomllib/_parser.py#L22">tomllib/_parser.py:22:28: Name 'i' used when not defined.</a>
<a href="https://github.com/python/cpython/blob/591f6754b56cb7f6c31fce8c22528bdf0a99556c/Lib/tomllib/_parser.py#L330">tomllib/_parser.py:330:46: Name 'i' used when not defined.</a>
</pre>

And, the following got added:

<pre>
<a href="https://github.com/python/cpython/blob/591f6754b56cb7f6c31fce8c22528bdf0a99556c/Lib/tomllib/_parser.py#L330">tomllib/_parser.py:330:41: Name 'key' used when not defined.</a>
</pre>

The one that got added is because type inference is done at the scope level, so the `key` variable is not defined in the generator scope but in the enclosing scope.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:187 on 2024-08-09 17:40_

As elsewhere, `Comprehension` not `Generator`.

I'm torn about whether we should have a separate scope kind here or just use `ScopeKind::Function`. But I think this will be useful when we do need to implement the special case about assignment expressions in comprehensions; otherwise we'd just need to keep separate state on the semantic index builder that the current scope is a comprehension.

But we should add this scope kind to `ScopeId::is_function_like` above, since otherwise comprehension scopes just behave like function scopes.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:691 on 2024-08-09 17:45_

Throughout this PR, we should use "Comprehension" instead of "Generator" as the general term covering list/dict/set/generator comprehensions.

A generator in Python is a specific kind of iterator object, which also has `send()` and `throw()` methods, which is created either using a generator expression / generator comprehension, or by defining a function that uses the `yield` keyword.

List/dict/set comprehensions are not generators. Generator expressions can also be referred to as "generator comprehensions"; a generator expression / generator comprehension is the expression syntax for defining a generator (as opposed to the function-that-yields syntax.)

The use of the term "generator" in the Python AST (and our AST) for "the things inside a comprehension that iterate and generate values" is perhaps a bit unfortunate, because it's overloading the term to describe something different than what "generator" usually means in Python.

The upshot of all this:
- it's fine to keep `visit_generator` etc in the builder as-is; this is just using the term as it's already used in the AST, to describe this particular piece of syntax inside a comprehension.
- everywhere else (definitions, symbol table, etc) we should use `Comprehension`, not `Generator`, as the general term encompassing list/dict/set/generator comprehensions.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:47 on 2024-08-09 17:55_

`Comprehension`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:111 on 2024-08-09 17:55_

`ComprehensionDefinitionNodeRef`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:184 on 2024-08-09 17:55_

`Comprehension`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:133 on 2024-08-09 17:56_

Technically it would be better to use `GeneratorExpression` or `GeneratorComprehension` here, not just `Generator`, since this doesn't include "function that yields" syntax.

It's kind of a toss-up which one to use; they are more commonly called "generator expressions", but "generator comprehension" highlights the parallel to other kinds of comprehensions better. I'm fine with whichever you prefer; if you don't care I guess I'd go with `GeneratorComprehension`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:317 on 2024-08-09 17:58_

`GeneratorExpression` or `GeneratorComprehension`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:418 on 2024-08-09 17:59_

`GeneratorExpression` or `GeneratorComprehension`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:432 on 2024-08-09 17:59_

`GeneratorExpression` or `GeneratorComprehension`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:264 on 2024-08-09 18:02_

I prefer these method names without the word "in", e.g. `infer_list_comprehension_expression_scope`. Using "in" almost seems to communicate the opposite of what we want. What distinguishes these methods is that they infer the comprehension scope, not the comprehension in its containing scope.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1080 on 2024-08-09 18:03_

```suggestion
            unreachable!("Comprehension must contain at least one generator");
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1189 on 2024-08-09 18:04_

```suggestion
            unreachable!("Comprehension must contain at least one generator");
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1095 on 2024-08-09 18:07_

We could update this to return `typing.Generator`, and similarly below for `builtins.dict`, `builtins.list`, `builtins.set`. It should be a fairly simple change to resolve the type from typeshed.

But I don't feel strongly about whether we do that in this PR or not, since we'd still have a TODO to make it generic and properly infer the contained type, once we have generic types.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1197 on 2024-08-09 18:13_

This method should look up the definition and use `infer_definition_types` query here (thus ending up in `infer_comprehension_definition` to actually do the work), otherwise we'll end up repeating inference of the same comprehension, once in the scope-level query and once in the definition-level query. You can use the existing helper `infer_definition` to do this; see e.g. `infer_function_definition_statement` for an existing case.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1218 on 2024-08-09 18:16_

Might be good to add a TODO here just to remind us that actually the `target` type should (later, not in this PR) be inferred by getting the item type of the (iterable) type of `iter`, not by an `infer_expression` call.

---

_@carljm requested changes on 2024-08-09 18:17_

This is great, thanks for figuring out so much just by reading the code, which isn't always as documented as it should be!

A few changes I think we should make (mostly to naming, a couple functional things), but the basic structure is exactly right.

---

_@AlexWaygood reviewed on 2024-08-09 18:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:691 on 2024-08-09 18:20_

> The use of the term "generator" in the Python AST (and our AST)

We differ from CPython's AST in quite a few ways at this point; we could easily rename this field to something less confusing (but obviously not in this PR)

---

_@AlexWaygood reviewed on 2024-08-09 18:22_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1095 on 2024-08-09 18:22_

> We could update this to return `typing.Generator`, and similarly below for `builtins.dict`, `builtins.list`, `builtins.set`. It should be a fairly simple change to resolve the type from typeshed.

Generators are actually somewhat interesting because do you infer the abstract type (`typing.Generator`) or the concrete type (`types.GeneratorType`)? I think mypy does the former and it causes issues... can't remember what pyright does...

---

_Comment by @carljm on 2024-08-09 18:28_

> The one that got added is because type inference is done at the scope level, so the `key` variable is not defined in the generator scope but in the enclosing scope.

Yes, this is just part of our larger TODO item to support nonlocal name references; currently we support local and global, but not nonlocal yet.

---

_Comment by @carljm on 2024-08-09 18:30_

> I added the filename/line/column information to diagnostics (still a string), and all checks out.

Want to push this as a PR? I think it's generally useful. The specific current diagnostics won't survive Micha's planned work for next week, but the code to include this information in diagnostics could survive the transition.

---

_@carljm reviewed on 2024-08-09 18:33_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1095 on 2024-08-09 18:33_

Oh, yeah... I think probably the concrete type (`types.GeneratorType`) is better, since in this case we know for sure it is the built-in Generator type, not something else that just implements the interface.

---

_@dhruvmanila reviewed on 2024-08-12 06:28_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index.rs`:661 on 2024-08-12 06:28_

Yeah, I think that makes sense specifically for "necessary for correctness in cases where the assigned name is also used within the comprehension."

---

_@dhruvmanila reviewed on 2024-08-12 06:31_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:28 on 2024-08-12 06:31_

Yeah, I generally prefer to use `crate::` for imports from the current crate and using `super::` only when it's from the parent directory and not exposed to the crate. That said, I didn't really add this import, it was `rust-analyzer` that did it when I accepted the completion ;)

---

_@dhruvmanila reviewed on 2024-08-12 06:32_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:177 on 2024-08-12 06:32_

It's not really required to compile the code but rust-analyzer wasn't able to infer the type of `definition_node` variable in this context (not sure why).

---

_@dhruvmanila reviewed on 2024-08-12 06:37_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:691 on 2024-08-12 06:37_

Thanks for the explanation between the two. This really helps in clarifying where to use one and the other. I think what you say makes sense and I'll update the wording to use "comprehension" in most places.

---

_@dhruvmanila reviewed on 2024-08-12 06:41_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:133 on 2024-08-12 06:41_

I don't mind using `GeneratorComprehension` although I couldn't find any reference to "generator comprehension" in the Python documentation. I don't really mind the inconsistency as it clearly highlights the syntax that this variant represents. I'm leaning towards `GeneratorExpression` for now.

---

_@dhruvmanila reviewed on 2024-08-12 06:51_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:187 on 2024-08-12 06:51_

> But we should add this scope kind to `ScopeId::is_function_like` above, since otherwise comprehension scopes just behave like function scopes.

Isn't that changed with [PEP 709 - Inlined comprehensions](https://peps.python.org/pep-0709/) in Python 3.12?

---

_@AlexWaygood reviewed on 2024-08-12 07:24_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:187 on 2024-08-12 07:24_

> Isn't that changed with [PEP 709 - Inlined comprehensions](https://peps.python.org/pep-0709/) in Python 3.12?

The internal implementation in CPython no longer uses a function object for these scopes, following the changes Carl made to implement PEP 709. But the user-facing behaviour is the same in 99% of situations; semantically, they still behave like function scopes in almost every way

---

_@dhruvmanila reviewed on 2024-08-12 07:32_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:187 on 2024-08-12 07:32_

I see, that makes sense. Thanks for clarifying.

---

_Review requested from @carljm by @dhruvmanila on 2024-08-12 11:45_

---

_Comment by @codspeed-hq[bot] on 2024-08-12 16:10_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/comprehension-red-knot)

### Merging #12748 will **not alter performance**

<sub>Comparing <code>dhruv/comprehension-red-knot</code> (e904b8d) with <code>main</code> (fb9f0c4)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @dhruvmanila on 2024-08-12 16:14_

(Updated the number of expected diagnostics for the Red Knot benchmark per the analysis in https://github.com/astral-sh/ruff/pull/12748#issuecomment-2278398925)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:536 on 2024-08-12 16:56_

Nit: for clarity, we should use "comprehension" rather than "generator" in the name of this test, its doc comment, and in variable names throughout the test.

Same in some other tests below.
```suggestion
    /// Test case to validate that the comprehension scope is correctly identified and that the target
    /// variable is defined only in the comprehension scope and not in the outer scope.
    #[test]
    fn comprehension_scope() {
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:563 on 2024-08-12 16:57_

```suggestion
        let [(comprehension_scope_id, comprehension_scope)] = index
            .child_scopes(FileScopeId::global())
            .collect::<Vec<_>>()[..]
        else {
            panic!("expected one child scope")
        };

        assert_eq!(comprehension_scope.kind(), ScopeKind::Comprehension);
        assert_eq!(
            comprehension_scope_id.to_scope_id(&db, file).name(&db),
            "<listcomp>"
        );

        let comprehension_symbol_table = index.symbol_table(comprehension_scope_id);

        assert_eq!(names(&comprehension_symbol_table), vec!["x"]);
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:567 on 2024-08-12 16:57_

```suggestion
    /// Test case to validate that the `x` variable used in the comprehension is referencing the `x`
    /// variable defined by the inner generator (`for x in iter2`) and not the outer one.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:607 on 2024-08-12 16:59_

```suggestion
        let [(comprehension_scope_id, _)] = index
            .child_scopes(FileScopeId::global())
            .collect::<Vec<_>>()[..]
        else {
            panic!("expected one child scope")
        };

        let use_def = index.use_def_map(comprehension_scope_id);

        let module = parsed_module(&db, file).syntax();
        let element = module.body[0]
            .as_expr_stmt()
            .unwrap()
            .value
            .as_list_comp_expr()
            .unwrap()
            .elt
            .as_name_expr()
            .unwrap();
        let element_use_id = element.scoped_use_id(&db, comprehension_scope_id.to_scope_id(&db, file));

        let [definition] = use_def.use_definitions(element_use_id) else {
            panic!("expected one definition")
        };
        let DefinitionKind::Comprehension(comprehension) = definition.node(&db) else {
            panic!("expected comprehension definition")
        };
        let ast::Comprehension { target, .. } = comprehension.node();
        let name = target.as_name_expr().unwrap().id().as_str();

        assert_eq!(name, "x");
        assert_eq!(target.range(), TextRange::new(23.into(), 24.into()));
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:567 on 2024-08-12 17:00_

This test name is correct, given the current terminology used in the AST; it is testing multiple "generators" in a comprehension. But there are some uses of "generator" in variable names within the test that are not accurate; using "generator" where we mean "comprehension."

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:614 on 2024-08-12 18:56_

```suggestion
    /// Test case to validate that the nested comprehension creates a new scope which is a child of the
    /// outer comprehension scope and the variables are correctly defined in the respective scopes.
    #[test]
    fn nested_generators() {
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:567 on 2024-08-12 18:57_

Oh and this is a very minor nit, but I think in these test case doc comments you could eliminate the entire phrase "Test case to validate that" without losing anything.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:657 on 2024-08-12 19:09_

```suggestion
        let [(comprehension_scope_id, comprehension_scope)] = index
            .child_scopes(FileScopeId::global())
            .collect::<Vec<_>>()[..]
        else {
            panic!("expected one child scope")
        };

        assert_eq!(comprehension_scope.kind(), ScopeKind::Comprehension);
        assert_eq!(
            comprehension_scope_id.to_scope_id(&db, file).name(&db),
            "<listcomp>"
        );

        let comprehension_symbol_table = index.symbol_table(comprehension_scope_id);

        assert_eq!(names(&comprehension_symbol_table), vec!["y", "iter2"]);

        let [(inner_comprehension_scope_id, inner_comprehension_scope)] =
            index.child_scopes(comprehension_scope_id).collect::<Vec<_>>()[..]
        else {
            panic!("expected one inner comprehension scope")
        };

        assert_eq!(inner_comprehension_scope.kind(), ScopeKind::Comprehension);
        assert_eq!(
            inner_comprehension_scope_id.to_scope_id(&db, file).name(&db),
            "<setcomp>"
        );

        let inner_comprehension_symbol_table = index.symbol_table(inner_comprehension_scope_id);

        assert_eq!(names(&inner_comprehension_symbol_table), vec!["x"]);
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:262 on 2024-08-12 19:11_

```suggestion
    /// Visit a list of [`Comprehension`] nodes, assumed to be the "generators" that compose a
    /// comprehension (that is, the `for x in y` and `for y in z` parts of `x for x in y for y in z`.)
```

---

_@carljm approved on 2024-08-12 19:14_

Looks great! Just a few naming things left to clean up, mostly in the tests.

---

_Merged by @dhruvmanila on 2024-08-13 01:30_

---

_Closed by @dhruvmanila on 2024-08-13 01:30_

---

_Branch deleted on 2024-08-13 01:30_

---

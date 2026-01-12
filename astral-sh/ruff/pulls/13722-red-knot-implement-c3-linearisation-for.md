```yaml
number: 13722
title: "[red-knot] Implement C3 linearisation for calculating a class's Method Resolution Order"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
base: main
head: alex/mro
created_at: 2024-10-11T22:08:08Z
updated_at: 2024-10-31T22:24:43Z
url: https://github.com/astral-sh/ruff/pull/13722
synced_at: 2026-01-12T15:55:45Z
```

# [red-knot] Implement C3 linearisation for calculating a class's Method Resolution Order

---

_@AlexWaygood_

## Summary

A Python class's ["Method Resolution Order"](https://docs.python.org/3/glossary.html#term-method-resolution-order) ("MRO") is the order in which superclasses of that class are traversed by the Python runtime when searching for an attribute (which includes methods) on that class. Accurately inferring a class's MRO is essential for a type checker if it is going to be able to accurately lookup the types of attributes and methods accessed on that class (or instances of that class).

For simple classes, the MRO (which is accessible at runtime via the `__mro__` attribute on a class) is simple:

```pycon
>>> object.__mro__
(<class 'object'>,)
>>> class Foo: pass
... 
>>> Foo.__mro__  # classes without explicit bases implicitly inherit from `object`
(<class '__main__.Foo'>, <class 'object'>)
>>> class Bar(Foo): pass
... 
>>> Bar.__mro__
(<class '__main__.Bar'>, <class '__main__.Foo'>, <class 'object'>)
```

For more complex classes that use multiple inheritance, things can get a bit more complicated, however:

```pycon
>>> class Foo: pass
... 
>>> class Bar(Foo): pass
... 
>>> class Baz(Foo): pass
... 
>>> class Spam(Bar, Baz): pass
... 
>>> Spam.__mro__  # no class ever appears more than once in an `__mro__`
(<class '__main__.Spam'>, <class '__main__.Bar'>, <class '__main__.Baz'>, <class '__main__.Foo'>, <class 'object'>)
```

And for some classes, Python realises that it cannot determine which order the superclasses should be positioned in order to create the MRO at class creation time:

```pycon
>>> class Foo(object, int): pass
... 
Traceback (most recent call last):
  File "<python-input-12>", line 1, in <module>
    class Foo(object, int): pass
TypeError: Cannot create a consistent method resolution order (MRO) for bases object, int
>>> class A: pass
... 
>>> class B: pass
... 
>>> class C(A, B): pass
... 
>>> class D(B, A): pass
... 
>>> class E(C, D): pass
... 
Traceback (most recent call last):
  File "<python-input-17>", line 1, in <module>
    class E(C, D): pass
TypeError: Cannot create a consistent method resolution order (MRO) for bases A, B
```

The algorithm Python uses at runtime to determine what a class's MRO should be is known as the C3 linearisation algorithm. An in-depth description of the motivations and details of the algorithm can be found in [this article](https://docs.python.org/3/howto/mro.html#python-2-3-mro) in the Python docs. The article is quite old, however, and the algorithm given at the bottom of the page is written in Python 2. As part of working on this PR, I translated the algorithm first into Python 3 (see [this gist](https://gist.github.com/AlexWaygood/674db1fce6856a90f251f63e73853639)), and then into Rust (the `c3_merge` function in `mro.rs` in this PR). In order for us to correctly infer a class's MRO, we need our own implementation of the C3 linearisation algorithm.

The C3 linearisation algorithm isn't _too_ complicated by itself. However, there are additional complexities for a type checker when it comes to calculating a class's MRO. The C3 linearisation algorithm calculates an MRO from a list of bases given to it: but what if one of the bases of a class is inferred as a `Union` type by red-knot? In this situation, there will be multiple possible lists of bases, and therefore potentially multiple possible MROs, for that single class. For this reason, mypy and pyright both reject classes that have an object in their bases list which is inferred as a `Union` type, as it adds a fair amount of complexity to the semantic model. However, this PR nonetheless attempts to support such cases, for several reasons:
- As long as all elements in the union are valid class bases, it's perfectly type-safe. We should, in general, try not to emit false positives on valid code.
- It's nice to support more things than mypy and pyright do; we should attempt to provide added value over these existing type checkers that goes beyond simply being faster
- Doing multi-version checking will probably be difficult without this feature due to the way that typeshed branches on `sys.version_info` conditions, which means that we will likely often infer a union for an object used as a class base.

As well as having to adapt the C3 linearisation algorithm to account for the possibility that a class might have a `Union` in its bases, I also had to make some changes to account for the fact that a class might have a dynamic type in its bases: in our current model, we have three dynamic types, which are `Unknown`, `Any` and `Todo`. This PR takes the simple approach of deciding that the MRO of `Any` is `[Any, object]`, the MRO of `Unknown` is `[Unknown, object]`, and the MRO of `Todo` is `[Todo, object]`; other than that, they are not treated particularly specially by the C3 linearisation algorithm. Other than simplicity, this has a few advantages:
- It matches the runtime:
  ```pycon
  >>> from typing import Any
  >>> Any.__mro__
  (typing.Any, <class 'object'>)
  ```
- It means that they behave just like any other class base in Python: an invariant upheld by all other class bases in Python is that they all inherit from `object`.

Dynamic types will have to be treated specially when it comes to attribute and method access from these types; however, that is for a separate PR.

## Implementation strategy

The implementation is encapsulated in a new `red_knot_python_semantic` submodule, `types/mro.rs`. `ClassType::mro_possibilities` returns a HashSet of possible MROs that the class could have given its bases, and this calls out to functions in the `mro.rs` submodule. If there are no union types in the bases list (or in any of the bases of those bases), then there will be exactly one possible MRO for the class; but if there are any union types in the class's bases or the bases of those bases (etc.), then the class could have one of several possible MROs. At each stage, the implementation tries to avoid forking if possible. I've attempted to optimise the implementation based on the assumption that most classes will not have union types in their bases (or the bases of their bases, etc.); there are various fast paths that are predicated on this.

It's necessary for us to emit a diagnostic if we can determine that a particular list of bases will (or could) cause a `TypeError` to be raised at runtime due to an unresolvable MRO. However, we can't do this while creating the `ClassType` and storing it in `self.types.declarations` in `infer.rs`, as we need to know the bases of the class in order to determine its MRO, and some of the bases may be deferred. This PR therefore iterates over all classes in a certain scope after all types (including deferred types) have been inferred, as part of `TypeInferenceBuilder::infer_region_scope`. For types that will (or could) raise an exception due to an invalid MRO, we infer the MRO as being `[<class in question>, Unknown, object]` as well as emitting the diagnostic.

## For discussion

There's a performance regression of around 4% on the codspeed incremental benchmark. I've tried to get it down a bit, and it has gone down a bit, but I'm out of ideas for how to reduce it further. Open to suggestions :-)

## Test Plan

Beautiful Markdown tests added using @carljm's new test framework. Several tests taken from https://docs.python.org/3/howto/mro.html#python-2-3-mro.

---

_Label `red-knot` added by @AlexWaygood on 2024-10-11 22:08_

---

_Renamed from "Add a failing test" to "[WIP] [red-knot] Implement C3 linearisation for calculating a Method Resolution Order" by @AlexWaygood on 2024-10-11 22:08_

---

_Renamed from "[WIP] [red-knot] Implement C3 linearisation for calculating a Method Resolution Order" to "[WIP] [red-knot] Implement C3 linearisation for calculating a class's Method Resolution Order" by @AlexWaygood on 2024-10-11 22:08_

---

_Comment by @github-actions[bot] on 2024-10-11 22:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (error)</summary>
<p>

```
Failed to clone ibis-project/ibis: error: RPC failed; curl 92 HTTP/2 stream 5 was not closed cleanly: CANCEL (err 8)
error: 4518 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
Failed to clone ibis-project/ibis: error: RPC failed; curl 92 HTTP/2 stream 5 was not closed cleanly: CANCEL (err 8)
error: 6680 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>




---

_Comment by @MichaReiser on 2024-10-12 16:13_

Do you have a reference that explains what the C3 linearisation is? It would help me understanding the PR

---

_Comment by @AlexWaygood on 2024-10-12 16:15_

> Do you have a reference that explains what the C3 linearisation is? It would help me understanding the PR

- https://docs.python.org/3/glossary.html#term-method-resolution-order
- https://docs.python.org/3/howto/mro.html#python-2-3-mro

Both linked to in various doc comments. I'll fill in the PR description more when it's closer to being ready for review :-)

---

_Comment by @AlexWaygood on 2024-10-12 16:16_

> [docs.python.org/3/howto/mro.html#python-2-3-mro](https://docs.python.org/3/howto/mro.html#python-2-3-mro)

The algorithm written at the bottom of this document is unfortunately written in Python 2, but I've written a Python 3 translation here: https://gist.github.com/AlexWaygood/674db1fce6856a90f251f63e73853639

---

_Comment by @codspeed-hq[bot] on 2024-10-12 18:00_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex/mro)

### Merging #13722 will **degrade performances by 4.05%**

<sub>Comparing <code>alex/mro</code> (953f6f8) with <code>main</code> (04b636c)</sub>



### Summary

`❌ 1` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex/mro)._

### Benchmarks breakdown

|     | Benchmark | `main` | `alex/mro` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `red_knot_check_file[incremental]` | 4.5 ms | 4.7 ms | -4.05% |


---

_Marked ready for review by @AlexWaygood on 2024-10-12 20:47_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-12 20:47_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-12 20:47_

---

_Renamed from "[WIP] [red-knot] Implement C3 linearisation for calculating a class's Method Resolution Order" to "[red-knot] Implement C3 linearisation for calculating a class's Method Resolution Order" by @AlexWaygood on 2024-10-12 20:47_

---

_Comment by @AlexWaygood on 2024-10-12 20:52_

@MichaReiser I've updated the PR summary and taken the PR out of draft mode :-)

---

_@AlexWaygood reviewed on 2024-10-13 13:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/mro.md`:72 on 2024-10-13 13:31_

I'm not a massive fan of using `O` (that's a capital-O letter!) as a variable name here, because it's so easy to misread it as `0` (the digit zero). However, I experimented with using a different variable name, and I found it even more confusing, because it was very hard to remember which class it corresponded to in the `ex_2` example in https://docs.python.org/3/howto/mro.html#the-end. Unfortunately that article uses `O` as the class name.

---

_Comment by @MichaReiser on 2024-10-14 12:35_

@AlexWaygood do you have an understanding of what's causing the perf regression?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1363 on 2024-10-14 12:51_

Nit: I find the name possibilities a bit weird. How about simplifying it to `Mro` or `ClassMro`? 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1363 on 2024-10-14 12:53_

Do we expect this method to be called frequently for it to be worth to track as a separate salsa query?

Could you add an example where the resolved MRO has a cycle.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1425 on 2024-10-14 12:55_

I suggest that we 
* implement this as an `Iterator` to avoid allocating for every class if the code is in the hot path (probably?)
* return an `impl Iterator` and always returning a `Vec` over a `Box<[]>` (boxed slices are mainly useful when storing in another struct it doesn't matter much when the variable is stored on the stack) when the code isn't performance sensitive.

---

_Comment by @AlexWaygood on 2024-10-14 12:57_

> @AlexWaygood do you have an understanding of what's causing the perf regression?

Not fully. The CodSpeed flamegraph isn't very informative because of all the Salsa frames.

The fact that the incremental benchmark regresses much more than the cold benchmark makes me think that it's something to do with the method having the `#[salsa::tracked]` attribute on it: these are reasonably large objects I'm storing in the Salsa cache. I tried to reduce the size of the objects stored in the cache by doing something like this:

```diff
diff --git a/crates/red_knot_python_semantic/src/types/mro.rs b/crates/red_knot_python_semantic/src/types/mro.rs
index b32faf71a..ad4b6ea73 100644
--- a/crates/red_knot_python_semantic/src/types/mro.rs
+++ b/crates/red_knot_python_semantic/src/types/mro.rs
@@ -20,19 +20,19 @@ pub(super) enum MroPossibilities<'db> {
     /// There are multiple possible `__mro__` values for this class, but they would
     /// all lead to the class being successfully created. Here are the different
     /// possibilities:
-    MultipleSuccesses(FxHashSet<Mro<'db>>),
+    MultipleSuccesses(Box<[Mro<'db>]>),
 
     /// It can be statically determined that the `__mro__` possibilities for this class
     /// (possibly one, possibly many) always fail. Here are the various possible
     /// bases that all lead to class creation failing:
-    CertainFailure(FxHashSet<BasesList<'db>>),
+    CertainFailure(Box<[BasesList<'db>]>),
 
     /// There are multiple possible `__mro__`s for this class. Some of these
     /// possibilities result in the class being successfully created; some of them
     /// result in class creation failure.
     PossibleSuccess {
-        possible_mros: FxHashSet<Mro<'db>>,
-        failure_cases: FxHashSet<BasesList<'db>>,
+        possible_mros: Box<[Mro<'db>]>,
+        failure_cases: Box<[BasesList<'db>]>,
     },
 
```

but unfortunately converting the HashSets into boxed slices seemed to cause a slowdown rather than a speedup; the cost of the conversion and new allocation was worse than any speed gained by storing smaller objects in the cache. Defining the `Mro` type as `struct Mro<'db>(Box<[ClassBase<'db>]>);` rather than `struct Mro<'db>(VecDeque<ClassBase<'db>>);` (which it was in an earlier iteration) _did_ provide a significant speedup, however.

The algorithm is in itself also inherently nontrivial (even with the fast paths I added for the common cases), and we have to call it for every class definition we encounter. There's quite a lot of allocation currently in some places: I think some of this is unfortunately unavoidable, but `fork_bases` and `add_next_base` could probably be improved somewhat.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1463 on 2024-10-14 13:00_

Nit: Isn't this the same as using `break`?
```suggestion
                    break;
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:419 on 2024-10-14 13:03_

It might help to move out of `declarations` for better performance. 

```suggestion
        let declarations = std::mem::take(&mut self.types.declarations);

        let class_definitions = declarations.values().filter_map(|ty| ty.into_class_type());

        for class in class_definitions {
            if let Some(mro_errors) = class.mro_possibilities(self.db).possible_errors() {
                for error in mro_errors {
                    self.add_diagnostic(
                        class.node(self.db).into(),
                        "inconsistent-mro",
                        format_args!(
                            "Cannot create a consistent method resolution order (MRO) for class `{}` with possible bases list `[{}]`",
                            class.name(self.db),
                            error.iter().map(|base|base.display(self.db)).join(", ")
                        )
                    );
                }
            }
        }

        self.types.declarations = declarations;
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:387 on 2024-10-14 13:05_

Can these just be `Option` rather than `Once`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:182 on 2024-10-14 13:11_

You can avoid the `expect` call by using
```suggestion
            let mro = match single_base.into_class_type() {
                Some(class) if class.is_known(db, KnownClass::Object) => {
                    MroPossibilities::single([class])
                }
                _ => {
                    let mut possibilities = FxHashSet::default();
                    let base = ClassBase::from(single_base);
                    for possibility in base.mro_possibilities(db).iter(db, base) {
                        possibilities.insert(
                            std::iter::once(ClassBase::Class(class))
                                .chain(possibility.iter().copied())
                                .collect(),
                        );
                    }
                    MroPossibilities::possibly_many(possibilities, FxHashSet::default())
                }
            };
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:180 on 2024-10-14 13:15_

I'm not sure I fully understand what's happening here but what I read here is that we always allocate a hash set if a class has an explicit base even though we only end up with a unique "MRO". 

```python
class A:
	...

class B(A): ...
```

It would be nice if we could do better in this very basic case, e.g. by testing if `mro_possibilities` is `Single` and, if so, simply propagating `class` but also propagating the `Single` case.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:531 on 2024-10-14 13:19_

What's the meaning of an empty base list. Is it if there's no explicit base?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:358 on 2024-10-14 13:21_

I'm struggling a bit with the names. There are so many `Bases` and lists. I think a module-level comment explaining the different types and how this all fits together would be useful. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:339 on 2024-10-14 13:22_

What I understand is that this can only happen if any type in `__bases__` resolves to a union. Mentioning unions here could be useful

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:531 on 2024-10-14 13:23_

It could be worth to use a `SmallVec` to avoid allocating in the most common cases where the class has only very few bases.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:313 on 2024-10-14 13:26_

Can you explain why one variant is always the implicit `object` parent even if it isn't included in the `bases` of the parents?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:325 on 2024-10-14 13:28_

What's the reason why we copy over the `base_possibilities` into a `new_possibilities` hash set. This seems very expensive because it results in `O(cases)` allocations and memcopy. 

Could we initialize the hash map with the `bases_possibilities` length to reduce re-hashing?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:366 on 2024-10-14 13:33_

All the `Once` usages should probably just be `Option` with a call to `value.take()?` inside the  `next` call`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:456 on 2024-10-14 13:37_

I would be surprised if `display` returns something that doesn't implement `std::fmt::Display`
```suggestion
    pub(super) fn display(self, db: &'db dyn Db) -> impl std::fmt::Display {
        struct Display<'a> {
            base: &'a ClassBase<'a>,
            db: &'a dyn Db,
        }

        impl std::fmt::Display for Display<'_> {
            fn fmt(&self, f: &mut Formatter<'_>) -> std::fmt::Result {
                match self.base {
                    Self::Any => f.write_str("Any"),
                    Self::Todo => f.write_str("Todo"),
                    Self::Unknown => f.write_str("Unknown"),
                    Self::Class(class) => write!(f, "<class '{}'>", class.name(self.db)),
                }
            }
        }
    }
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:624 on 2024-10-14 13:45_

Does the sequence order matter? It might otherwise be faster to use `swap_remove` (and remove the element when the `pop_front` call poped the last element)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:650 on 2024-10-14 13:47_

It seems we're always poping from the front. Would it instead be possible to use regular `Vec`s and reverse the elements (and poping from the end)?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:630 on 2024-10-14 13:52_

Nit: I would combine the two iterators because the iterator code adds more boilerplate, making it harder to follow what's happening

```suggestion
        // If the candidate exists "deeper down" in the inheritance hierarchy,
        // we should refrain from adding it to the MRO for now. Add the first candidate
        // for which this does not hold true. If this holds true for all candidates,
        // return `None`; it will be impossible to find a consistent MRO for the class
        // with the given bases.
        let mro_entry = sequences.iter().find_map(|outer_sequence| {
            let candidate = outer_sequence[0];

            let some_condition = sequences
                .iter()
                .all(|sequence| sequence.iter().skip(1).all(|base| base != candidate));

            some_condition.then_some(candidate)
        })?;

        mro.push(mro_entry);
```

---

_@MichaReiser reviewed on 2024-10-14 13:53_

Thanks for the excellent write up. 

This code is indeed complicated. I don't think I fully understand what's going on and supporting multiple MROs also results in a fair amount of allocations and hashing all over the place (both are expensive). That's why I'm inclined to not support union MROs unless we have common use cases demonstrating why it's needed. I do fear that this requires making a decision on `sys.version_info`. 
I think we should prioritise our decision on checking multiple python versions because my first reaction on supporting unions was to defer the implementation (and all downstream complexity) until we have real use cases where it matters. But you then mentioned `sys.version_info` support... 

I do feel like there's a lot of boilerplate going on with many intermediate types. I would have to take a deeper look to understand if it's possible to simplify it some way but I noted that I struggled to connect all the dots and would probably also struggle to make changes to the implementation. I'm not sure if there's any simplification potential but it might be worth going over the implementation again to see if we can remove some of the many intermediate structs.

---

_@AlexWaygood reviewed on 2024-10-14 14:00_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/mro.rs`:531 on 2024-10-14 14:00_

The `BasesList` represents all bases the class has (both implicit and explicit). For a class that implicitly inherits from `object`, e.g. `class Foo: pass`, the bases of `Foo` will be `[object]`. Only one class has an empty bases list in Python: `object` itself:

```pycon
>>> object.__bases__
()
>>> class Foo: pass
... 
>>> Foo.__bases__
(<class 'object'>,)
```

---

_@AlexWaygood reviewed on 2024-10-14 14:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/mro.rs`:531 on 2024-10-14 14:01_

I tried using `SmallVec` but struggled to get it to work. I'll have another go.

---

_@MichaReiser reviewed on 2024-10-14 14:02_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:531 on 2024-10-14 14:02_

Yeah, small vec has a bug around lifetime variants. 

---

_@AlexWaygood reviewed on 2024-10-14 14:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/mro.rs`:313 on 2024-10-14 14:03_

If a class does not explicitly inherit from anything, Python synthesizes a `__bases__` tuple consisting solely of `object`. This is because all classes in Python are subclasses of `object`, which is the top type, the supertype of everything else.

```pycon
>>> class Foo: pass
... 
>>> Foo.__bases__
(<class 'object'>,)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/mro.rs`:624 on 2024-10-14 14:04_

yes, the sequence order is crucial here

---

_@AlexWaygood reviewed on 2024-10-14 14:04_

---

_@AlexWaygood reviewed on 2024-10-14 14:05_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/mro.rs`:630 on 2024-10-14 14:05_

Oh, sure. I did it that way originally, but rewrote it like this for clarity 😄 I'll change it back if that's backfiring!

---

_@AlexWaygood reviewed on 2024-10-14 14:06_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1363 on 2024-10-14 14:06_

I chose this name because it returns an object that represents the set of possible MROs the class could have: the returned object is not a single MRO

---

_@MichaReiser reviewed on 2024-10-14 14:11_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1363 on 2024-10-14 14:11_

Yeah I understand that. It just sounds so very uncertain. How about `ClassMros`?





---

_@AlexWaygood reviewed on 2024-10-14 14:12_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1363 on 2024-10-14 14:12_

Oh I see. Sure, I'll go for something like that!

---

_@AlexWaygood reviewed on 2024-10-14 14:29_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1463 on 2024-10-14 14:29_

No, we need to skip the `union_builder = union_builder.add(Type::Unbound);` line which is called after the termination of the inner loop. We wouldn't skip that line if we used `break`.


---

_@AlexWaygood reviewed on 2024-10-14 14:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1363 on 2024-10-14 14:30_

I expect it to be called very frequently. We need to iterate over the MRO whenever we resolve the type of an attribute or method access (explicit or implict) on a class or an instance of a class. This PR already starts using it in `ClassType::inherited_class_member`, which currently (incorrectly) only iterates over a class's bases rather than over a class's entire MRO.

---

_@AlexWaygood reviewed on 2024-10-14 14:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1363 on 2024-10-14 14:32_

> Could you add an example where the resolved MRO has a cycle.

Hmmm... what are you thinking of exactly? An MRO that requires an import cycle to be resolved?

---

_@AlexWaygood reviewed on 2024-10-14 14:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/mro.rs`:182 on 2024-10-14 14:48_

Very nice!

---

_@AlexWaygood reviewed on 2024-10-14 14:57_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/mro.rs`:180 on 2024-10-14 14:57_

Yeah, I think you're right, we can take hte fast path much more often than I do currently. I'll rejig this.

---

_@AlexWaygood reviewed on 2024-10-14 16:00_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/mro.rs`:650 on 2024-10-14 16:00_

Would you expect that to be more performant than using a `VecDeque` here?

---

_@MichaReiser reviewed on 2024-10-14 16:07_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:650 on 2024-10-14 16:07_

Probably not by much but a `Vec` is simpler than a `VecDeque`. It also simplifies the method signature by requiring a less specific type (requiring a `Vec` is more common)

---

_@carljm reviewed on 2024-10-14 16:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1363 on 2024-10-14 16:43_

> what are you thinking of exactly? An MRO that requires an import cycle to be resolved?

No, an actual cycle in the MRO, i.e. what you'd get if you had `class str(str): ...` in a type stub. Clearly we can't allow this and have to error on it as an invalid MRO, but it would be good to have a test showing that we do this, rather than getting stuck in an infinite loop building the MRO.

---

_@AlexWaygood reviewed on 2024-10-14 17:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1363 on 2024-10-14 17:26_

Ahh, got it, thanks.

---

_@AlexWaygood reviewed on 2024-10-14 17:27_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/mro.rs`:325 on 2024-10-14 17:27_

Yeah, I think I can rework this function and do it more cheaply and simply. Will attempt to do so.

---

_@AlexWaygood reviewed on 2024-10-14 18:07_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/mro.rs`:650 on 2024-10-14 18:07_

I'll try this out and see if it's workable. A downside is that the implementation will become less recognisably a reimplementation of the algorithm given in the Python docs, so it'll be less self-evident that the function does the correct thing. But I've already made many changes in the process of rewriting it in Python 3, then Rust, so perhaps that's already lost

---

_Comment by @carljm on 2024-10-16 12:43_

I'm not doing a detailed review of this yet because my understanding is we need to come to agreement on multi-version checking before we are ready to land this with union support. Let me know if that's wrong.

---

_Comment by @AlexWaygood on 2024-10-16 12:45_

> I'm not doing a detailed review of this yet because my understanding is we need to come to agreement on multi-version checking before we are ready to land this with union support. Let me know if that's wrong.

That, and also @MichaReiser's review made me look again and realise that I can make some of this a fair bit more efficient and easier to understand with a rewrite :-) So yes, not much use looking at it right now.

---

_Comment by @MichaReiser on 2024-10-16 13:08_

We could also consider splitting the PR so that we can land a "simple" version now. That would also give us a nice "diff" to assess the extra complexity

---

_Comment by @carljm on 2024-10-18 19:26_

It occurred to me today that if we do want to go forward with supporting multi-version-checking and thus unions in class bases, the better representation for that would probably be to have the single `class` statement result in a union of `Type::Class`, one for each possible MRO. Having a single `ClassType` with multiple MRO will I think result in us having to effectively re-implement a lot of union handling inside methods of `ClassType`.

The trick here is that we can't really do this easily in stub files, where type inference of class bases can be cyclic and must be deferred :/ I wonder if the right fix for that is to not actually defer resolution of class bases in stub files, but only defer resolution of generic parameters _within_ class bases; this would be sufficient to handle the actual cases that we need to support (like `class str(Sequence[str])` -- we don't actually need to support `class str(str):`, because it's meaningless.)

---

_Comment by @AlexWaygood on 2024-10-31 22:24_

Closing in favour of #14027, since we've decided to postpone a decision on whether to do multi-version checking for now

---

_Closed by @AlexWaygood on 2024-10-31 22:24_

---

_Branch deleted on 2024-10-31 22:24_

---

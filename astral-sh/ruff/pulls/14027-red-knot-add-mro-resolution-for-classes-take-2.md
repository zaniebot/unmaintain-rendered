```yaml
number: 14027
title: "[red-knot] Add MRO resolution for classes (take 2)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/simple-mro
created_at: 2024-10-31T22:18:58Z
updated_at: 2024-11-04T20:11:11Z
url: https://github.com/astral-sh/ruff/pull/14027
synced_at: 2026-01-12T15:55:46Z
```

# [red-knot] Add MRO resolution for classes (take 2)

---

_@AlexWaygood_

A second attempt at implementing MRO resolution (see #13722 for the first attempt). Unlike the first attempt, this version does _not_ attempt to handle union types in a class's bases list (which would potentially mean that you would have to consider multiple possible MROs for any given class).

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

As well as implementing the C3 linearisation algorithm in Rust, I also had to make some changes to account for the fact that a class might have a dynamic type in its bases: in our current model, we have three dynamic types, which are `Unknown`, `Any` and `Todo`. This PR takes the simple approach of deciding that the MRO of `Any` is `[Any, object]`, the MRO of `Unknown` is `[Unknown, object]`, and the MRO of `Todo` is `[Todo, object]`; other than that, they are not treated particularly specially by the C3 linearisation algorithm. Other than simplicity, this has a few advantages:
- It matches the runtime:
  ```pycon
  >>> from typing import Any
  >>> Any.__mro__
  (typing.Any, <class 'object'>)
  ```
- It means that they behave just like any other class base in Python: an invariant upheld by all other class bases in Python is that they all inherit from `object`.

Dynamic types will have to be treated specially when it comes to attribute and method access from these types; however, that is for a separate PR.

## Implementation strategy

The implementation is encapsulated in a new `red_knot_python_semantic` submodule, `types/mro.rs`. `ClassType::try_mro` attempts to resolve a class's MRO, and returns an error if it cannot; `ClassType::mro` is a wrapper around `ClassType::try_mro` that resolves the MRO to `[<class in question>, Unknown, builtins.object]` if no MRO for the class could be resolved.

It's necessary for us to emit a diagnostic if we can determine that a particular list of bases will (or could) cause a `TypeError` to be raised at runtime due to an unresolvable MRO. However, we can't do this while creating the `ClassType` and storing it in `self.types.declarations` in `infer.rs`, as we need to know the bases of the class in order to determine its MRO, and some of the bases may be deferred. This PR therefore iterates over all classes in a certain scope after all types (including deferred types) have been inferred, as part of `TypeInferenceBuilder::infer_region_scope`. For types that will (or could) raise an exception due to an invalid MRO, we infer the MRO as being `[<class in question>, Unknown, object]` as well as emitting the diagnostic.

We also emit diagnostics for classes that inherit from bases which would be too complicated for us to resolve an MRO for: anything except a class-literal, `Any`, `Unknown` or `Todo` is rejected.

I deleted the `ClassType::bases()` method, because:
- It's easier to calculate the MRO if you work directly from the AST rather than having an intermediate method that converts the slice of AST nodes into an iterator of types
- There are no direct uses of `ClassType::bases()` in `types.rs` or `infer.rs` anymore now (they all should have been iterating over the MRO all along!).
- The `bases()` method was something of a footgun: it only gave you the slice of the class's _explicit_ bases, and ignored the fact that a class will _implicitly_ have `object` in its bases list at runtime if it has no _explicit_ bases.

## Test Plan

Lots of mdtests added. Several tests taken from https://docs.python.org/3/howto/mro.html#python-2-3-mro.

---

_Label `red-knot` added by @AlexWaygood on 2024-10-31 22:18_

---

_Comment by @github-actions[bot] on 2024-11-01 12:53_

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

_Marked ready for review by @AlexWaygood on 2024-11-01 12:55_

---

_Review requested from @carljm by @AlexWaygood on 2024-11-01 12:55_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-01 12:55_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-11-01 12:55_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1883 on 2024-11-01 14:16_

Nit: A `bases` method on `mro` would be nice that hides the fact that it skips the current class.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/mro.md`:346 on 2024-11-01 14:21_

Could we add a case where we have an indirect cycle (which is probably more common)

```py
class Foo(Bar): ...

class Bar(Baz): ...

class Baz(Foo): ...
```

and with a union

```
class Foo(Bar): ...

class Bar(Baz): ...

class Bar2: ...

class Baz(Foo, Bar2) ...
```


and a sub-graph

```
class FooCycle(BarCycle): ...

class Foo: ...

class BarCycle(FooCycle) ...

class Bar(Foo): ...

class Baz(Bar, BarCycle): ...


```



---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:215 on 2024-11-01 14:23_

Could we document the meaning of the `usize` or use a named field?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:205 on 2024-11-01 14:24_

Could you add a comment explaining what the indices are indexing. Maybe consider using named fields or a type alias.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:16 on 2024-11-01 14:26_

Do we need hash? 

```suggestion
#[derive(PartialEq, Eq, Default, Clone, Debug)]
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:69 on 2024-11-01 14:27_

Considering that this is very common. I wonder if it's worth having a special `Mro::Default(class)` variant that lazily resolves `object` or to use a small vec

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:109 on 2024-11-01 14:30_

I'm not sure if that works with indirect cycles (see tests pointed out above). 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:479 on 2024-11-01 14:38_

Is it possible that multiple sequences have the same first-mro? Or is this really about removing the found `mro_entry`?
```suggestion
        let mro_sequence_index = sequences.iter().position(|outer_sequence| {
            let candidate = outer_sequence[0];

            let not_head = sequences
                .iter()
                .all(|sequence| sequence.iter().skip(1).all(|base| base != &candidate));

            not_head
        })?;

        let mro_sequence = &mut sequences[mro_sequence_index];
        mro.push(mro_sequence[0]);

        // Make sure we don't try to add the candidate to the MRO twice:
        mro_sequence.pop_front();

        if mro_sequence.is_empty() {
            sequences.remove(mro_sequence_index);
        }
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:433 on 2024-11-01 14:42_

Do we still need this trick, now that `diagnostics` is its own field?

---

_@MichaReiser reviewed on 2024-11-01 14:44_

This is great and I like the simplification. I would be interested in a few more cycle tests to make sure that indirect cycles are correctly handled too.

---

_@AlexWaygood reviewed on 2024-11-01 14:49_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:433 on 2024-11-01 14:49_

We don't, good catch!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1883 on 2024-11-01 14:52_

Hmm, I'm not sure what we'd call such a method:
- `bases()` would be inaccurate, because it would iterate through all proper superclasses of the class, not just the class's direct bases
- `superclasses()` would also be inaccurate, because every class is a superclass of itself, but we would want to exclude `self` from the elements yielded by this method.
- `proper_superclasses()` would be accurate, but feels quite jargony, and isn't really any less verbose than `.elements().skip(1)`

---

_@AlexWaygood reviewed on 2024-11-01 14:52_

---

_@MichaReiser reviewed on 2024-11-01 15:36_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1883 on 2024-11-01 15:36_

how about `direct_bases`? I think I read this snippet twice and was both times wondering why there's a skip 1

Edit: Oh sorry, you said it's not `direct_bases` not that it is lol. How about `inherited_classes`? I also think that `superclasses()` is kind of fine. The fact that every class is a `superclasses` feels a bit nitpicky

---

_@AlexWaygood reviewed on 2024-11-01 15:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1883 on 2024-11-01 15:39_

Again... `direct_bases` would be the _opposite_ of what this proposed method would do. It would iterate through _all_ proper superclasses of the class, _not just_ the direct bases :-)

---

_@AlexWaygood reviewed on 2024-11-01 15:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1883 on 2024-11-01 15:52_

I fixed this by deleting the method -- inlining it means we don't even need the `.skip(1)` call at all anymore ;)

---

_@AlexWaygood reviewed on 2024-11-01 15:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/mro.md`:346 on 2024-11-01 15:54_

Nice. Yeah, even just your first one triggers a panic on this branch.

> and with a union

Just checking, did you mean "multiple inheritance" here? There are no union types there, as far as I can tell :-)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/mro.md`:332 on 2024-11-01 15:58_

Hmm, I'm not sure about this. Is inheriting from two unknowns really any different (in terms of the degree to which we have no idea what is going on) than inheriting from a single unknown (which doesn't trigger a "cannot create a consistent MRO" error)?

This violates the gradual guarantee, so I think we should err on the side of not doing it. (I don't think it's super important, though, so if _not_ erroring here is hard to implement, we can just alter the comment here to be a "TODO don't emit this error" for now.)

---

_Comment by @codspeed-hq[bot] on 2024-11-01 15:58_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex/simple-mro)

### Merging #14027 will **not alter performance**

<sub>Comparing <code>alex/simple-mro</code> (a9d379b) with <code>main</code> (88d9bb1)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:65 on 2024-11-01 15:59_

Why did `__module__` disappear here?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/stubs/class.md`:8 on 2024-11-01 16:00_

Did you verify that this test fails if you make this a `.py` instead of a `.pyi`? (Just want to make sure it's still testing something meaningful.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1815 on 2024-11-01 16:03_

should this be an `unreachable!`?

---

_@MichaReiser reviewed on 2024-11-01 16:05_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/mro.md`:346 on 2024-11-01 16:05_

Lol yeah, union makes no sense here. I'm mainly interested to see how the MRO recovers if there are multiple base classes and there's a cycle in one "base-class-branch"

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1843 on 2024-11-01 16:07_

This means that if we have a class with an error MRO, we reconstruct a brand-new owned error MRO for that class every time its MRO is accessed. Maybe that's the best tradeoff here? We can see if it shows up in profiles in practice. I guess the alternative would require making both of these methods salsa tracked, which increases the query count.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1852 on 2024-11-01 16:08_

```suggestion
                // MRO.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1883 on 2024-11-01 16:12_

I would be OK with `proper_superclasses()`. I don't care if it's long. The goal of adding a method here  is semantic clarity, not concision. Micha's issue here was not "`elements().skip(1)` is too long", it was "it's not self-evident why we are skipping 1" -- and I agree with that.

We could also go less jargony and just call it `without_self()`?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1883 on 2024-11-01 16:13_

But, inlining it leads to a large perf regression because we now need to resolve the MRO for many more attribute lookups 😆

---

_@AlexWaygood reviewed on 2024-11-01 16:13_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1904 on 2024-11-01 16:14_

```suggestion
                    # TODO we may instead want to record the fact that we encountered dynamic, and intersect it with
                    # the type found on the next real class.
                    return Type::from(superclass).member(db, name)
```

---

_@carljm reviewed on 2024-11-01 16:19_

Not finished reviewing yet, but submitting some comments now rather than waiting because you're actively working on the branch :)

Overall, this is awesome!

---

_@carljm reviewed on 2024-11-01 16:23_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1904 on 2024-11-01 16:23_

Ha, not me writing Python comments in Rust 😆 But you get the idea.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/mro.md`:212 on 2024-11-01 16:28_

You went to some trouble in the implementation to ensure that these diagnostics span the correct node in the bases list. (Which is great!) Maybe use the mdtest feature to assert on the start column of the error, to test this? (And similar for other errors in these tests where it is relevant.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4406 on 2024-11-01 16:31_

TBH I feel you could just delete this test. It doesn't test anything that isn't covered much better by your added MRO tests in this PR.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/mro.rs`:79 on 2024-11-01 16:37_

This approach won't work for handling indirect cycles. I think you'll need to test if we appear anywhere in `Mro::of_base().elements()`?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:65 on 2024-11-01 16:48_

Instances of `ModuleType` don't actually have `__module__` attributes in "real life" 😆 So it seemed like a bad thing to assert in a test, in retrospect.

We infer that they _do_ have `__module__` attributes because of that slightly-irritating inaccuracy in typeshed that we discussed the other day.

---

_@AlexWaygood reviewed on 2024-11-01 16:48_

---

_@AlexWaygood reviewed on 2024-11-01 16:49_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/mro.md`:332 on 2024-11-01 16:49_

Hmm, yeah, not sure either. I can see arguments both ways. The main thing I wanted to avoid in this initial implementation was a complaint about "duplicate base classes", which would obviously be flat-out wrong.

I'll add a TODO for now; if we _did_ want to avoid emitting any error here, it would add some more complexity, and I think it's okay to postpone the details on this point.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/mro.rs`:479 on 2024-11-01 16:50_

It is possible that multiple sequences have the same first entry, and this is a key part of the algorithm. (It's why the `let not_head = ...` thing checks only for presence in not-first position, because if it's in first position in multiple sequences we can handle it now and pop it from all of them.) So this change won't work.

---

_@AlexWaygood reviewed on 2024-11-01 16:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1843 on 2024-11-01 16:50_

I think I can actually store the fallback MRO on the `MroError`. It also has the advantage that it makes some signatures a bit simpler (less need for `Cow`s everywhere).

I'll try that now and see what Codspeed thinks.

---

_@carljm approved on 2024-11-01 16:50_

Looks good to me! Some comments, but nothing blocking besides the cycle handling.

---

_@AlexWaygood reviewed on 2024-11-01 16:57_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1843 on 2024-11-01 16:57_

Hooray, Codspeed likes that version even more than what I had before!

---

_@AlexWaygood reviewed on 2024-11-01 17:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/stubs/class.md`:8 on 2024-11-01 17:19_

Yes, it still fails if it's a `.py` file!

```
    /stubs/class.md:15 unmatched assertion: revealed: Literal[Bar]
    /stubs/class.md:15 unexpected error: 1 [revealed-type] "Revealed type is `Unknown`"
    /stubs/class.md:16 unmatched assertion: revealed: tuple[Literal[Bar], Unknown, Literal[object]]
    /stubs/class.md:16 unexpected error: 1 [revealed-type] "Revealed type is `Unknown`"
```

---

_@AlexWaygood reviewed on 2024-11-01 17:41_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/mro.rs`:79 on 2024-11-01 17:41_

> This approach won't work for handling indirect cycles. I think you'll need to test if we appear anywhere in `Mro::of_base().elements()`?

Hmm, I tried that and it doesn't fix the tests that @MichaReiser suggested in https://github.com/astral-sh/ruff/pull/14027#discussion_r1825877438; we still panic on those. I think I need to do something a bit fancier (I have an idea).

The diff I tried was this:

<details>

```diff
--- a/crates/red_knot_python_semantic/src/types/mro.rs
+++ b/crates/red_knot_python_semantic/src/types/mro.rs
@@ -79,10 +79,19 @@ impl<'db> Mro<'db> {
                             };
                             Err(MroError::new(db, class, error_kind))
                         } else {
-                            let mro = std::iter::once(ClassBase::Class(class))
-                                .chain(Mro::of_base(db, base).elements())
-                                .collect();
-                            Ok(mro)
+                            let class_as_base = ClassBase::Class(class);
+                            let base_mro = Mro::of_base(db, base);
+                            if base_mro.contains(&class_as_base) {
+                                let error_kind = MroErrorKind::CyclicClassDefinition {
+                                    invalid_base_index: 0,
+                                };
+                                Err(MroError::new(db, class, error_kind))
+                            } else {
+                                let mro = std::iter::once(class_as_base)
+                                    .chain(base_mro.elements())
+                                    .collect();
+                                Ok(mro)
+                            }
                         }
                     }
                     Err(invalid_base_ty) => {
@@ -107,11 +116,21 @@ impl<'db> Mro<'db> {
                     match ClassBase::try_from_node(db, base_node, class_stmt_node, definition) {
                         Ok(valid_base) => {
                             if valid_base.into_class_literal_type() == Some(class) {
+                                let error_kind = MroErrorKind::CyclicClassDefinition {
+                                    invalid_base_index: 0,
+                                };
+                                return Err(MroError::new(db, class, error_kind));
+                            }
+
+                            let class_as_base = ClassBase::Class(class);
+                            let base_mro = Mro::of_base(db, valid_base);
+                            if base_mro.contains(&class_as_base) {
                                 let error_kind = MroErrorKind::CyclicClassDefinition {
                                     invalid_base_index: i,
                                 };
                                 return Err(MroError::new(db, class, error_kind));
                             }
+
                             valid_bases.push(valid_base);
```

</details>

---

_@AlexWaygood reviewed on 2024-11-01 17:53_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/mro.rs`:69 on 2024-11-01 17:53_

It's an interesting idea. However, Codspeed is currently neutral on this branch, so I think I'd prefer to leave this for now to keep things simple. If MRO resolution starts showing up as a bottleneck in profiling in the future, I'd be happy to try this.

---

_@carljm reviewed on 2024-11-01 17:55_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/mro.rs`:79 on 2024-11-01 17:55_

Oh, yeah, because trying to calculate the MRO of the base gets us right back to here, and we get into the infinite cycle before ever checking for our presence in the base MRO.

Looking forward to the fancier version! I suspect you'll have to keep a set of seen classes and pass it down, or similar.

---

_@AlexWaygood reviewed on 2024-11-01 17:57_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/mro.rs`:79 on 2024-11-01 17:57_

> Looking forward to the fancier version! I suspect you'll have to keep a set of seen classes and pass it down, or similar.

yeah...

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1938 on 2024-11-01 20:21_

How frequently is this function called. Is the caching worth it considering that the mro is cached? 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1951 on 2024-11-01 20:27_

What's the reason for cloning `classes_to_watch` here? Doesn't that mean that we miss cycles if they're very nested?

I would expect that we pass a `&mut Class`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:438 on 2024-11-01 20:30_

Considering that we're always calling `try_mro` here, could we instead move the `is_cyclically_defined` into `try_mro` so that it is only called once and that all MRO error handling is centralized?

---

_@MichaReiser reviewed on 2024-11-01 20:30_

---

_@AlexWaygood reviewed on 2024-11-01 20:47_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1951 on 2024-11-01 20:47_

Without the clone, we start complaining that classes that use diamond inheritance are cyclically defined, because the same class appears in the MROs of two different bases. But that doesn't necessarily mean that the class is cyclic; Python can resolve the MRO for a class that uses diamond inheritance.

> Doesn't that mean that we miss cycles if they're very nested?

I don't _think_ so. Do you have an example of something that you think this logic might miss?

---

_@MichaReiser reviewed on 2024-11-01 20:57_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1951 on 2024-11-01 20:57_

Oh interesting. I forgot about diamond inheritance chains.


I think what we could do here instead of cloning is to use an index map and instead always truncate the set when we're done visiting a subgraph to the length it had before visiting that subgraph (which is the same as dropping the entire subgraph)

---

_@AlexWaygood reviewed on 2024-11-01 21:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1951 on 2024-11-01 21:03_

Oh very cool idea! Would never have thought of that. I'll try that out, thanks!

---

_@carljm reviewed on 2024-11-01 21:44_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/stubs/class.md`:12 on 2024-11-01 21:44_

I was intrigued/confused by this TODO, because I didn't think we did silently infer `Unknown` for subscripts. And we don't! What's happening here is a different bug (authored by me!), where we silently ignore all diagnostics arising from deferred type inference (which includes class bases in a stub file.) I tracked it down and here is the fix, as a diff on this PR:

```diff
diff --git a/crates/red_knot_python_semantic/resources/mdtest/stubs/class.md b/crates/red_knot_pyt>
index cc1297499..28a6b1f5e 100644
--- a/crates/red_knot_python_semantic/resources/mdtest/stubs/class.md
+++ b/crates/red_knot_python_semantic/resources/mdtest/stubs/class.md
@@ -8,9 +8,8 @@ In type stubs, classes can reference themselves in their base class definitions.
 ```py path=a.pyi
 class Foo[T]: ...

-# TODO we resolve `Foo[Bar]` to `Unknown` here without emitting a diagnostic.
-# (Ideally we'd understand generics, but failing that, we shouldn't *silently* infer `Unknown`)
-class Bar(Foo[Bar]): ...
+# TODO subscripting generic types
+class Bar(Foo[Bar]): ...  # error: [non-subscriptable]

 reveal_type(Bar)  # revealed: Literal[Bar]
 reveal_type(Bar.__mro__)  # revealed: tuple[Literal[Bar], Unknown, Literal[object]]
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/>
index 0c55db556..8e30049ee 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -410,15 +410,18 @@ impl<'db> TypeInferenceBuilder<'db> {
         if self.types.has_deferred {
             let mut deferred_expression_types: FxHashMap<ScopedExpressionId, Type<'db>> =
                 FxHashMap::default();
+            let mut deferred_diagnostics = TypeCheckDiagnostics::new();
             // invariant: only annotations and base classes are deferred, and both of these only
             // occur within a declaration (annotated assignment, function or class definition)
             for definition in self.types.declarations.keys() {
                 if infer_definition_types(self.db, *definition).has_deferred {
                     let deferred = infer_deferred_types(self.db, *definition);
                     deferred_expression_types.extend(&deferred.expressions);
+                    deferred_diagnostics.extend(&deferred.diagnostics);
                 }
             }
             self.types.expressions.extend(deferred_expression_types);
+            self.diagnostics.extend(&deferred_diagnostics);
         }

         self.check_class_definitions();
```

But if you'd rather not include the fix in this PR, that's totally reasonable; I can do it as a follow-up PR with a dedicated test.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1951 on 2024-11-01 21:54_

Equivalent to that would be for `is_cyclically_defined_recursive` to remove `class` from `classes_to_watch` before returning. But Micha's idea to use an `IndexSet` will be more efficient.

---

_@carljm reviewed on 2024-11-01 21:54_

---

_@MichaReiser reviewed on 2024-11-01 22:11_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/stubs/class.md`:12 on 2024-11-01 22:11_

Nit: do we need the intermediate deferred types and diagnostics struct? I'm probably blind and there's an obvious borrow problem 

---

_@carljm reviewed on 2024-11-01 22:13_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/stubs/class.md`:12 on 2024-11-01 22:13_

Heh, I was just looking at this and wondering the same thing! AFAICS there is no need for it; no idea why I did it that way. Updated diff:

```diff
diff --git a/crates/red_knot_python_semantic/resources/mdtest/stubs/class.md b/crates/red_knot_pyt>
index cc1297499..28a6b1f5e 100644
--- a/crates/red_knot_python_semantic/resources/mdtest/stubs/class.md
+++ b/crates/red_knot_python_semantic/resources/mdtest/stubs/class.md
@@ -8,9 +8,8 @@ In type stubs, classes can reference themselves in their base class definitions.
 ```py path=a.pyi
 class Foo[T]: ...

-# TODO we resolve `Foo[Bar]` to `Unknown` here without emitting a diagnostic.
-# (Ideally we'd understand generics, but failing that, we shouldn't *silently* infer `Unknown`)
-class Bar(Foo[Bar]): ...
+# TODO subscripting generic types
+class Bar(Foo[Bar]): ...  # error: [non-subscriptable]

 reveal_type(Bar)  # revealed: Literal[Bar]
 reveal_type(Bar.__mro__)  # revealed: tuple[Literal[Bar], Unknown, Literal[object]]
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/>
index 0c55db556..4ca286be0 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -408,17 +408,15 @@ impl<'db> TypeInferenceBuilder<'db> {
         }

         if self.types.has_deferred {
-            let mut deferred_expression_types: FxHashMap<ScopedExpressionId, Type<'db>> =
-                FxHashMap::default();
             // invariant: only annotations and base classes are deferred, and both of these only
             // occur within a declaration (annotated assignment, function or class definition)
             for definition in self.types.declarations.keys() {
                 if infer_definition_types(self.db, *definition).has_deferred {
                     let deferred = infer_deferred_types(self.db, *definition);
-                    deferred_expression_types.extend(&deferred.expressions);
+                    self.types.expressions.extend(&deferred.expressions);
+                    self.diagnostics.extend(&deferred.diagnostics);
                 }
             }
-            self.types.expressions.extend(deferred_expression_types);
         }

         self.check_class_definitions();
```

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-03 18:38_

---

_@AlexWaygood reviewed on 2024-11-03 18:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/mro.md`:403 on 2024-11-03 18:43_

I'm pretty unhappy about the cascading errors here. I might work on a followup PR to see if I can avoid them.

Unfortunately a solution doesn't seem trivial (at least from what I can see). Also, this situation should really be very rare (I don't see any plausible reason why anybody would try to make a class inherit from itself, even indirectly, in real code). So I think this is _okay_, if necessary.

---

_@MichaReiser reviewed on 2024-11-04 07:10_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/mro.md`:403 on 2024-11-04 07:10_

I don't think there's a reason why they would **want** to do that but I remember that I at least accidentally did just that more than once in my early career because I was unaware that it leads to a cycle. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1876 on 2024-11-04 07:32_

Nit: It wasn't clear to me why the `[()]` was necessary and naively removed it to see if it still compiles.... and it does. But what I understand is that doing so removes the performance optimization. 

I suggest moving this code into a custom `Iterator` implementation to make the lazy lookup more explicit. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1855 on 2024-11-04 07:33_

Nit: Maybe `mro_iter` to make it more explicit that this method iterates over the `MRO` and is not a non-failable alternative to `mro`.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1881 on 2024-11-04 07:34_

Can we use `contains(ClassBase::Class(other))`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1862 on 2024-11-04 07:38_

While it's true that we don't emit diagnostics for those, we still call `check_class_definitions` for every `ClassDefinition` which results in eagerly pulling the class's MRO. 

That's why I don't think the optimization is really about avoiding computing the MRO. It's only about avoiding making the salsa query call.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:132 on 2024-11-04 07:40_

```suggestion
                seqs.push(valid_bases.into_iter().collect());
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/mro.rs`:418 on 2024-11-04 07:41_

Nice use of `Either` :)

---

_@MichaReiser approved on 2024-11-04 07:41_

---

_Comment by @MichaReiser on 2024-11-04 07:41_

This is great! 

---

_@AlexWaygood reviewed on 2024-11-04 11:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/mro.rs`:132 on 2024-11-04 11:32_

That doesn't work because we need access to `valid_bases` in the closure passed to `.ok_or_else()` immediately below

---

_Comment by @AlexWaygood on 2024-11-04 13:27_

Thanks both for the excellent review comments!

---

_Merged by @AlexWaygood on 2024-11-04 13:31_

---

_Closed by @AlexWaygood on 2024-11-04 13:31_

---

_Branch deleted on 2024-11-04 13:31_

---

_Comment by @carljm on 2024-11-04 20:11_

🎉 

---

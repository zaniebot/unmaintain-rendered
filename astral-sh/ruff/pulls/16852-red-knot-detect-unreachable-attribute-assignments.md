```yaml
number: 16852
title: "[red-knot] detect unreachable attribute assignments"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: unreachable-attribute-assignments
created_at: 2025-03-19T17:22:42Z
updated_at: 2025-04-14T12:57:51Z
url: https://github.com/astral-sh/ruff/pull/16852
synced_at: 2026-01-12T15:55:59Z
```

# [red-knot] detect unreachable attribute assignments

---

_@mtshiba_

## Summary

This PR closes astral-sh/ruff#15967.

Attribute assignments that are statically known to be unreachable are excluded from consideration for implicit instance attribute type inference. If none of the assignments are found to be reachable, an `unresolved-attribute` error is reported.

## Test Plan

[A test case](https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/resources/mdtest/attributes.md#attributes-defined-in-statically-known-to-be-false-branches) marked as TODO now work as intended, and new test cases have been added.


---

_Review requested from @carljm by @mtshiba on 2025-03-19 17:22_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-03-19 17:22_

---

_Review requested from @sharkdp by @mtshiba on 2025-03-19 17:22_

---

_Review requested from @dcreager by @mtshiba on 2025-03-19 17:22_

---

_Comment by @github-actions[bot] on 2025-03-19 17:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `red-knot` added by @AlexWaygood on 2025-03-19 17:42_

---

_@MichaReiser reviewed on 2025-03-20 08:23_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:149 on 2025-03-20 08:23_

I'm trying to understand the need for this new field. It seems that we need the symbol for every `AttributeAssignment`. Would it be possible to store the symbol as part of the `attribute_assignments` state instead. Or are there any downsides to it that I'm overlooking?

---

_@carljm reviewed on 2025-03-21 00:05_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:149 on 2025-03-21 00:05_

I think it goes deeper than this. If we see an attribute assignment like `self.x = "foo"` in `__init__` method, the only symbol in the scope of `__init__` that is involved here is `self`. What we are newly tracking in this PR is the "symbol namespace" of "names assigned to `self` in methods". So it's not just tracking some existing symbol ID, we are creating a whole new symbol table for this new namespace, that before was not tracked at all. And we also add support to the use-def map so that we can track conditional visibility of these names as well.

It's a clever and interesting approach, which makes good use of the existing symbol-table and control-flow infrastructure we have. It's also interesting because it could likely generalize well to future attribute-type-narrowing support.

Off the top of my head, I also wonder whether we need both the `attribute_assignments` map and the new `instance_attribute_tables`. It seems like the latter should be able to replace the former entirely, as we can just query the symbol table and the use-def map to get all assigned names?

---

_@carljm reviewed on 2025-03-21 00:06_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:149 on 2025-03-21 00:06_

Would definitely be interested in @sharkdp's thoughts on this PR, as the author of the `instance_attributes` work.

---

_@carljm reviewed on 2025-03-21 00:27_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:149 on 2025-03-21 00:27_

My main "a priori" concern with this PR would have been the cost of adding `instance_attribute_tables` to every scope, even though we only use it in some scopes. But codspeed says no regression; I guess the cost of an empty symbol table and empty extra set of symbol states in the use-def map is not that high?

---

_@mtshiba reviewed on 2025-03-21 03:09_

---

_Review comment by @mtshiba on `crates/red_knot_python_semantic/src/semantic_index.rs`:149 on 2025-03-21 03:09_

Yes, I thought that a new field were needed to track the visibility of instance attributes, but we can actually divert some existing structure. That is, we can add an empty definition to `UseDefMapBuilder::all_definitions` to make it publish a `ScopedDefinitionID`, which will correspond to the instance attribute assignment.
Since `self.x = 1` and `x = 1` are different things, we need to have them as different symbol states from `symbol_states/public_symbols`, and in addition, the (reachable) instance attributes must remain visible after the method is returned since they are accessible from the outside.

As you say, the essence of this PR is the addition of states to manage instance attributes.
It was not needed in this PR, but if we want to do more analysis for instance attributes in the future, we may need to add a new variant representing attribute assignment to `DefinitionKind` and make a `Definition` instead of adding an empty definition to `all_definitions`.

---

_@sharkdp reviewed on 2025-03-24 16:02_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:416 on 2025-03-24 16:02_

This is an improvement that we should probably add a test for?

```py
class C:
    def __init__(self):
        self.x = 1
        self.x = "a"

reveal_type(C().x)
```

Previously, we would infer `Unknown | Literal[1, 2]` in that case, and now we infer the more correct `Unknown | Literal[2]`.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/class.rs`:515 on 2025-03-24 16:15_

The changes in this PR would probably also allow us to add a more fine-granular possibly-unbound handling for instance attributes. In a case like this …

```py
class C:
    def f(self, cond: bool):
        if cond:
            self.x = 1

C().x
```

… we could use the full return type of `is_attribute_assignment_visible` to see that `x` is not definitely bound.

Boundness of implicit instance instance attributes is treated a bit inconsistently at the moment. We treat them as always possibly-unbound in some situations (e.g. in the descriptor protocol implementation), since we don't (attempt to) understand if that method `f` has been called or not.

But on the other hand, we do not emit a possibly-unbound-attribute diagnostic in a case like above, because `f` *could* have been called. So we don't set an explicit `Boundness::PossiblyUnboud` state for implicit instance attributes, as that would lead to many false positive diagnostics.

I'm not sure if there is much value in adding a more sophisticated understanding, but it seems reasonable to emit a possibly-unbound-attribute diagnostic in the example above. Because even if `f` is called, `x` is still possibly-unbound.

---

_@sharkdp reviewed on 2025-03-24 16:15_

---

_@sharkdp reviewed on 2025-03-24 16:21_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index.rs`:149 on 2025-03-24 16:21_

> Would definitely be interested in @sharkdp's thoughts on this PR, as the author of the instance_attributes work.

I also think this looks great, especially because it doesn't just solve the unreachable-attribute assignment case, but also allows us to distinguish between those two cases:
```py
self.x = 1
self.x = 2
```
and
```py
if cond:
    self.x = 1
else:
    self.x = 2
```


> Off the top of my head, I also wonder whether we need both the `attribute_assignments` map and the new `instance_attribute_tables`. It seems like the latter should be able to replace the former entirely, as we can just query the symbol table and the use-def map to get all assigned names?

I agree. I would be great if we could attempt to make that change in this PR.

---

_Comment by @mtshiba on 2025-03-27 04:18_

While trying to solve the issue, I noticed that the following code does not pass:

```python
class C:
    def __init__(self):
        def closure():
            self.a: str = 1
        closure()
        [... for self.b in range(1)]
        class D:
            self.c = 1

# should be OK
C().a
C().b
C().c
```

It appears that the `SemanticIndexBuilder::register_attribute_assignment` does not take inner scopes into account.
I'm trying to fix this, but this may be an issue that should be resolved in another PR.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-03-27 21:19_

---

_Review requested from @dhruvmanila by @mtshiba on 2025-03-30 19:18_

---

_Comment by @codspeed-hq[bot] on 2025-03-30 19:26_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Aunreachable-attribute-assignments)

### Merging astral-sh/ruff#16852 will **not alter performance**

<sub>Comparing <code>mtshiba:unreachable-attribute-assignments</code> (e52f9d6) with <code>main</code> (3aa3ee8)</sub>



### Summary

`✅ 32` untouched benchmarks  





---

_Comment by @mtshiba on 2025-03-30 19:30_

The commit I just made completely removes `SemanticIndex::attribute_assignments/AttributeAssignment`.
Attribute assignments are now stored in `SemanticIndex::all_definitions`.

I noticed that we can also track attribute assignments in comprehension/named/augmented assignments as well, so I will be working on that.


---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:195 on 2025-03-31 07:42_

The main motivation for using an `Arc` for the `SymbolTable` was so that we could return them from a salsa query that is specific per scope (instead of the entire `semantic_index`). However, it looks like no such query exists any more, so we should remove the `Arc` wrapper

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/class.rs`:39 on 2025-03-31 07:46_

What's the reason for moving this function out of `Class`? It makes the diff harder to review

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/class.rs`:38 on 2025-03-31 07:50_

Have you investigated where the performance regression is coming from? 

I suspect that it is because this function is now a salsa query and its arguments are somewhat expensive (requires hashing and allocating a name). Gating the `attribute_assignments` and `is_attribute_assignment_visible` methods behind a query is important because they access `semantic_index` or `node`. However, it makes me wonder if we could change the methods so that they wouldn't have to depend on the entire `semantic_index` or access specific nodes. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/class.rs`:42 on 2025-03-31 07:50_

```suggestion
    name: Name, // tracked functions do not accept a borrowed `str` with an implicit lifetime
```

---

_@MichaReiser reviewed on 2025-03-31 07:50_

---

_@mtshiba reviewed on 2025-03-31 17:18_

---

_Review comment by @mtshiba on `crates/red_knot_python_semantic/src/types/class.rs`:38 on 2025-03-31 17:18_

Thanks. Initially I thought the performance regression was caused by removing `salsa::tracked` of `attribute_assignments`, but changing the field types of `{Assignment, ForStmt, WithItem}DefinitionKind` may be the true cause (there seems to be a regression with type inference on these).

---

_@sharkdp reviewed on 2025-04-01 07:44_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/class.rs`:515 on 2025-04-01 07:44_

It looks like this leads to many new diagnostics in the ecosystem checks. Did you have a chance to investigate this?

---

_@mtshiba reviewed on 2025-04-01 17:25_

---

_Review comment by @mtshiba on `crates/red_knot_python_semantic/src/types/class.rs`:515 on 2025-04-01 17:25_

I found that the following code would result in a `possibly-unbound-attribute` warning.

* case 1: may loop forever in `__init__`

```python
from typing import Iterable

class Foo:
    def __init__(self, iterable: Iterable[int], cond: bool) -> None:
        self.x = 1

        # These loops may never stop
        for _ in iterable:
            pass
        while cond:
            pass

foo = Foo([], False)
# error: [possibly-unbound-attribute]
foo.x
```

* case 2: may throw an error in `__init__`

```python
class Foo:
    def __init__(self, b: bytes, cond: bool) -> None:
        try:
            s = b.decode()
        except UnicodeDecodeError:
            raise ValueError("Invalid UTF-8 sequence")
        if cond:
            raise ValueError("Something went wrong")

        self.s = s

foo = Foo(b"abc")
# error: [possibly-unbound-attribute]
foo.s
```

These are false positive warnings; when creating a `SemanticIndex`, it would be necessary to treat the recording of visibility for attribute assignments differently than for name assignments.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:341 on 2025-04-02 23:52_

If we keep the possibly-unbound-attribute diagnostic, I think it makes just as much sense for this attribute to be marked as possibly-unbound as it does in the `if flag:` case with `c_instance.possibly_undeclared_unbound` above. The iterable here might be empty. (We could add a comment here explaining that "iterable might be empty" is the reason the attribute might not be defined.)

That is,  I think either this case and the above `if flag:` case should both emit a diagnostic, or neither of them should.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:345 on 2025-04-02 23:53_

Same as above: _if_ we are emitting `possibly-unbound-attribute` for cases where the only attribute definition we see is conditional, then we _should_ emit it here IMO; `for` is just as conditional as `if`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:432 on 2025-04-02 23:53_

If we keep this behavior, then the text above on lines 415-416 would need to be updated.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:115 on 2025-04-03 00:00_

This logic is already implemented as `ChildrenIter`, should we just use that instead of `DescendantsIter`?

(There's no efficiency improvement, the implementation of `ChildrenIter` is effectively the same, it just avoids re-implementing the logic.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:102 on 2025-04-03 00:03_

We should probably mention in the doc comment for this function that calling it adopts a direct dependency on the AST of the file containing `class_body_scope`, since we access the semantic index for that file.
```suggestion
///
/// Only call this when doing type inference on the same file as `class_body_scope`, otherwise it
/// introduces a direct dependency on that file's AST.
pub(crate) fn attribute_assignments<'db, 's>(
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1917 on 2025-04-03 00:20_

In this PR, `TargetKind` still has only two variants, `Name` and `Sequence`, but now we end up using `Name` also when the target is an attribute. This should be fixed in some way, since it's misleading.

It seems maybe we don't ever need to distinguish between name target and attribute target using the `TargetKind`? In which case we could rename the `Name` variant to `NameOrAttribute`. Or we could add a third variant, `Attribute`, and make sure we pass `Attribute` variant here. The latter seems nicer in that we preserve more information, but if we don't need that information maybe it's not worth it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:288 on 2025-04-03 00:23_

Does this PR introduce `None` entries elsewhere in the array? If not, we can revert this change?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:325 on 2025-04-03 00:34_

nit: we could just call this `instance_attributes`.

We don't call `public_symbols` `public_symbol_states`, even though it is also an indexvec of `SymbolState`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:362 on 2025-04-03 00:41_

Just for naming consistency I suppose we could call this `instance_attribute_bindings`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:417 on 2025-04-03 00:44_

Do we have any tests for an attribute assignment inside a definitely-not-existing method? I.e. do any tests fail if we remove this `is_method_visible` check?

---

_@carljm reviewed on 2025-04-03 00:46_

Haven't finished my review but I have to head out, so submitting the comments I have so far. Thanks for your work on this!

Given the false positives observed, I'm not sure we should keep the possibly-unbound-attribute checks, but I'm still thinking about whether there is a satisfactory way to address the false positive cases. It may have some overlap with https://github.com/astral-sh/ruff/issues/15777

---

_@mtshiba reviewed on 2025-04-03 06:05_

---

_Review comment by @mtshiba on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:417 on 2025-04-03 06:05_

I added a test to check the visibility of an attribute defined in a definitely-non-existing-method.

---

_Comment by @mtshiba on 2025-04-04 06:38_

Support for attribute assignments in comprehension/aug-assign/~~named-expression~~ is incomplete, but considering that it is still not implemented in the main branch and that it was not originally planned in this PR, this may be work that should be done in another PR.

I believe that the implementation of the feature originally planned in this PR is almost complete and ready for final review.

---

_Review requested from @carljm by @mtshiba on 2025-04-04 06:38_

---

_Review requested from @MichaReiser by @mtshiba on 2025-04-04 06:39_

---

_Review requested from @sharkdp by @mtshiba on 2025-04-04 06:39_

---

_Review requested from @BurntSushi by @mtshiba on 2025-04-11 16:16_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:882 on 2025-04-12 02:26_

Can't we just get this once at the top of the function? It won't ever change.

---

_@carljm approved on 2025-04-12 02:38_

This looks pretty good to me! It turns `ClassLiteralType::implicit_instance_attribute` into even more of a monster method, but I think I'm ok with that for now: it has a clear responsibility, and the logic in it is well encapsulated; if we need to break it up for better reuse in future, we can do that.

@sharkdp if you have a chance, I'd like to make sure this looks good to you as well.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:598 on 2025-04-14 06:49_

This seems to be a small extension of the existing "Possibly unbound" test case below. Maybe we can move it down.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/class.rs`:928 on 2025-04-14 07:11_

Elegant!

---

_@sharkdp approved on 2025-04-14 07:13_

This looks great — thank you very much. I have just one minor comment, which I can also resolve myself before merging this.

---

_Merged by @sharkdp on 2025-04-14 07:23_

---

_Closed by @sharkdp on 2025-04-14 07:23_

---

_Branch deleted on 2025-04-14 12:57_

---

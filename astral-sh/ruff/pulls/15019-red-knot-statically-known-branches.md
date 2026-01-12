```yaml
number: 15019
title: "[red-knot] Statically known branches"
type: pull_request
state: merged
author: sharkdp
labels:
  - great writeup
  - ty
assignees: []
merged: true
base: main
head: david/statically-known-branches-2
created_at: 2024-12-16T12:33:12Z
updated_at: 2024-12-21T13:36:37Z
url: https://github.com/astral-sh/ruff/pull/15019
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] Statically known branches

---

_@sharkdp_

## Summary

Rendered version of the test suite including a proper introduction to the topic / motivation: [**click**](https://github.com/astral-sh/ruff/blob/david/statically-known-branches-2/crates/red_knot_python_semantic/resources/mdtest/statically_known_branches.md).

This changeset adds support for precise type-inference and boundness-handling of definitions inside control-flow branches with statically-known conditions, i.e. test-expressions whose truthiness we can unambiguously infer as *always false* or *always true*. In code:

```py
x = 1

if "z" in "haystack":  # Literal[False]
    x = 2

reveal_type(x)  # revealed: Literal[1]
```

and:

```py
x = 1

if "y" in "haystack":  # Literal[True]
    x = 2

reveal_type(x)  # revealed: Literal[2]
```

## Implementation

### Visibility constraints, truthiness, negation

One option to implement this would have been to add special handling for a limited set of test-expressions in semantic index-building. We would then analyze expressions like `sys.version_info >= (x, y)`, `typing.TYPE_CHECKING`, `True` and `False` without any type inference and consequently close down (or unconditionally open) branches whose truthiness we can analyze in this way. This would simplify the implementation, but is much less general than the approach taken here.

Instead, we collect all necessary information during semantic index building, and then re-analyze the control flow during type-checking. The way this works is by recording so-called visibility constraints for each binding and declaration. Note that these constraints are in some sense similar to narrowing constraints, but also work quite differently and are applied at different points in control flow. Consider the following example first. Note how visibility constraints can apply to bindings outside of the if-statement:
```py
x = 1  # visibility constraint: ~test
if test:
    x = 2  # visibility constraint: test

    y = 2  # visibility constraint: test

use(x)
use(y)
```
The static truthiness of the `test` condition can either be always-false, ambiguous, or always-true. Similarly, if the visibility constraint of a binding evaluates to always-true/always-false, it will be either always visible or never visible. If the truthiness of the constraint is ambiguous, we need to consider both options (the binding could be visible or not). For the example above, this would result in the following type inference / boundness results for the uses of `x` and `y`:

| `test` truthiness | `~test` truthiness | type of `x`     | boundness of `y` |
|-------------------|-----------------|------------------|------------------|
| always false      | always true | `Literal[1]`    | unbound          |
| ambigous          | ambigous | `Literal[1, 2]` | possibly unbound |
| always true       | always false | `Literal[2]`    | bound            |

### Sequential constraints

Next, let's consider a sequence of multiple control flow elements:
```py
x = 0

if test1:  
    x = 1

if test2:
    x = 2
```
The binding `x = 2` is easy to analyze. Its visibility correponds to the truthiness of `test2`. For the `x = 1` binding, things are a bit more interesting. It is always visible if `test1` is always-true *AND* `test2` is always-false. It is never visible if `test1` is always-false *OR* `test2` is always-true. Note the asymmetry in the logical operators here. We introduce a new constraint [`KleeneAnd(a, b)`](https://en.wikipedia.org/wiki/Three-valued_logic#Kleene_and_Priest_logics), which is always-true if both `a` and `b` are always-true, always-false if either `a` or `b` are always-false, and ambiguous otherwise. Then we can formulate the constraint for the `x = 1` binding as `KleeneAnd(test1, ~test2)`.

The `x = 0` binding can be handled similarly, with the difference that both `test1` and `test2` are negated:
```py
x = 0  # KleeneAnd(~test1, ~test2)

if test1:  
    x = 1  # KleeneAnd(test1, ~test2)

if test2:
    x = 2  # test2
```

### Merged (or parallel) constraints

Finally, let's consider this example of "parallel" control flow (where we have omitted the test condition for the outer control flow element, as it would only complicate the discussion; instead of `if <…>` you can also consider any other control-flow splitting element where we have no static analysis of which branch is taken)
```py
x = 0

if <…>:
    if test1:
        x = 1
else:
    if test2:
        x = 2

use(x)
```
At the usage of `x`, i.e. after the control flow has been merged again, the visibility of the `x = 0` binding behaves as follows: the binding is always visible if `test1` is always-false *OR* `test2` is always-false; and it is never visible if `test1` is always-true *AND* `test2` is always-true. Again, note the asymmetry. We introduce a new constraint [`KleeneOr(a, b)`](https://en.wikipedia.org/wiki/Three-valued_logic#Kleene_and_Priest_logics) which is always-true if `a` is always-true OR `b` is always true; and always-false if `a` is always-false AND `b` is always-false. This allows us to annotate the bindings with the following constraints:

```py
x = 0  # KleeneOr(~test1, ~test2)

if <…>:
    if test1:
        x = 1  # test1
else:
    if test2:
        x = 2  # test2

use(x)
```

### Properties

We note that `KleeneAnd` and `KleeneOr` have the property that `KleeneOr(~a, ~b) = ~KleeneAnd(a, b)`. This means we can, in principle, get rid of either of these two to simplify the representation.

However, we already apply negative constraints `~test1` and `~test2` to the "branches not taken" in the example above. This means that the tree-representation `KleeneOr(~test1, ~test2)` is much cheaper/shallower than basically creating `~KleeneAnd(~(~test1), ~(~test2))`. Similarly, if we wanted to get rid of `KleeneAnd`, we would also have to create additional nodes. So for performance reasons, there is a certain "duplication" in the code between those two constraint types.

## Other

This branch also includes:
- `sys.platform` support
- statically-known branches handling for Boolean expressions and while loops
- new `target-version` requirements in some Markdown tests which were now required due to the understanding of `sys.version_info` branches.


closes #12700 
closes #15034 

## Performance

### `tomllib`, -7%, needs to resolve one additional module (sys)

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `./red_knot_main --project /home/shark/tomllib` | 22.2 ± 1.3 | 19.1 | 25.6 | 1.00 |
| `./red_knot_feature --project /home/shark/tomllib` | 23.8 ± 1.6 | 20.8 | 28.6 | 1.07 ± 0.09 |

### `black`, -6%

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `./red_knot_main --project /home/shark/black` | 129.3 ± 5.1 | 119.0 | 137.8 | 1.00 |
| `./red_knot_feature --project /home/shark/black` | 136.5 ± 6.8 | 123.8 | 147.5 | 1.06 ± 0.07 |

## Test Plan

- New Markdown tests for the main feature in `statically-known-branches.md`
- New Markdown tests for `sys.platform`
- Adapted tests for `EllipsisType`, `Never`, etc

---

_Label `red-knot` added by @sharkdp on 2024-12-16 12:33_

---

_Comment by @codspeed-hq[bot] on 2024-12-17 09:39_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fstatically-known-branches-2)

### Merging #15019 will **degrade performances by 17.15%**

<sub>Comparing <code>david/statically-known-branches-2</code> (95d079c) with <code>main</code> (d3f51cf)</sub>



### Summary

`❌ 1 (👁 1)` regressions  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `main` | `david/statically-known-branches-2` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `red_knot_check_file[cold]` | 69.5 ms | 83.9 ms | -17.15% |


---

_Comment by @github-actions[bot] on 2024-12-17 19:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `great writeup` added by @AlexWaygood on 2024-12-18 18:42_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/statically_known_branches.md`:838 on 2024-12-18 20:04_

What is the nature of the limitation here? (That is, what would be required to infer the more precise type?)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/statically_known_branches.md`:1 on 2024-12-18 20:14_

These tests are so fantastic and exhaustive that I hesitate to suggest any addition, but it does strike me that we thoroughly test our control-flow support, but not so much our recognition of expression truthiness when determining statically known branches. I think these tests could (almost?) all be made to pass if we just did simple pattern-recognition of `True`, `False`, the `sys` cases, and maybe a few cases involving immediate literals.

Maybe would be worth adding at least one test explicitly showing that if we have e.g. a custom class with `def __bool__(self) -> Literal[True]: ...`, even imported from a different file, an instance of that class still counts as a statically-known test condition. This shows that we use the full power of type inference, we don't just match on a few hard-coded cases.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:334 on 2024-12-18 20:32_

```suggestion
        self.current_use_def_map_mut().record_constraint(negated)
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:835 on 2024-12-18 20:35_

Why push the ID also to this vector, when it seems like the only time we iterate over it, we ignore the ID?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:29 on 2024-12-18 20:42_

The intent of the previous design was that the `ScopedConstraintId` would stay local to `UseDefMap[Builder]`, not be exposed outside it, since we have so many ID kinds already (`Constraint<'db>` is also an ID!), and it's supposed to just be an index into the internal IndexVec. It may be that's not feasible with the APIs we need, I haven't fully processed all that yet; just dropping this thought here so I don't forget it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:39 on 2024-12-18 21:48_

Should this name and `add_sequence` below be updated to match the enum variant naming? Seems like we should probably choose one naming scheme and stick with it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:224 on 2024-12-18 21:57_

This should be symmetric, no? If `b` is always true, we can simplify to `a`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:69 on 2024-12-18 22:03_

```suggestion
        &self,
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:78 on 2024-12-18 22:03_

```suggestion
        &self,
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:26 on 2024-12-18 22:14_

It would be interesting to see the performance impact if we made a `VisibilityConstraint` a tracked struct, and analyzing it a query.

It would increase our ingredient count, which would probably hurt incremental performance, but it would mean we don't have to repeatedly re-analyze the same binary expression tree, which I assume is most of what causes the exponential behavior analyzing Black (I'm judging here from the fact that getting truthiness of the leaf nodes is already Salsa-cached, and you only enforce max recursion depth on analysis.) There's a fair amount of work happening in `analyze{_impl,_single}` even in the case where all the `infer_expression_types` are already cached, and we may currently repeat that work many times for the same constraints.

Not sure if I would prioritize this experiment before or after experimenting with the impact of using a TDD, though. Seeing that the recursion depth is applied only to evaluation (and thus reducing evaluation cost is what's needed to address the Black regression) also makes me optimistic about the impact of a representation that is better simplified and more efficient to evaluate.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:68 on 2024-12-18 22:21_

Naming nit: I would go for `evaluate` or `eval` over `analyze` for this. "Analyze" sounds to me like we are deriving some further complex information or representation; "evaluate" sounds like rendering it to a single concrete truthiness value, given concrete inputs.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:121 on 2024-12-18 22:37_

Two naming nits here: one is consistency on "Constraint" vs "Constraints", and the other is consistency on binding vs declaration vs definition (where "definition" encompasses both binding and declaration).

Narrowing constraints apply only to bindings, not declarations. Visibility constraints apply to both.

```suggestion
type VisibilityConstraintsPerDefinition = SmallVec<InlineVisibilityConstraintsArray>;
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:225 on 2024-12-18 22:56_

We seem to be a little inconsistent about whether wrapped iterators use the `_iter` suffix.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:249 on 2024-12-18 22:59_

really very nitpicky: let's keep consistent usage of defined vs bound vs declared:
```suggestion
    pub(super) fn undefined(undefined_visibility: ScopedVisibilityConstraintId) -> Self {
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:279 on 2024-12-18 23:06_

So I take it this is an optimization for the case where we've passed through a conditional branch point and re-merged control flow, and there were no definitions/declarations in either conditional branch? So this just restores the prior visibility state and avoids having extraneous merged visibility constraints on all our definitions that ultimately will have no effect?

Is it the case that if we had perfect simplification of the binary expression trees, this optimization would be unnecessary?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:376 on 2024-12-18 23:12_

This documentation is slightly out of date now, regarding unbound. (Probably you would have addressed this anyway when doing the documentation below.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:499 on 2024-12-18 23:17_

update

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:511 on 2024-12-18 23:21_

I'm not sure why this needs to be added here rather than added at the use-def map level

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:252 on 2024-12-18 23:24_

seems to use an outdated name

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:331 on 2024-12-18 23:26_

Why does the inner iterator need these references? It doesn't seem to use them in its own `next` method, that I could see.

I see they are used in type inference, but they will be the same for every `BindingWithConstraints`; couldn't they could be accessed directly off the `BindingWithConstraintsIterator` as attributes (or directly off the use-def map?), rather than making every `BindingWithConstraints` two words larger?

Or alternatively, not sure if `BindingWithConstraintsIterator::next` could itself encapsulate more of the logic that is currently in `bindings_ty`, and not require direct access to these IndexVecs in type inference. (Hmm, maybe this last is not a good idea, if it means that iterating over `BindingWithConstraintsIterator` would then require a db and doing type inference of visibility constraints.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:341 on 2024-12-18 23:26_

Same question here, why does the inner iterator need these?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:257 on 2024-12-18 23:27_

We should probably update this massive doc comment to discuss visibility constraints also. If that feels daunting I could also maybe do it?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:452 on 2024-12-18 23:29_

outdated name?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:457 on 2024-12-18 23:35_

Hmm, not sure what to best call this. I don't like using "unbound" because that implies its specific to bindings, not declarations, which is not true. But `undefined_visibility` reads a little oddly. We could literally just call it `scope_start_visibility`?

```suggestion
    unbound_visibility: ScopedVisibilityConstraintId,
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:18 on 2024-12-18 23:41_

This could hold a `Constraint<'db>` instead of a `ScopedConstraintId`? Both are just `u32` IDs, but it would remove one lookup (and the need for access to `AllConstraints`) when analyzing, and it might remove the need for the awkward `(constraint_id, constraint)` return type stuff in semantic index builder?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:351 on 2024-12-18 23:52_

Given what this actually does (nothing at all if there have been new bindings/declarations seen since the snapshot) perhaps it should be `maybe_reset_visibility_constraints`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1408 on 2024-12-18 23:54_

I'm guessing addressing the TODOs in the tests would involve adding an additional negated visibility constraint somewhere in here? Is it worth a TODO here as well, if there's any useful context you can record about how to do it / why it's hard?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:360 on 2024-12-18 23:56_

It would be possible, in the case where `constraint` is a `VisibleIf` constraint, to avoid the wrapping with `VisibleIfNot` and instead just record a `VisibleIf` around a new `Constraint` that's a copy of `constraint` with the sign flag flipped. Not sure if this would be a net win due to less deep visibility constraint trees, or a loss due to more Constraint ingredients.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:95 on 2024-12-18 23:59_

`Boundness::Bound` seems wrong here; is this just to avoid scope creep in this PR? If so, maybe a TODO comment?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:96 on 2024-12-19 00:01_

```suggestion
                // Symbol is possibly undeclared and definitely bound
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:97 on 2024-12-19 00:02_

I thought the previous behavior would have been to union declared and inferred type in this case as well as the below case? That seems like what we should do. Do we not have any test coverage of this case, or am I wrong about what the previous behavior did?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:109 on 2024-12-19 00:06_

This comment wasn't previously a TODO, and I don't think it should be a TODO. The relevant comment here is

> Intentionally ignore conflicting declared types; that's not our problem, it's the problem of the module we are importing from.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:286 on 2024-12-19 00:17_

This seems like it would be more of a debug assertion / parity check than a behavior change, since "ambiguous" should always mean "there is at least one other visible binding" and "always-true" should mean "there are no other visible bindings".

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:85 on 2024-12-19 00:19_

This could potentially claw back some perf?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:272 on 2024-12-19 00:21_

It seems like we shouldn't have to collect these into a vec? Can we make it a lazy map all the way through to the final union?

If not, then using a smallvec here might win back noticeable perf, since usually there should be few visible bindings.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:342 on 2024-12-19 00:21_

Maybe a debug assertion here that `is_unbound_visible`?

(If we tracked always-true visible vs ambiguous-visible, I think it should be always-true visible here?)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:347 on 2024-12-19 00:26_

Hmm, I see that by using `Symbol` here, which includes "Boundness", we give up on distinguishing boundness (applies to bindings) from declaredness (applies to declarations). Maybe that's fine as temporary state in this PR, something to consider as follow up, along with the whole issue of declaredness vs boundness.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:359 on 2024-12-19 00:26_

Same question here: do we have to collect? If so maybe smallvec?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:358 on 2024-12-19 00:28_

nit: `is_undeclared_visible`?

---

_@carljm reviewed on 2024-12-19 00:33_

This is really superb!! Very happy with how it turned out. (Modulo the arbitrary recursion limit, but I'm hopeful we'll be able to bump that up.)

Probably more review than you were looking for at this stage, sorry. Just not sure where to draw the line, once I got started :)

---

_@sharkdp reviewed on 2024-12-19 09:53_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/statically_known_branches.md`:838 on 2024-12-19 09:53_

I solved this now.

---

_@sharkdp reviewed on 2024-12-19 09:58_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:224 on 2024-12-19 09:58_

I thought about it. The way we construct the `And` constraints right now, this can never happen. But we should probably add the other path as well, in order not to confuse readers. It's not like that would be much slower, if at all (with branch prediction).

---

_@sharkdp reviewed on 2024-12-19 12:05_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:121 on 2024-12-19 12:05_

> Two naming nits here: one is consistency on "Constraint" vs "Constraints",

I chose `Constraint` (singular) deliberately. There is just one visibility constraint per definition. It can be a more complex constraint like `a AND b`, but it's always just one `VisibilityConstraint`. This is different from narrowing constraints, where we can have a whole set of constraints (one `Constraints` bitset) per definition.

I guess you could argue that the tree of visibility constraints also includes multiple constraints, but then we should rename `VisibilityConstraint` to `VisibilityConstraints`, which seems a bit weird to me.

> and the other is consistency on binding vs declaration vs definition (where "definition" encompasses both binding and declaration).

I didn't do that because we would then have `ConstraintsPerBinding` and `VisibilityConstraintPerDefinition` in `SymbolBindings`. I also thought about changing both to `XyzPerDefinition`, but then the (already quite hard to understand) `SymbolBindings` struct becomes less readable, as it has a `live_bindings` field, not a `live_definitions` field.

I went with a different solution now and simply introduced the same type with two different names:
```
type VisibilityConstraintPerDeclaration = SmallVec<InlineVisibilityConstraintsArray>;
type VisibilityConstraintPerBinding = SmallVec<InlineVisibilityConstraintsArray>;
```
I then consistely use the first in `SymbolDeclarations` and the second in `SymbolBindings`. I feel like this makes things a bit more clear, but let me know if you prefer your variant.

---

_@sharkdp reviewed on 2024-12-19 12:30_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:331 on 2024-12-19 12:30_

> I see they are used in type inference, but they will be the same for every `BindingWithConstraints`; couldn't they could be accessed directly off the `BindingWithConstraintsIterator` as attributes (or directly off the use-def map?), rather than making every `BindingWithConstraints` two words larger?

Yes — thanks! I was about to clean this all up (it was in this state from a previous iteration), but I'm not sure if I would have spotted this. Changed it now.

> Or alternatively, not sure if `BindingWithConstraintsIterator::next` could itself encapsulate more of the logic that is currently in `bindings_ty`, and not require direct access to these IndexVecs in type inference. (Hmm, maybe this last is not a good idea, if it means that iterating over `BindingWithConstraintsIterator` would then require a db and doing type inference of visibility constraints.)

I think this might be similar to what I had in my first iteration. I basically had `VisibilityConstraintRef`erences and they would be transformed into `VisibilityConstraint`s in the `next` method. The `VisibilityConstraint`s then didn't contain any references to the `IndexMap` anymore, but this required the tree nodes to be boxed, which was far too slow.

Let me know if you had something else in mind.

---

_@sharkdp reviewed on 2024-12-19 12:35_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:257 on 2024-12-19 12:35_

I was planning to do that in the open "Documentation" TODO item

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:457 on 2024-12-19 12:41_

> But `undefined_visibility` reads a little oddly

Glad we agree. That's what I had before, but it sounded too much like a "visibility that is undefined".



> We could literally just call it `scope_start_visibility`?

I like it.

---

_@sharkdp reviewed on 2024-12-19 12:41_

---

_@sharkdp reviewed on 2024-12-19 13:00_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1441 on 2024-12-19 13:00_

I'd rather not say how long it took me to get `ast::Expr::BoolOp` part below right :smile:. There might be a simpler formulation of this with less snapshotting and resetting, but I need a break from these ~20 lines of code.

---

_@sharkdp reviewed on 2024-12-19 13:03_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:351 on 2024-12-19 13:03_

I renamed it to `simplify_visibility_constraints` now and added an explanation.

---

_@sharkdp reviewed on 2024-12-19 13:05_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1408 on 2024-12-19 13:05_

Yes. I had negated visibility constraints in here, but reverted that before putting it up for review, as it broke even more test cases (by hiding things from earlier in the scope). Anyway, I figured it out now, so no need for any TODOs.

---

_@sharkdp reviewed on 2024-12-19 14:16_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/statically_known_branches.md`:1 on 2024-12-19 14:16_

Yes, good suggestion — thanks! see https://github.com/astral-sh/ruff/pull/15019/commits/c2efc77ded24997c602c97ad3bc372615281b2c2

---

_@sharkdp reviewed on 2024-12-19 14:49_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:279 on 2024-12-19 14:49_

> Is it the case that if we had perfect simplification of the binary expression trees, this optimization would be unnecessary?

Hm... maybe? Consider this example:
```py
y = 1

if test1:
    pass
elif test2:
    pass

use(y)
```

Ideally, we would see that `y` is always visible. But the constraint that we build is the following:
```
test1 OR (~test1 AND test2) OR (~test1 AND ~test2)
```
If we had binary logic, that condition would always be true. But since we have ternary logic, that is not the case. If both `test1` and `test2` are ambiguous, then the constraint evaluates to ambiguous as well.

The problem is that we also need information about mutual exclusivity. `test1` can be of ambiguous static truthiness, but the pair `(test1, ~test1)` can be either `(true, false)` or `(false, true)`.

So if we somehow go outside the bounds of ternary logic and add this information (maybe using the `is_positive` flag), it might be possible. But staying within the realms of ternary logic, we can't even simplify `test1 OR ~test` to always-true¹. "Kleene logic has no tautologies".

¹ To be clear, this *is* a simplification that we already perform. But nothing more advanced.

---

_@sharkdp reviewed on 2024-12-19 15:15_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:18 on 2024-12-19 15:15_

> Both are just `u32` IDs

Thanks for the reminder. I need to get used to salsa-macro-annotated structs :upside_down_face:.

This is so much better now.

---

_@sharkdp reviewed on 2024-12-19 15:16_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:286 on 2024-12-19 15:16_

Yes, thanks. I added the assertion and fixed a bug related to this (in for loops and try-except, we need to report a `VisibilityConstraint::Ambiguous` to mark the ambiguous branching point).

---

_@sharkdp reviewed on 2024-12-19 15:42_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:347 on 2024-12-19 15:42_

Yes. I am using "boundness" as "declaredness" in some cases now.

I have the feeling that it will be easier to solve those declaredness-vs-boundness issues you mentioned with the changes on this branch, as we simplify the boundness/undeclaredness logic quite a bit. But let me know if we should do something here and now.

---

_@sharkdp reviewed on 2024-12-19 15:43_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:272 on 2024-12-19 15:43_

It improved performance by a tiny bit (<1%). And it's also a bit more readable, thanks.

---

_@sharkdp reviewed on 2024-12-19 15:53_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:97 on 2024-12-19 15:53_

It seems we don't have any test coverage for this. I'll note it down as a follow-up task. Changed to return a union in both cases for now.

---

_@sharkdp reviewed on 2024-12-19 15:54_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:85 on 2024-12-19 15:54_

Yes, around 1%.

---

_Marked ready for review by @sharkdp on 2024-12-19 16:00_

---

_Review requested from @MichaReiser by @sharkdp on 2024-12-19 16:00_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-12-19 16:00_

---

_@carljm reviewed on 2024-12-19 16:15_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:331 on 2024-12-19 16:15_

I think my third paragraph was a bad suggestion and I should have just left it out; it sounds like you already explored that design space. Happy if we can just avoid the extra references in every `BindingWithConstraints`.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:298 on 2024-12-19 16:27_

Referencing a previous review comment by @MichaReiser: https://github.com/astral-sh/ruff/pull/14759#discussion_r1881630989 

This is probably still relevant here.

---

_@sharkdp reviewed on 2024-12-19 16:27_

---

_@sharkdp reviewed on 2024-12-19 16:35_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/statically_known_branches.md`:253 on 2024-12-19 16:35_

Referencing a review comment by @AlexWaygood here: https://github.com/astral-sh/ruff/pull/14759#discussion_r1882429730

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/statically_known_branches.md`:253 on 2024-12-19 16:36_

I really like that we have parameter type inference and can use them in tests, but somehow I prefer the flat structure of all the tests here. If I would use `def _(flag: bool)`, then all tests that use ambiguous truthiness would have a different indentation level than the tests operating on `True`/`False`.

---

_@sharkdp reviewed on 2024-12-19 16:36_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/python_platform.rs`:16 on 2024-12-19 16:37_

interesting -- do pyright/mypy allow arbitrary strings for their equivalent setting?

Obviously mypy will _default_ to whatever `sys.platform` is in Python at runtime, which could be any arbitrary string (within certain limits)...

The reason I'm not a _massive_ fan of this is it seems like it would be really easy to make a typo in a config file like `python-platform = "linus"` and then get really weird (and hard-to-debug) type-checking results. See also our existing linter checks that try to catch bugs in this genre:
- https://docs.astral.sh/ruff/rules/unrecognized-platform-name/
- https://docs.astral.sh/ruff/rules/unrecognized-platform-check/

It just seems like a shame not to have _any_ validation here... but there are obviously _many_ different values that `sys.platform` could be at runtime, so I'm not sure what the best solution is...

---

_@sharkdp reviewed on 2024-12-19 16:38_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:29 on 2024-12-19 16:38_

We don't need to export `ScopedConstraintId` anymore here.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/statically_known_branches.md`:253 on 2024-12-19 16:38_

I know what you mean. I worry that writing the tests like this makes them a little prone to future breakage as well, though, since at some point we'll start emitting errors on functions like this that are annotated as returning `bool` but _really_ return `None`

---

_@sharkdp reviewed on 2024-12-19 16:39_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:95 on 2024-12-19 16:39_

Changed it now.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:420 on 2024-12-19 16:43_

Is it worth creating a `Declaration` struct rather than using a 3-item tuple here?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:519 on 2024-12-19 16:46_

nit: would it be better to have this method as a manual implementation of `Default::default`? It might lead to a slightly smaller diff in this PR (you wouldn't have to update uses of `UseDefMapBuilder::deafult()` to `UseDefMapBuilder::new()`), and implementing `Default` obviously has several other advantages

---

_@carljm reviewed on 2024-12-19 16:52_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:347 on 2024-12-19 16:52_

No, I think this is fine for now, and we should have a follow up on how we treat boundness vs declaredness.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:616 on 2024-12-19 16:55_

I believe this is equivalent, and it's a bit simpler? (All tests pass, anyway!)

```suggestion
        // Loop terminates when we reach a symbol not present in snapshot
        // (means we keep visibility constraints)
        for (current, snapshot) in self.symbol_states.iter_mut().zip(snapshot.symbol_states) {
            current.simplify_visibility_constraints(snapshot);
        }
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:559 on 2024-12-19 16:56_

we can avoid the `len()` call in release builds:

```suggestion
        debug_assert!(self.symbol_states.len() >= snapshot.symbol_states.len());
```

---

_@carljm reviewed on 2024-12-19 16:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/statically_known_branches.md`:253 on 2024-12-19 16:57_

Yeah I don't think we have to enforce a single style here, we can use our judgment about what is nicer in each specific case. There is some inherent value in variety, in that it gives us better incidental test coverage of different areas of inference.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:247 on 2024-12-19 17:01_

```suggestion
        debug_assert_ne!(binding_id, ScopedDefinitionId::UNBOUND);
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:82 on 2024-12-19 17:09_

it looks like this is a pre-existing issue, but it seems odd to me that the `declarations_ty` function returns a `Symbol` rather than a `Type`, and that the variable here is called `declared_ty` when it's a `Symbol`. Anyway, feel free to ignore this comment for now!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:143 on 2024-12-19 17:17_

What if it's a random user-defined module called `sys`? Do we have tests for that?

What you have here _might_ actually be okay for `sys` specifically, because of the fact that certain core stdlib modules can't be overridden by first-party or third-party modules (Python's import machinery _always_ gives priority to the stdlib for these modules), and `sys` is one of them. But we should at least add a comment to this branch, if that's the reason why we're not checking the search path of this module.

I think the first branch of this function is definitely incorrect, actually, now that I look at it again: `typing` is _not_ one of the modules that is immune from being overridden by first-party code. So we need to check that `module.search_path().is_standard_library()` in the first branch of this function before applying the special-casing for `typing.TYPE_CHECKING`

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:388 on 2024-12-19 17:23_

```suggestion
            for other in std::iter::once(second).chain(types) {
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:411 on 2024-12-19 17:23_

```suggestion
                std::iter::once(first).chain(conflicting).collect(),
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:6350 on 2024-12-19 17:27_

```suggestion
            .find_map(|b| b.binding)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:191 on 2024-12-19 17:28_

nit: maybe a manual implementation of `Default` rather than a `new()` method?

---

_@AlexWaygood reviewed on 2024-12-19 17:29_

Fantastic work. I think @carljm will be by far the better reviewer on this overall, but I noticed a few things

---

_Comment by @TomerBin on 2024-12-19 18:23_

Not a reviewer, just wanted to say that's the most beautiful tests I've ever seen 😍

---

_@sharkdp reviewed on 2024-12-19 18:42_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/python_platform.rs`:16 on 2024-12-19 18:42_

> interesting -- do pyright/mypy allow arbitrary strings for their equivalent setting?

Yes, `mypy` allows arbitrary strings for its `--platform` option and also uses these user-provided strings in `sys.platform` checks.

Pyright only allows the strings "Darwin, Linux, or Windows", which seems too restrictive. I don't like that the identifiers deviate from the actual runtime values of `sys.platform`.

/cc @MichaReiser 

---

_@sharkdp reviewed on 2024-12-19 18:48_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:616 on 2024-12-19 18:48_

Yes — thanks!

---

_@AlexWaygood reviewed on 2024-12-19 19:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:143 on 2024-12-19 19:33_

https://github.com/astral-sh/ruff/pull/15071 should help with this -- you'll be able to just do `file_to_module(db, scope.file(db)).is_some_and(|module| module.is_known(KnownModule::Sys))` if it is merged

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:95 on 2024-12-19 21:33_

We discussed that we'll revert this for now with a TODO

---

_@sharkdp reviewed on 2024-12-19 21:33_

---

_@sharkdp reviewed on 2024-12-19 21:36_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:298 on 2024-12-19 21:36_

@MichaReiser I discussed this with @carljm: Using `node_ref` here should be fine, since the `Expression`s always come from the same file from which we call `VisibilityConstraints::evaluate`.

---

_@carljm reviewed on 2024-12-20 00:41_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/statically_known_branches.md`:253 on 2024-12-20 00:41_

> at some point we'll start emitting errors on functions like this that are annotated as returning `bool` but _really_ return `None`

I agree that's a concern; it might be better to actually include a `return True`. Of course, then that's also vulnerable to a (less certain) potential future where we do some inlining / return type inference of small functions and would infer that it always returns True.

---

_@carljm reviewed on 2024-12-20 00:51_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:121 on 2024-12-20 00:51_

Ah, I had missed the singular/plural difference! I agree that "a visibility constraint can be a tree of visibility constraints" is not a good reason to use plural here.

I think your two-identical-types solution here is fine, but I also think that since we've already lost direct parallel with `ConstraintsPerBinding`, it would be fine to lose parallel here as well, and have `ConstraintsPerBinding` and `VisibilityConstraintsPerDefinition`. This also represents a real difference in how they are used.

Mostly I think it would be good in your documentation pass to add some comments on these types; right now the only comment says "similar to above but for visibility constraints", when actually there are key differences from above.

---

_@carljm reviewed on 2024-12-20 02:45_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:279 on 2024-12-20 02:45_

Ok, so I spent some more time thinking about this and reading up on boolean logic, and I think I pointed us in the wrong direction by suggesting three-value logic for this problem. Or not necessarily the _wrong_ direction, as the problem can be solved that way (and you've done that), but not the optimal direction.

This problem is better solved by a regular binary decision diagram, where we first build the full BDD, then in type inference we [restrict](https://docs.rs/oxidd/latest/oxidd/trait.BooleanFunctionQuant.html#method.restrict) it by providing values for the variables (test conditions) that have static truthiness, and then visibility is determined by [satisfiability](https://docs.rs/oxidd/latest/oxidd/trait.BooleanFunction.html#method.satisfiable) of the restricted function (where the only remaining variables in the restricted function would be the test conditions that have ambiguous truthiness). Building a reduced ordered BDD will automatically perform the kinds of simplifications we've discussed here; e.g. I've verified that using OxiDD, building a BDD for the function `test1 OR (~test1 AND test2) OR (~test1 AND ~test2)` does immediately simplify to just TRUE. (There are several BDD libraries for Rust; not sure if OxiDD is the best, it's just the one I've been playing with.)

So I'm feeling increasingly confident that BDDs will be the path to our desired performance characteristics here. I think we should still go ahead and land this and consider BDD as a further optimization exploration, as we discussed today.

---

_@MichaReiser reviewed on 2024-12-20 08:15_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/python_platform.rs`:16 on 2024-12-20 08:15_

I don't mind using the `sys.platform` names directly. They're a bit more cryptic but they are also used in environment markers https://peps.python.org/pep-0496/

---

_@MichaReiser reviewed on 2024-12-20 08:21_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:384 on 2024-12-20 08:21_

Nit: Can we use `find_map` here? Can we add a documentation why calling `unwrap` is safe?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:305 on 2024-12-20 08:22_

It might be worth to add some documentation. E.g. to me it's unclear what the difference between `add_constraint` and `record_constraint` is. When should I use which?

---

_@sharkdp reviewed on 2024-12-20 08:25_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/statically_known_branches.md`:253 on 2024-12-20 08:25_

Done.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:1 on 2024-12-20 08:26_

Can you review the module documentation for outdated documentation (if you haven't already done so)?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:559 on 2024-12-20 08:28_

I'm fairly confident that rust/llvm can remove the `len` call in release builds

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:62 on 2024-12-20 08:29_

Could you add some documentation for what `UNBOUND` is used and why it's okay to use 0?

---

_@sharkdp reviewed on 2024-12-20 08:34_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:519 on 2024-12-20 08:34_

Well there is only one place where we create a `UseDefMapBuilder`, and this `new()` function does something non-trivial. But in the sense of https://rust-lang.github.io/rust-clippy/master/index.html#new_without_default, I now changed it to a custom `Default` impl.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:298 on 2024-12-20 08:42_

That might be, but the problem is that `evaluate` isn't a salsa query, nor is the code calling into `evaluate.` The problem is, to some extent, pre-existing because `symbol_id` already depends on the `UseDefMap`, that's why I think we can tackle it as a follow-up (but we definitely should): 

The flow I'm concerned about is:

* `Type::member` calls `types::symbol` which is not a salsa query (nor is `Type::member`)
* `symbol` calls `types::symbol_by_id` which resolves the use def map (so it's a pre-existing issue!)
* `symbol_by_id` calls into `declarations_ty` which calls `evaluate` and depends on the AST

There are other examples for the same flow which, I think are all cross-module:

* `global_symbol`
* `Class:own_member`

We should add a comment if, for some reason, the constraint holds that `symbol_by_id` is only called for the same file and never cross files.


---

_@sharkdp reviewed on 2024-12-20 08:45_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index.rs`:384 on 2024-12-20 08:45_

> Can we use `find_map` here?

Yes, thanks!



> Can we add a documentation why calling `unwrap` is safe?

It is *not* safe to call unwrap here. But note that this is only used in tests. I changed it to `.expect(…)` now and added `#[track_caller]`.

---

_@MichaReiser reviewed on 2024-12-20 08:45_

---

_@sharkdp reviewed on 2024-12-20 08:46_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:1 on 2024-12-20 08:46_

Yes — sorry. See the other review comment about this and my open "Documentation" TODO in the description. I'll definitely add this.

---

_@sharkdp reviewed on 2024-12-20 08:50_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:360 on 2024-12-20 08:50_

Right, the duplication also bothered me a bit. I'll leave it as is for now, given that we might revisit performance later anyway.

---

_@sharkdp reviewed on 2024-12-20 09:05_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:342 on 2024-12-20 09:05_

We discussed this separately. The short summary is that we can't add this assertions, as there can be situations in which we call `declarations_ty` (or `bindings_ty`?) in type inference of unreachable code, where this assertion is not fulfilled.


The longer explanation is this: in an example like
```py
(x := 0) and (x := 1) and (x := "foo")
```
- when we infer types for that named expression `x := "foo"`, we call `declarations_ty` to make sure that we can assign `"foo"` to the declared type of `x`.
- the only possible control flow path that leads to `x := "foo"` goes through the other two expressions. 
- along this path, we apply visibility constraints. we apply a visibility constraint of `(x := 0)` to the `(x := 1)` binding, because it is only visible if that first condition is not always-false (which it is here) 
- The `(x := 1)` expression is unreachable code
- When we apply that `(x := 0)` visibility constraint, we also apply it to the scope-start binding/declaration.
- so now if we ask for the declared type when inferring types for `(x := "foo")`, we see the scope-start with a visibility constraint of `(x := 0)`, i.e. always-false
- the scope start is not "reachable" from there, but that is fine

---

_@sharkdp reviewed on 2024-12-20 09:07_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:559 on 2024-12-20 09:07_

I was about to comment the same thing, but I guess Alex' version is more readable anyway :smile: 

---

_@sharkdp reviewed on 2024-12-20 09:15_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/python_platform.rs`:16 on 2024-12-20 09:15_

Ok, we discussed to revisit this later.

---

_@sharkdp reviewed on 2024-12-20 10:40_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:26 on 2024-12-20 10:40_

I started the experiment here, but gave up after ~1.5 hours with a lot of lifetime errors still to be resolved: https://github.com/astral-sh/ruff/pull/15079

---

_@MichaReiser reviewed on 2024-12-20 12:22_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:298 on 2024-12-20 12:22_

https://github.com/astral-sh/ruff/pull/15080 could address this and may also help with performance if it reduce (caches) the constraints that need solving?

---

_@sharkdp reviewed on 2024-12-20 13:13_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:121 on 2024-12-20 13:13_

Done.

---

_Comment by @sharkdp on 2024-12-20 15:24_

I did the following benchmark to see how to set the recursion limit (where `red_knot_xy` has a `MAX_RECURSION_DEPTH` of `xy`):

```bash
hyperfine \                                                                                          
  --warmup=20 \
  --shell=none \
  --ignore-failure \
  --export-json results.json \
  -L limit 0,8,16,24,32,48,64,no_limit \
  "./red_knot_{limit} --project path/to/black"
```

See the results below. There are two things to notice: there is no significant change in performance when going from zero (i.e. no static visibility analysis at all!) all the way up to a limit of ~32. After that, there is a jump of the runtime by a factor of five. I'll set the limit to 24 for now.

![benchmark](https://github.com/user-attachments/assets/3c0692c9-23a7-4fd7-af6b-4b130981f26e)

Zoomed in on the runtimes up to a limit of 32:

![image](https://github.com/user-attachments/assets/8faf3f7c-4439-4e08-9630-5b6f272db9c5)


---

_@sharkdp reviewed on 2024-12-20 15:32_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/statically_known_branches.md`:1500 on 2024-12-20 15:32_

I wanted to format this in a nicer way, but black wouldn't let me.

---

_@AlexWaygood reviewed on 2024-12-20 15:41_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/statically_known_branches.md`:1500 on 2024-12-20 15:41_

You can use `# fmt: skip` to switch black off for a single line, or `# fmt: off` to switch black off for a whole Python snippet (but use with restraint, or it defies the point of having an autoformatter 😉)

---

_@sharkdp reviewed on 2024-12-20 16:05_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:123 on 2024-12-20 16:05_

I need to look at this section again. I'm not sure if that makes sense. Or if there is another way to solve this without explicit ambiguity constraints.

---

_@carljm reviewed on 2024-12-20 17:44_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:26 on 2024-12-20 17:44_

Oof. Yeah I don't think we should spend more time on this now, since I think it would may become irrelevant if we switch to a BDD representation of the constraint tree.

---

_@carljm reviewed on 2024-12-20 17:50_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:298 on 2024-12-20 17:50_

Looks like #15080 has been rolled directly into this PR. That seems like a robust and general fix. I guess when we have a tracked query that takes >1 argument (and one of them is not a Salsa ID), then effectively under the hood Salsa will end up creating an ingredient key for this `(ScopeId, ScopedSymbolId)` pair?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:246 on 2024-12-20 17:57_

This is really good! One nit on the wording: unlike Rust, Python automatically converts an expression of any type to bool in a boolean context. So `test` itself need not be of type `Literal[False]` or `Literal[True]` or `bool`; it could be of any type. What we actually look at is the return type of the `__bool__` method of `test`: is it of type `Literal[False]` or `Literal[True]` or `bool`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:280 on 2024-12-20 18:01_

Is this accurate?
```suggestion
    /// Array of [`Definition`] in this scope. Only the first entry should be `None`;
    /// this represents the implicit "unbound" definition of every symbol.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:275 on 2024-12-20 18:10_

I think this type no longer needs to be `pub(crate)`, it's only used in this file.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:422 on 2024-12-20 18:19_

Similar to a question in my last review, why is it that we need each individual `BindingWithConstraints` to carry a reference to the `VisibilityConstraints`, which is shared data for all bindings in the scope? Why can't the `BindingWithConstraintsIterator` carry a single reference? It seems everywhere we use this in type inference, we have the iterator available. Is there a lifetimes problem?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:451 on 2024-12-20 18:20_

Same question here as above; why does each individual `DeclarationWithConstraint` need to carry a copy of this reference?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:583 on 2024-12-20 18:23_

At a high level, it would be nice if we could simplify these APIs and avoid exposing the use-def map's internal IndexVec IDs to the semantic index builder. But I realize that you didn't add these for no good reason, it's because you needed them to get the semantics right (it looks like in particular for the BoolOp case.) I don't think we should spend more time right now trying to see if we can simplify this, just maybe something to keep in mind in future if/when we revisit this code.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:273 on 2024-12-20 18:25_

```suggestion
    /// Add given visibility constraint to all live definitions.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:123 on 2024-12-20 18:38_

I think it makes sense? On a statically-analyzable branch, we have to add `test` visibility to the branch path and `~test` visibility to the not-branch path. `VisibilityConstraint::Ambiguous` is just a shortcut to do the same thing in the case where there is no `test` expression we plan to analyze for truthiness later. It's "the same thing" because `ambiguous` and `~ambiguous` are indistinguishable; we can just add `VisibilityConstraint::Ambiguous` to both paths.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:169 on 2024-12-20 18:39_

Maybe add a comment here recording your performance experimentation on the Black codebase that led to picking this number?

---

_@carljm approved on 2024-12-20 18:50_

This is looking good to me! Just a few nits, feel free to land after addressing.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:36 on 2024-12-20 18:53_

Looks like Rust is trying to consider this a doctest (probably because of the indentation?) and failing to compile it.

---

_@carljm reviewed on 2024-12-20 18:54_

---

_@sharkdp reviewed on 2024-12-21 10:12_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:246 on 2024-12-21 10:12_

> Python automatically converts an expression of any type to bool in a boolean context

Right. I was wondering whether or not I should bring it up and decided to keep it simple, because I didn't want to talk too much about types in the use-def-map module. But it's better to be precise, rather than concise. Changed now.

---

_@sharkdp reviewed on 2024-12-21 10:17_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:422 on 2024-12-21 10:17_

No, the suggestion is fine — thanks.

---

_@sharkdp reviewed on 2024-12-21 10:24_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:583 on 2024-12-21 10:24_

Fully agreed. I will note it down as a TODO item for myself.

---

_Merged by @sharkdp on 2024-12-21 10:33_

---

_Closed by @sharkdp on 2024-12-21 10:33_

---

_Branch deleted on 2024-12-21 10:33_

---

_Comment by @MichaReiser on 2024-12-21 13:36_

Congrats on landing this massive improvement! Enjoy your time off

---

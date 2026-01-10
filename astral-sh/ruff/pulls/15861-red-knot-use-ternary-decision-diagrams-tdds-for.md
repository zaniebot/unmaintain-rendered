```yaml
number: 15861
title: "[red-knot] Use ternary decision diagrams (TDDs) for visibility constraints"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/tdd
created_at: 2025-01-31T21:51:14Z
updated_at: 2025-02-04T19:32:13Z
url: https://github.com/astral-sh/ruff/pull/15861
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Use ternary decision diagrams (TDDs) for visibility constraints

---

_Pull request opened by @dcreager on 2025-01-31 21:51_

We now use ternary decision diagrams (TDDs) to represent visibility constraints.  A TDD is just like a BDD ([_binary_ decision diagram](https://en.wikipedia.org/wiki/Binary_decision_diagram)), but with "ambiguous" as an additional allowed value.  Unlike the previous representation, TDDs are strongly normalizing, so equivalent ternary formulas are represented by exactly the same graph node, and can be compared for equality in constant time.

We currently have a slight 1-3% performance regression with this in place, according to local testing.  However, we also have a _5× increase_ in performance for pathological cases, since we can now remove the recursion limit when we evaluate visibility constraints.

As follow-on work, we are now closer to being able to remove the `simplify_visibility_constraint` calls in the semantic index builder.  In the vast majority of cases, we now see (for instance) that the visibility constraint after an `if` statement, for bindings of symbols that weren't rebound in any branch, simplifies back to `true`.  But there are still some cases we generate constraints that are cyclic.  With fixed-point cycle support in salsa, or with some careful analysis of the still-failing cases, we might be able to remove those.

---

_Comment by @carljm on 2025-02-01 00:17_

Very excited about this!

Per your request (and the "draft" status), I'll refrain from a detailed look at the code until you declare it ready for review. Just one general thought, based on the title of the PR: when I looked at this previously, my conclusion was that we don't actually need TDDs here, BDDs suffice. If we build a BDD of the constraints, and then evaluate that BDD, anytime a decision node evaluates to "ambiguous", we can immediately short-circuit to a result of "ambiguous". I guess another way to say this is that a TDD where the "ambiguous" exit of every node leads immediately to the "ambiguous" terminal (which is our scenario, I think), is isomorphic to a BDD where the "ambiguous" exits can be implied.

Just mentioning that in case it's useful or allows any simplification (I don't know if it does, since I haven't looked at your code yet.). Ultimately I'll be happy with any solution that performs well and allows us to get rid of the arbitrary depth limit, no matter what we call it!

(One other thought: if we are getting rid of the depth limit, we should run this on Black and verify that the big perf regression @sharkdp observed is in fact gone.)

---

_Label `red-knot` added by @AlexWaygood on 2025-02-01 00:19_

---

_Comment by @dcreager on 2025-02-01 02:09_

> If we build a BDD of the constraints, and then evaluate that BDD, anytime a decision node evaluates to "ambiguous", we can immediately short-circuit to a result of "ambiguous".

Hmm, I thought we'd need to model ambiguous edges fully, since (e.g.) `ambiguous ∧ false = false`.  So if we have a constraint of `C1 ∧ C2`, where `C1` is `amb` and `C2` is `false`, I think your optimization would over-approximate the result to `amb` instead of `false`.  Am I following that right?

> (One other thought: if we are getting rid of the depth limit, we should run this on Black and verify that the big perf regression @sharkdp observed is in fact gone.)

Ah, that makes me a bit happier about the performance numbers I'm seeing!  Codspeed is reporting a 3% regression with this in place.  When I test locally, using hyperfine on black, I see similar numbers.  But that might not be apples-to-apples, since `main` has a recursion limit in place but this feature branch does not.

---

_Comment by @carljm on 2025-02-01 16:05_

> I thought we'd need to model ambiguous edges fully, since (e.g.) `ambiguous ∧ false = false`

You're right, of course! I remembered that I'd reached the conclusion we could do this with just BDDs, but when I tried to recall the reasoning for that conclusion, I thought I'd come up with a simpler reason, but instead it was just a wrong reason :)

The actual reason I'd reached this conclusion before was because OxiDD will allow you to supply "resolutions" (that is, t/f values) for nodes in a BDD and will then simplify the BDD accordingly, such that the resolved nodes disappear from it. So I thought if we built a BDD and then resolved the nodes that we can infer a statically-known truthiness for, the BDD would either simplify to just a terminal, or not, in which case it remains ambiguous.

But I don't know if that's actually a good/efficient approach, compared to building a TDD and evaluating it!

> Codspeed is reporting a 3% regression with this in place. When I test locally, using hyperfine on black, I see similar numbers. But that might not be apples-to-apples,

IIRC @sharkdp was seeing like 5x regression on Black without the recursion limit, so it seems like you're doing much better on the pathological cases. It's too bad if this comes with an across-the-board regression, but I think it's worth it to get rid of the arbitrary limit. We probably will need such limits in some places, but they are a really bad experience when users encounter them, so as much as we can avoid them, or make them so high that users will never encounter them in realistic code, we should try to do so.

Of course we should also take a look at profiles and see if there are ways to bring down the regression here!

---

_Comment by @github-actions[bot] on 2025-02-03 15:24_

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

_Comment by @dcreager on 2025-02-03 15:26_

I've extracted the refactoring parts of this into a separate PR to make everything easier to review: https://github.com/astral-sh/ruff/pull/15913

---

_Comment by @codspeed-hq[bot] on 2025-02-03 15:36_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Ftdd)

### Merging #15861 will **not alter performance**

<sub>Comparing <code>dcreager/tdd</code> (ec35dcf) with <code>main</code> (6bb3235)</sub>



### Summary

`✅ 32` untouched benchmarks  





---

_Comment by @dcreager on 2025-02-03 18:36_

> IIRC @sharkdp was seeing like 5x regression on Black without the recursion limit, so it seems like you're doing much better on the pathological cases. It's too bad if this comes with an across-the-board regression, but I think it's worth it to get rid of the arbitrary limit. We probably will need such limits in some places, but they are a really bad experience when users encounter them, so as much as we can avoid them, or make them so high that users will never encounter them in realistic code, we should try to do so.
> 
> Of course we should also take a look at profiles and see if there are ways to bring down the regression here!

As expected, the execution time shifts from being mostly in the evaluate step on `main`:

![main](https://github.com/user-attachments/assets/9985524d-9ec6-496f-a0aa-ecedcaaf4474)

to being mostly in the build step with this PR:

![feature](https://github.com/user-attachments/assets/2d2657a4-69b2-4107-9ea7-60323a08b3cc)


---

_Comment by @dcreager on 2025-02-03 19:47_

I think this is at a good point now.  In local testing, I've gotten the performance regression down to 1-2%.  Interested to see what codspeed says about the latest commits on the branch.

---

_Marked ready for review by @dcreager on 2025-02-03 19:48_

---

_Review requested from @carljm by @dcreager on 2025-02-03 19:48_

---

_Review requested from @MichaReiser by @dcreager on 2025-02-03 19:48_

---

_Review requested from @AlexWaygood by @dcreager on 2025-02-03 19:48_

---

_Review requested from @sharkdp by @dcreager on 2025-02-03 19:48_

---

_Comment by @carljm on 2025-02-03 20:05_

For some reason CodSpeed [thinks](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Ftdd) it doesn't have data on the baseline for this PR, but just comparing the results here with the ones on main (or `vc-api`), it looks like about 2% regression (89.5 ms -> 91.6 ms).

---

_Comment by @carljm on 2025-02-03 20:09_

> As follow-on work, we are now closer to being able to remove the simplify_visibility_constraint calls in the semantic index builder. In the vast majority of cases, we now see (for instance) that the visibility constraint after an if statement, for bindings of symbols that weren't rebound in any branch, simplifies back to true. But there are still some cases we generate constraints that are cyclic. With fixed-point cycle support in salsa, or with some careful analysis of the still-failing cases, we might be able to remove those.

I would definitely like to see the failing cases and understand why they need to be cyclic, or if there's some missing simplification (other than the "no new bindings" heuristic that doesn't work with with terminals) that could avoid the cycles. But this can be a follow-up.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:262 on 2025-02-03 20:12_

16 million constraints ought to be enough for anyone!

---

_Comment by @dcreager on 2025-02-03 20:14_

> For some reason CodSpeed [thinks](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Ftdd) it doesn't have data on the baseline for this PR, but just comparing the results here with the ones on main (or `vc-api`), it looks like about 2% regression (89.5 ms -> 91.6 ms).

I rebased after merging #15913, which should hopefully update codspeed

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:385 on 2025-02-03 20:22_

This doesn't define any deterministic ordering of terminal nodes, but just always returns `Greater` for any comparison of two terminals (that is, the result depends on which is `a` and which is `b`.) I guess in practice that's OK, because the implementations of `add_and_constraint` and `add_or_constraint` always handle any both-terminal case up-front, before needing to order nodes? Should that implicit requirement be documented?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:284 on 2025-02-03 20:26_

Typically we use `IndexVec` (from `ruff_index` crate) for this kind of indexed arena; I assume you considered this and rejected it?

The bit-packing of constraint indices might make that trickier for constraints.



---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:334 on 2025-02-03 20:27_

Should we at least have a `debug_assert` that `self.constraints.len()` fits in a u32?

---

_Comment by @dcreager on 2025-02-03 20:28_

> I would definitely like to see the failing cases and understand why they need to be cyclic, or if there's some missing simplification (other than the "no new bindings" heuristic that doesn't work with with terminals) that could avoid the cycles. But this can be a follow-up.

As one example, [`flake8_return/RET503.py`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/resources/test/fixtures/flake8_return/RET503.py) works fine when analyzed as a `.py` file, but has a cycle when analyzed as a `.pyi` file

---

_Comment by @dcreager on 2025-02-03 20:34_

> As one example, [`flake8_return/RET503.py`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/resources/test/fixtures/flake8_return/RET503.py) works fine when analyzed as a `.py` file, but has a cycle when analyzed as a `.pyi` file

And I minimized it down to this repro:

```py
def foo() -> int:
    def baz() -> int:
        return 1
    if baz() > 3:
        return 1
    raise ValueError
```

Passes as a `.py` file, cycles as a `.pyi` file. Passes if you remove the `if` statement.

---

_Comment by @carljm on 2025-02-03 21:20_

Summarizing from Discord: I think a) such code would never actually occur in a stub file; they shouldn't even have function bodies other than ellipsis (and perhaps we should make this explicit in how we handle them), and b) it would be fine to go ahead and remove the simplify calls and mark that file xfail (though ideal if we could mark only the pyi version of it as xfail?) pending fixpoint iteration, which I think would solve the panic.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:343 on 2025-02-03 21:25_

Is it possible to silence this on just the one line, instead of the whole function?

---

_@carljm approved on 2025-02-03 21:25_

Looks great to me!

Micha or David may have Rust/perf suggestions I missed, but I think it's OK to go ahead with this and handle any such comments as follow-ups.

---

_Comment by @dcreager on 2025-02-03 21:28_

> Summarizing from Discord: I think a) such code would never actually occur in a stub file; they shouldn't even have function bodies other than ellipsis (and perhaps we should make this explicit in how we handle them), and b) it would be fine to go ahead and remove the simplify calls and mark that file xfail (though ideal if we could mark only the pyi version of it as xfail?) pending fixpoint iteration, which I think would solve the panic.

Done.  Performance between keeping and removing the simplify calls seems to be a wash.  I've removed them in the interests of simpler code, and added a pyi-only xfail for the one failing test.

---

_Comment by @MichaReiser on 2025-02-03 21:34_

I haven't reviewed the code yet but the regression might simply come from that we now evaluate (or simplify) more constraints than before because the semantic indexer processes all constraints whereas simplification during inference was lazy (and only for the queried definitions)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:284 on 2025-02-03 22:18_

Mostly because I thought it was overkill to add `newtype_index`es for the two vectors, but actually that type safety seems like a good thing.  I'll change it

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:334 on 2025-02-03 22:24_

There's a `debug_assert` that we're less than 2^24 in `from_index_and_copy`, though that happens after the cast.  Switching to `IndexVec` gives us that u32 check for free.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:385 on 2025-02-03 22:43_

Good catch, I'll add that as another case; it seems brittle to rely on the caller always doing that

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:343 on 2025-02-03 22:44_

These went away with the switch to `IndexVec`

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:262 on 2025-02-03 22:45_

16 million constraints _per scope_!

---

_@dcreager reviewed on 2025-02-03 22:45_

---

_Comment by @carljm on 2025-02-03 23:24_

Hmm, the switch to `IndexVec` (or at least, something in the recent commits) seems to have grown the regression; now we're seeing a significant regression even in incremental check, which is a bit strange.

---

_@carljm approved on 2025-02-03 23:27_

Semantics and code here look good to me. May be worth understanding why the regression seems to have grown again, and see if that reproduces locally on a larger codebase like Black.

---

_Comment by @dcreager on 2025-02-04 00:07_

> Hmm, the switch to `IndexVec` (or at least, something in the recent commits) seems to have grown the regression; now we're seeing a significant regression even in incremental check, which is a bit strange.

I do see a 2-3% regression between the `IndexVec` commit and the one immediately before it.  My hunch is that it's because we're not storing `ConstraintId` and `InteriorNodeId` directly, and have to convert into that from the bit-hacked `u32` on each access.  That does a bounds check each time.  I'm going to revert that commit and add a note about why we're using `Vec`.  (Alternatively I could add an `unchecked` constructor for the ID types.)

---

_Comment by @dcreager on 2025-02-04 00:24_

> I'm going to revert that commit and add a note about why we're using `Vec`. (Alternatively I could add an `unchecked` constructor for the ID types.)

Ackshually I can write a custom `Idx` impl for a couple of types and bypass the asserts, I think.

---

_Comment by @dcreager on 2025-02-04 01:49_

Still seeing the larger regression with custom `Idx` impls, and again with a revert of the `Vec` → `IndexVec` commit.  I'm stumped, will look at this some more in the morning.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:191 on 2025-02-04 08:08_

```suggestion
            ALWAYS_TRUE => f.field(&"AlwaysTrue").finish(),
            AMBIGUOUS => f.field(&"Ambiguous").finish(),
            ALWAYS_FALSE => f.field(&"AlwaysFalse").finish(),
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:193 on 2025-02-04 08:09_

Nit: Makes it clearer what's different/shared between the branches:

```suggestion
        let mut f = f.debug_tuple("ScopedVisibilityConstraintId");
        match *self {
            ALWAYS_TRUE => f.field(&"AlwaysTrue"),
            AMBIGUOUS => f.field(&"Ambiguous"),
            ALWAYS_FALSE => f.field(&"AlwaysFalse"),
            _ => f.field(&self.0),
        }
        
        f.finish()
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:232 on 2025-02-04 08:11_

I'm not familiar with TDD myself (I'm familiar with test driven development but that's not it). Would it make sense to extend the module level documentation and also include a link to what TDD means in this context?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:219 on 2025-02-04 08:12_

It's unclear to me what a copy number is. Can you expand the comment to cover this in more detail. It may also be worthwhile to explain the internal representation. How many constraints can be expressed etc. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:270 on 2025-02-04 08:15_

I'd expected this to also be defined on `ScopedVisiblityConstraitId` as it is used in `is_terminal` and that there's only an alias here. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:285 on 2025-02-04 08:18_

if we end up not using `IndexVec`, I still recommend to create a newtype wrapper around `u32` to signify what the `u32` here means (it's an index into the constraints/interiors vec?)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:283 on 2025-02-04 08:30_

This struct defines a fair amount of heap-allocated data structures and requires a lot of hashing. It would be nice not to have to do that, but I don't see how we can restructure the code to avoid it (unless maybe combining some maps and encoding the `or`/`and` in the key but that might as well turn out to be slower if it happens that the map has to resize more often because of it.

---

_@MichaReiser approved on 2025-02-04 08:30_

Nice :) I only have nit/documentation comments. 

It would be nice if we could use `IndexVec` or a newtype wrapper and implement `std::ops::Index` but it seems you tried that and it regressed performance.

It would also be great if we understand the reason for the perf regression better. Where are we spending more time now? Is it because we do more work eagerly or is it because the caching is expensive? Could we remove the caching?

---

_@MichaReiser reviewed on 2025-02-04 08:34_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:288 on 2025-02-04 08:34_

Does it make any difference if you remove the assert here just to make sure we compare apples with apples

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:191 on 2025-02-04 18:24_

The `format_args!` part makes this render as a symbol, not a string: `ScopedVisibilityConstraintId(AlwaysTrue)` as opposed to `ScopedVisibilityConstraintId("AlwaysTrue")`.  It's small and aesthetic but I like it better!  I've added a comment explaining why

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:193 on 2025-02-04 18:24_

Done (modulo above)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:232 on 2025-02-04 18:50_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:288 on 2025-02-04 18:52_

Once I had my performance tests taking into account thermal throttling, it turned out the `Vec` vs `IndexVec`, and `assert` vs `debug_assert`, both did not make a difference.  Leaving this as an `assert` so that we get a proper panic if we ever try to analyze a file that needs more than 16 million constraints!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:270 on 2025-02-04 18:53_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:219 on 2025-02-04 19:01_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:285 on 2025-02-04 19:01_

We're back to using `IndexVec`

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:283 on 2025-02-04 19:02_

The hashing is an important part of the data structure, since we have to intern all of the nodes to guarantee that the TDD is reduced.  (I included a description of what it means to be reduced, and why that's important, in the module documentation that I added).

---

_@dcreager reviewed on 2025-02-04 19:04_

---

_Merged by @dcreager on 2025-02-04 19:32_

---

_Closed by @dcreager on 2025-02-04 19:32_

---

_Branch deleted on 2025-02-04 19:32_

---

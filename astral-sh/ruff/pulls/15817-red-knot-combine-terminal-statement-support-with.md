```yaml
number: 15817
title: "[red-knot] Combine terminal statement support with statically known branches"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/static-terminal
created_at: 2025-01-29T21:34:39Z
updated_at: 2025-05-07T15:22:25Z
url: https://github.com/astral-sh/ruff/pull/15817
synced_at: 2026-01-10T18:57:02Z
```

# [red-knot] Combine terminal statement support with statically known branches

---

_Pull request opened by @dcreager on 2025-01-29 21:34_

This example from @sharkdp shows how terminal statements can appear in statically known branches: https://github.com/astral-sh/ruff/pull/15676#issuecomment-2618809716

```py
def _(cond: bool):
    x = "a"
    if cond:
        x = "b"
        if True:
            return

    reveal_type(x)  # revealed: "a", "b"; should be "a"
```

We now use visibility constraints to track reachability, which allows us to model this correctly.  There are two related changes as a result:

- New bindings are not assumed to be visible; they inherit the current "scope start" visibility, which effectively means that new bindings are visible if/when the current flow is reachable

- When simplifying visibility constraints after branching control flow, we only simplify if none of the intervening branches included a terminal statement.  That is, earlier unaffected bindings are only _actually_ unaffected if all branches make it to the merge point.

---

_Label `red-knot` added by @AlexWaygood on 2025-01-29 22:24_

---

_Marked ready for review by @dcreager on 2025-01-30 16:40_

---

_Review requested from @carljm by @dcreager on 2025-01-30 16:40_

---

_Review requested from @MichaReiser by @dcreager on 2025-01-30 16:40_

---

_Review requested from @AlexWaygood by @dcreager on 2025-01-30 16:40_

---

_Review requested from @sharkdp by @dcreager on 2025-01-30 16:40_

---

_Converted to draft by @dcreager on 2025-01-30 16:41_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:510 on 2025-01-30 19:02_

At first I replaced this with `reachability: ScopedVisibilityConstraintId`, but I realized that `scope_start_visibility` is already what we need: the start of the scope is visible iff the flow is still reachable.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:680 on 2025-01-30 19:03_

Commenting out these two `if` clauses is how we verify that this is truly an optimization — we should get the same results for the tests with and without it

---

_@dcreager reviewed on 2025-01-30 19:08_

This is not currently working because of visibility constraint simplification:

```py
def _(cond: bool):
    x = "a"
    if cond:
        x = "b"
        # ← {0}
        if True:
            return
            # ← {1}
        # ← {2}

    reveal_type(x)  # revealed: Literal["a"]
```

At point `{1}`, we're marking the `x = "b"` binding as non-visible (by setting a visibility constraint of `ALWAYS_FALSE`).

But at point `{2}`, after we merge the two flows back together (the `then` flow and the artifically inserted `else` flow), we simplify the result relative to point `{0}`.  That sees that there weren't any new bindings of `x`, and resets the visibility of `x = "b"` back to what it was at point `{0}`, forgetting the unreachability that we just introduced.

So I think I need to skip the simplification step when a flow contains a terminal statement.

---

_Comment by @carljm on 2025-01-30 20:08_

> So I think I need to skip the simplification step when a flow contains a terminal statement.

This sounds right. The simplification step is a bit of a performance hack. I think it could be eliminated if we used BDDs instead of syntax trees to represent visibility constraints (since BDDs self-simplify). But I think it should never be _wrong_ to skip the simplification, just potentially hurt performance. Which in this case shouldn't be too bad, since it would only occur when there's a terminal statement in the branch, which won't be the common case.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:456 on 2025-01-30 20:24_

Does it matter that it's `raise`? I think anytime the `try` block unconditionally terminates (via any terminal statement) that means we can't enter the `else` block. I'm not sure why we'd have a hard time modeling this, since I think entry into the `else` block just comes off the end of the `try` block, so we should mark that whole flow as unreachable?

Or to put it another way, why does this regress in this PR, if we handled it correctly before?

---

_Comment by @codspeed-hq[bot] on 2025-01-30 21:32_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fstatic-terminal)

### Merging astral-sh/ruff#15817 will **degrade performances by 7.96%**

<sub>Comparing <code>dcreager/static-terminal</code> (7423de2) with <code>main</code> (d47088c)</sub>



### Summary

`❌ 1 (👁 1)` regressions  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` red_knot_check_file[incremental] `` | 4.8 ms | 5.2 ms | -7.96% |


---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:543 on 2025-02-04 20:23_

New bindings should only be considered visible if/when the flow is reachable

---

_@dcreager reviewed on 2025-02-04 20:23_

---

_Comment by @dcreager on 2025-02-04 21:30_

Running `cargo bench` locally agrees with codspeed that this is a performance regression, but using hyperfine on both black and tomllib says that it's a slight performance increase!

---

_Marked ready for review by @dcreager on 2025-02-04 21:34_

---

_Comment by @dcreager on 2025-02-04 21:34_

I'm marking this as ready for review to get :eyes: on it.  Just like the TDD patch that this builds on, I'm quite confused by the codspeed findings.

---

_Renamed from "[red-knot] [WIP] Combine terminal statement support with statically known branches" to "[red-knot] Combine terminal statement support with statically known branches" by @dcreager on 2025-02-04 21:35_

---

_Comment by @carljm on 2025-02-04 21:35_

> Running `cargo bench` locally agrees with codspeed that this is a performance regression, but using hyperfine on both black and tomllib says that it's a slight performance increase!

CodSpeed says it's a regression on the incremental benchmark, not the cold benchmark. Is your hyperfine testing on black and tomllib testing incremental performance (that is, make an insignificant/comment change to one file and test how fast we re-check incrementally), or cold-check performance?

Incremental regression generally suggests we are creating more Salsa-cached values that have to be revalidated on incremental re-check, or making that revalidation of cached values (rather than the actual semantic indexing and type inference itself) more expensive in some way.

---

_Comment by @dcreager on 2025-02-04 22:11_

> CodSpeed says it's a regression on the incremental benchmark, not the cold benchmark. Is your hyperfine testing on black and tomllib testing incremental performance (that is, make an insignificant/comment change to one file and test how fast we re-check incrementally), or cold-check performance?

hyperfine is testing cold performance, but I was seeing the regression locally on `cargo bench` for the cold test too

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:805 on 2025-02-04 22:35_

Since we usually use `TODO` for things we definitely need to return to and fix, let's get a `TODO: fix definition of public type in function-like scopes, see #15777" or similar somewhere in this comment

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:484 on 2025-02-04 22:36_

Why do we need `UseDefMapBuilder` to not derive `Debug`?

I guess maybe it's so big it's not likely to be useful, can't recall if I've ever debug-dumped an entire `UseDefMapBuilder`. I may have when I was working on it, but it's certainly been a while.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:543 on 2025-02-04 22:42_

Is this the fix to the "visibility constraints only apply to prior, not subsequent, bindings" issue that @sharkdp pointed out on the very first terminals PR that used visibility constraints?

Do we already have (and if not, can we add?) a test showing that a definition that occurs in the unreachable code after a terminal statement in a branch doesn't then become visible after that branch merges?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:599 on 2025-02-04 22:44_

Nice! So you don't need an additional boolean for this after all.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:680 on 2025-02-04 23:07_

And I verified that we can indeed remove these checks and all tests pass! I'm not seeing a detectable performance improvement in the benchmark from including these lines; perhaps that just suggests conditional terminals aren't common enough for it to show up? It definitely seems like this should be faster in cases where it does apply.

---

_@carljm approved on 2025-02-04 23:14_

This looks great!

I think it's worth putting some time-boxed effort (on the scale of a few hours) into looking into the regression here, but I don't think it should block the PR; I don't see anything obviously inefficient here, and this is what we need in order to get the right semantics. It's a better use of optimization effort to look broadly for the best ROI than to focus narrowly on a specific regression.

---

_@carljm reviewed on 2025-02-04 23:15_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:456 on 2025-02-04 23:15_

Uh, ignore this -- old review comment that apparently never got submitted before.

---

_@carljm reviewed on 2025-02-04 23:18_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/exception/basic.md`:161 on 2025-02-04 23:18_

Hmm, do you know why this is now an unresolved reference? I would think we'd still want to bind `e` to `Unknown` in this clause, even though `int` is not a valid exception type.

---

_@carljm reviewed on 2025-02-04 23:18_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:641 on 2025-02-04 23:18_

The prose paragraph above this test needs updating.

---

_Comment by @MichaReiser on 2025-02-05 08:02_

Two things I like doing when investigating performance issues are run `red_knot -vvv` and compare between main and my feature (e.g. by running over tomllib):

* the ingredient counts printed just before existing: Are there more or fewer ingredients?
* Paste the logs into a text diff tool and see how they differ (you may want to disable concurrency for this)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:543 on 2025-02-05 14:27_

> Is this the fix to the "visibility constraints only apply to prior, not subsequent, bindings" issue that @sharkdp pointed out on the very first terminals PR that used visibility constraints?

It is!

> Do we already have (and if not, can we add?) a test showing that a definition that occurs in the unreachable code after a terminal statement in a branch doesn't then become visible after that branch merges?

The `"unreachable"` assignment in the `try`/`else` clause should cover this, but I can add a more direct test of this as well.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:599 on 2025-02-05 14:27_

Yep, this is the main change that I wanted to enable with the TDD PR, since this relies on being able to compare the two TDD node IDs to test whether the reachability of the two branches is equivalent

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:680 on 2025-02-05 14:30_

> I'm not seeing a detectable performance improvement in the benchmark from including these lines

I think it might be that the merge step is now faster: if the other snapshot's reachability is `ALWAYS_FALSE`, then the visibility of all of its bindings should also be `ALWAYS_FALSE`.  (I think there were cases before TDD normalization where we wouldn't be able to see that in the structure of the visibility constraint.)  Merge will iterate through all of the bindings and AND their visibility constraints, but ANDing with `ALWAYS_FALSE` is one of the fast-path returns.

To be clear, it's a hunch — I haven't backed any of :point_up: with data!


---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/exception/basic.md`:161 on 2025-02-05 14:35_

I believe it's because of how new bindings' visibility starts off as the reachability of the current control flow.  The `raise` is currently treated the same as a `return`, which (I think) means that we consider the entire `except` block unreachable now.

That said, the clause immediately above does _not_ have new `unresolved-reference` errors:

```py
try:
    raise
except KeyboardInterrupt as e:  # fine
    reveal_type(e)  # revealed: KeyboardInterrupt
    raise LookupError from e  # fine
```

and the only difference is whether the `except` expression resolves to a subclass of `BaseException` or not...

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:641 on 2025-02-05 14:41_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:484 on 2025-02-05 14:44_

Whoops!  It doesn't anymore.  This is left over from a previous version of this PR, where I had added something non-`Debug` to this struct.  Reverted

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:805 on 2025-02-05 14:50_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:646 on 2025-02-05 14:53_

This is the new test case showing that bindings after a terminal statement are considered not visible. https://github.com/astral-sh/ruff/pull/15817/files#r1941996689

Note that this means we're currently implementing the "least helpful" option in https://github.com/astral-sh/ruff/issues/15797.  (I think that's still okay for this PR, just pointing out that this will change depending on how we decide to handle unreachable code)

---

_@dcreager reviewed on 2025-02-05 14:53_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/exception/basic.md`:161 on 2025-02-05 18:47_

> I believe it's because of how new bindings' visibility starts off as the reachability of the current control flow. The `raise` is currently treated the same as a `return`, which (I think) means that we consider the entire `except` block unreachable now.

This doesn't seem right, both because I think we currently model that an exception could be raised right "before" the `raise` statement, which jumps to `except` blocks, and because (as you observe) the other similar `try: raise` cases above don't have this problem.

This seems worth understanding before we land this (IMO) semantic regression.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:646 on 2025-02-05 18:51_

Makes sense. I would put a TODO on that `unresolved-reference` diagnostic below.

I'm a little worried about the difficulty of implementing "more useful" options for checking unreachable code, but we can leave that as a separate problem.

---

_@carljm reviewed on 2025-02-05 18:52_

---

_@dcreager reviewed on 2025-02-05 19:52_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/exception/basic.md`:161 on 2025-02-05 19:52_

Ahhhhh, it's actually something slightly different: the previous `try` statement has a `raise`, which we treat the same as `return` — and so we think the entire second `try` statement is unreachable!  If you reverse the order of the two, it's always the second one that has the new error.

---

_@dcreager reviewed on 2025-02-05 19:57_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/exception/basic.md`:161 on 2025-02-05 19:57_

And because the earlier example's `except` clause also has a `raise`, I think that's the correct interpretation.  I've wrapped all of the `try` statements in this test in functions so each statement can't affect the reachability of later ones.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/exception/basic.md`:1 on 2025-02-05 20:00_

The changes in this file are easier to see with "Hide whitespace"

---

_@dcreager reviewed on 2025-02-05 20:00_

---

_@dcreager reviewed on 2025-02-05 20:08_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:646 on 2025-02-05 20:08_

> Makes sense. I would put a TODO on that `unresolved-reference` diagnostic below.

Done

> I'm a little worried about the difficulty of implementing "more useful" options for checking unreachable code, but we can leave that as a separate problem.

Are you worried that this PR makes it more difficult?  Or just that it's on the list of things to tackle sooner rather than later in case it requires large changes to the design?

---

_@carljm reviewed on 2025-02-05 20:15_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:646 on 2025-02-05 20:15_

It's not really so much that this PR makes it more difficult, just that I'm not sure how much this approach will end up having to change. I don't think it's a reason not to merge this. I am curious if you have a rough sense of how we might go about implementing "check unreachable code as if it were reachable" while still preserving (as I think we must) "unreachable branches never merge back to outer control flow". That is, fixing the TODO you just added, without having that unreachable assignment become visible in the outer flow.

---

_Comment by @dcreager on 2025-02-05 20:50_

> Two things I like doing when investigating performance issues are run `red_knot -vvv` and compare between main and my feature (e.g. by running over tomllib):
> 
> * the ingredient counts printed just before existing: Are there more or fewer ingredients?

For my future self, a `--profile=profiling` build is considered a "release" build for the purposes of our static max logging level, so you have to remove this feature to get `-vvv` to print out trace log messages:

https://github.com/astral-sh/ruff/blob/d47088c8f83f966f734c254a23073eeee123c347/crates/red_knot/Cargo.toml#L29

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:646 on 2025-02-05 21:01_

> I am curious if you have a rough sense of how we might go about implementing "check unreachable code as if it were reachable" while still preserving (as I think we must) "unreachable branches never merge back to outer control flow".

I'd say we'd either need to track multiple visibility constraints for each binding, or multiple "current flow states" — both being ways to represent the visibility that each binding has _now_, and what it would reset to at the next merge point.

But I'm also not sure that's what we'd want to implement — if we want to "check unreachable code as if it were reachable", I'm not sure that should reset at merge points.  e.g. if someone inserted a `return` statement for debugging, I don't see a difference in UX between:

```py
def _(cond: bool):
    if cond:
        x = 1
        return
        reveal_type(x)  # revealed: Literal[1]
```

and

```py
def _(cond: bool):
    if cond:
        x = 1
        return
    reveal_type(x)  # revealed: Literal[1]
```

And so if we want to treat these both the same, I'd say we'd go for an option that controls the visibility of new bindings: "always true" if we want to check unreachable code as if it were reachable, and "current reachability" if not.

---

_@dcreager reviewed on 2025-02-05 21:01_

---

_Comment by @dcreager on 2025-02-05 21:06_

> I think it's worth putting some time-boxed effort (on the scale of a few hours) into looking into the regression here, but I don't think it should block the PR; I don't see anything obviously inefficient here, and this is what we need in order to get the right semantics. It's a better use of optimization effort to look broadly for the best ROI than to focus narrowly on a specific regression.

After figuring out https://github.com/astral-sh/ruff/pull/15817#issuecomment-2637995420, I get identical ingredient counts for `main` and this feature branch:

```
1   0.059255s TRACE red_knot Counts for entire CLI run:
1red_knot_python_semantic::semantic_index::definition::Definition         7_722        7_722        7_722
1red_knot_python_semantic::semantic_index::expression::Expression         1_150        1_150        1_150
1red_knot_python_semantic::semantic_index::symbol::ScopeId                2_322        2_322        2_322
1red_knot_python_semantic::unpack::Unpack                                    21           21           21
1ruff_db::files::File                                                        76           76           76
1ruff_db::source::SourceText                                                 20           20           20
1                                                                         total     max_live         live
```

---

_@carljm reviewed on 2025-02-05 22:15_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:646 on 2025-02-05 22:15_

> I'd say we'd either need to track multiple visibility constraints for each binding, or multiple "current flow states" — both being ways to represent the visibility that each binding has _now_, and what it would reset to at the next merge point.

Yeah makes sense.

> But I'm also not sure that's what we'd want to implement — if we want to "check unreachable code as if it were reachable", I'm not sure that should reset at merge points.

Not sure either. I feel like checking unreachable code as if it were reachable is kind of an unprincipled approach that may not have a sensible and consistent semantics.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:646 on 2025-02-05 22:15_

In any case, I think the semantics implemented in this PR are a good step forward, and we should go ahead with them for now.

---

_@carljm reviewed on 2025-02-05 22:15_

---

_Merged by @dcreager on 2025-02-05 22:47_

---

_Closed by @dcreager on 2025-02-05 22:47_

---

_Branch deleted on 2025-02-05 22:47_

---

_Comment by @MichaReiser on 2025-02-06 06:49_

@dcreager can you tell me a bit more about what performance investigation you did other than comparing ingredient numbers? Did you try to compare the verbose output of watching tomllib? Did you compare two recorded profiles?

I'm asking because an 8% regression is huge, especially considering we don't even know where it's coming from. For comparison, our biggest win on the incremental benchmark is https://github.com/astral-sh/ruff/pull/15763, and it's only 15%. This PR "eats up" 50% of that improvement. My main worry is: It's hard to figure out the root cause today, but it will even be harder to win back this regression in the future if nothing obvious shows up in benchmarks or profiles today.  

---

_Comment by @MichaReiser on 2025-02-06 07:14_

I went ahead and ran `knot check --watch -vvv` locally over the tomllib project (after moving the files into a src directory ). I appended some whitespace at the end of the file once initial checking is complete, this should mimic our benchmark fairly closely. [Here's the output](https://www.diffchecker.com/nC1rZxR2/) that compares pre-terminal statement support with main.

One main finding: We now call `symbol(__bool__)` way more often. 

---

_Comment by @MichaReiser on 2025-02-06 07:45_

One thing I noticed is that the following stack only shows up in the new version, suggesting that `UseDefMapBuilder::snapshot` has to do more heap allocation because a small vec spills to the stack more often? This could make sense, considering that we're pushing now more `visibility_constraints` (at least, not always TRUE)

<img width="1359" alt="Screenshot 2025-02-06 at 08 41 11" src="https://github.com/user-attachments/assets/74656a97-8300-4fba-be85-cf24b5c04409" />

* [pre-terminal-statement](https://share.firefox.dev/4jXBi0y)
* [main](https://share.firefox.dev/4hppdzv)

You have to select the small "peak" around second 3 or 5. Everything else is just me being slow to manually make an edit in the file


---

_Comment by @dcreager on 2025-02-06 13:33_

> can you tell me a bit more about what performance investigation you did other than comparing ingredient numbers? Did you try to compare the verbose output of watching tomllib? Did you compare two recorded profiles?

I ran the benchmark test under valgrind and used the [`crabgrind`](https://docs.rs/crabgrind/latest/crabgrind/) crate to only instrument the incremental type-check call.  I can post the stack traces I got, but I'll have to recompile and recollect to do that.  It showed some small differences in the amount of time spent in salsa internals, which is why I focused on the ingredient counts per Carl's hypothesis.

I also had added some `printf`s to spit out the number of visibility constraints that were created inside of each `UseDefMapBuilder`, and verified that those counts were the same before/after as well.

> One thing I noticed is that the following stack only shows up in the new version, suggesting that `UseDefMapBuilder::snapshot` has to do more heap allocation because a small vec spills to the stack more often? This could make sense, considering that we're pushing now more `visibility_constraints` (at least, not always TRUE)

That looks like an `IndexVec` being cloned, not a `SmallVec`.  We use an `IndexVec` to hold all of the visibility constraints that we create while building the use-def map.  But per above, I did confirm that we're not creating any new visibility constraints with this PR — I was able to piggy-back on the `scope_start_visibility` constraint that we were already collecting.  There's a `SmallVec` that records a visibility constraint for each binding, but that shouldn't be larger since we aren't introducing any new bindings.

Could this be sampling bias due to `perf` taking stack frame snapshots periodically?  Incremental checking doesn't take long in absolute terms, so it seems like it might be more susceptible to that.

> One main finding: We now call `symbol(__bool__)` way more often.

That suggests that the cause might be [this change](https://github.com/astral-sh/ruff/pull/15817/files#r1941839502) — we might be recording more complex visibility constraints for a noticeable number of bindings, which would take more time to evaluate than the `AlwaysTrue` that we were recording before.  And those would necessarily have some kind of `Expression` inside of them that we would have to type-check.  Though I would have thought that would show up as a noticeable increase in the time spent in `VisibilityConstraints::evaluate`.

---

_Comment by @MichaReiser on 2025-02-06 13:47_

Thanks for the extra explanation. It does show that you spent a fair amount of time investigating! Thanks for doing that.

> we might be recording more complex visibility constraints for a noticeable number of bindings, which would take more time to evaluate than the AlwaysTrue that we were recording before. And those would necessarily have some kind of Expression inside of them that we would have to type-check. T

I think that could partially explain the regression. It means that the queries evaluating visibility constraints have more dependencies and marking each dependency as "green" is a non-zero cost. 

---

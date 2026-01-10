```yaml
number: 13922
title: "[red-knot] Remove lint-phase"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/remove-lint
created_at: 2024-10-25T08:50:40Z
updated_at: 2024-10-28T08:39:31Z
url: https://github.com/astral-sh/ruff/pull/13922
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] Remove lint-phase

---

_Pull request opened by @MichaReiser on 2024-10-25 08:50_

## Summary

The lint rules are very verbose and distracting when running the CLI and our short term goal is to build a standalone type checker, not a type aware linter. 

That's why I suggest removing the linting phase for now.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-10-25 08:50_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-25 08:50_

---

_Label `red-knot` added by @MichaReiser on 2024-10-25 08:50_

---

_Comment by @codspeed-hq[bot] on 2024-10-25 09:20_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha/remove-lint)

### Merging #13922 will **improve performances by 4.39%**

<sub>Comparing <code>micha/remove-lint</code> (09b739b) with <code>main</code> (5eb87aa)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `micha/remove-lint` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `red_knot_check_file[incremental]` | 4.8 ms | 4.6 ms | +4.39% |


---

_@AlexWaygood reviewed on 2024-10-25 09:22_

---

_Review comment by @AlexWaygood on `crates/red_knot_workspace/src/workspace.rs`:548 on 2024-10-25 09:22_

```suggestion
    fn check_file_skips_type_checking_when_file_cant_be_read() -> ruff_db::system::Result<()> {
```

---

_@AlexWaygood reviewed on 2024-10-25 09:26_

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/red_knot.rs`:60 on 2024-10-25 09:26_

I agree that the lint rules complaining about double quotes and lines being too long just add noise and distraction. But the lint rule complaining about possibly undefined variables seems like a classic case of a "lint" which is core to a type checker's purpose: if we don't emit these lint diagnostics, we can't say for sure that the program is type-safe (and if we're not capable of doing this analysis significantly better than Ruff's current semantic model, we've failed).

Moreover, without these lint rules being emitted, I think we're losing valuable signal about the accuracy of our control-flow analysis

---

_Comment by @github-actions[bot] on 2024-10-25 09:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-10-25 09:31_

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/red_knot.rs`:60 on 2024-10-25 09:31_

Here, for example, these diagnostics indicate a serious deficiency in our control-flow analysis for exception handlers due to the fact that we don't yet understand terminal statements. That's why it's one of my monthly goals to work on that!

---

_@MichaReiser reviewed on 2024-10-25 10:33_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/red_knot.rs`:60 on 2024-10-25 10:33_

I agree that this rule is useful and helps highlight deficiencies in our control flow analysis. However, I think it should be implemented as part of the type checker rather than as a separate linter phase. 

---

_@AlexWaygood reviewed on 2024-10-25 10:44_

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/red_knot.rs`:60 on 2024-10-25 10:44_

Okay, that makes sense.

I would prefer to reimplement the rule as part of the type checker in this PR: otherwise we'll have to just add these entries back to the "expected diagnostics" list in the followup PR when we reimplement the rule. Doing it as part of the same PR reduces churn in this file. It should also help us see whether the performance improvement currently being reported is due to removing the lint phase or due to removing this rule.

The rule is also very simple; it shouldn't be hard to reimplement it in this PR (just a matter of copying a couple of lines that we currently have in `crates/red_knot_workspace/src/lint.rs` to `crates/red_knot_python_semantic/src/types/infer.rs`).

On the other hand, if you _do_ get rid of the rule entirely in this PR, you also need to get rid of the comment on line 26 :-)

---

_@MichaReiser reviewed on 2024-10-25 10:57_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/red_knot.rs`:60 on 2024-10-25 10:57_

I'm not very eager on implementing the rule because i worry that you'll ask me to add tests ;) but I did so anyway

---

_@MichaReiser reviewed on 2024-10-25 10:57_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/red_knot.rs`:60 on 2024-10-25 10:57_

Oh god, this breaks so many mdtests

---

_@MichaReiser reviewed on 2024-10-25 11:01_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/red_knot.rs`:60 on 2024-10-25 11:01_

Okay, moving this into `infer` is more work than I currently plan spending on this because it breaks many mdtests that use undefined variables. 

and just doing in `infer.rs` is too naive

```
                let ty = bindings_ty(self.db, definitions, unbound_ty);

                if ty.is_unbound() {
                    self.add_diagnostic(
                        name.into(),
                        "unresolved-reference",
                        format_args!("Name `{}` used when not defined", id),
                    );
                } else if ty.may_be_unbound(self.db) {
                    self.add_diagnostic(
                        name.into(),
                        "unresolved-reference",
                        format_args!("Name `{}` used when possibly not defined", id),
                    );
                }

                ty
```

We can either close this PR, or create an issue to implement said rule in `infer` and do all the follow up work

---

_@AlexWaygood reviewed on 2024-10-25 11:56_

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/red_knot.rs`:60 on 2024-10-25 11:56_

This illustrates to me that it's very important that we make this change soon! It makes sense to me that we should do this lint in the type-checking phase, and that errors from this lint should be reported in our mdtests. (If we _don't_ report this lint in our mdtests, it opens up the possibility of us not testing what we wanted to test, because a variable in an mdtest might be undefined when we didn't want it to be.)

So I think we'll have to update our mdtests to make them resilient to this rule at some point. And that means that we should make the change sooner rather than later (because the longer we defer it, the more mdtests we'll have to update when we do make the change).

If you're okay with me pushing to your branch, I'd be happy to do the busywork here of moving the rule to `infer.rs` and updating the mdtests.

---

_@MichaReiser reviewed on 2024-10-25 12:00_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/red_knot.rs`:60 on 2024-10-25 12:00_

> If you're okay with me pushing to your branch, I'd be happy to do the busywork here of moving the rule to infer.rs and updating the mdtests.

Sure, feel free to go ahead (generally speaking, I don't mind other people pushing to my branch)

---

_@carljm reviewed on 2024-10-25 13:52_

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:60 on 2024-10-25 13:52_

I assume this is what you're already planning, @AlexWaygood , but I hope that we can in most cases actually update the mdtests to not use undefined variables, rather than updating them to expect the undefined variable error. 

---

_@AlexWaygood reviewed on 2024-10-25 13:53_

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/red_knot.rs`:60 on 2024-10-25 13:53_

> I assume this is what you're already planning, @AlexWaygood , but I hope that we can in most cases actually update the mdtests to not use undefined variables, rather than updating them to expect the undefined variable error.

Yes, of course!!

---

_Comment by @AlexWaygood on 2024-10-25 17:11_

I've done the following:
- Reimplemented the rule as part of `infer.rs`
- ~~Added some logic to make sure that we don't emit the diagnostic twice on any give `ExprName` node~~ (removed, see discussion below)
- Split it into two separate error codes, `unresolved-reference` and `possibly-unresolved-reference`. This enables us to distinguish between these two cases easily in mdtest without asserting the full error message. (It's a bit of an anti-pattern to assert on the full error message too much in mdtest, because we might change the error message in the future, and that would result in us having to change lots of tests)
- Fixed our mdtest suite to pass with all these changes

---

_@AlexWaygood approved on 2024-10-25 17:22_

This LGTM, but I'd now appreciate it if somebody could take a look at the changes _I_ pushed to see if they make sense 😄

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:6 on 2024-10-25 18:11_

I think this is the best option for now, but it's a lot of boilerplate noise in a lot of tests. I look forward to rewriting these all again in the near future, once we support inferring types from parameter annotations, to use `def f(flag: bool): ...` (and put the test body inside the function) instead 😆 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2426 on 2024-10-25 18:15_

Add this to the list of things I kinda wish we'd removed Unbound _before_ doing, since it will now need to change to adapt to the removal of Unbound.

On the other hand, adding this diagnostic might also make it easier to remove Unbound, since it means that our tests that assert on unboundness or maybe-unboundness now already have the ability to keep making those assertions, even after Unbound is no longer part of their revealed type. So that's good.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:296 on 2024-10-25 18:17_

In which cases did you see duplicate diagnostics if you didn't do this?

In general it is supposed to be an invariant that we don't double-infer anything, so this _shouldn't_ be necessary. Unpacking assignment is one known counter-example currently.

If there are non-unpacking-assignment cases where you saw duplicate diagnostics, that would be interesting, as it might reveal a bug.

Unless there are so many "bugs" here that it turns out we just can't feasibly maintain this invariant, I would rather not add this and just have duplicate diagnostics in some tests, with TODO comments to fix those cases by not double-inferring the expression.

If for some reason we can't maintain the no-double-inference invariant, it means we are going to need to generalize this hashset approach to not just unbound-names, but all diagnostics.

---

_@carljm approved on 2024-10-25 18:22_

Looks good to me! My only issue is with the duplicate-diagnostics avoidance, as commented below. Unless I'm missing something, I think I'd prefer not to include that.

---

_@AlexWaygood reviewed on 2024-10-25 18:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:6 on 2024-10-25 18:23_

Yes, definitely!

---

_@AlexWaygood reviewed on 2024-10-25 18:28_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:296 on 2024-10-25 18:28_

> In which cases did you see duplicate diagnostics if you didn't do this?

Take a look at the change to the assertions in the tomllib benchmark in this PR. Both of these diagnostics were being reported twice with the lint rule as it was previously:

```
    "/src/tomllib/_parser.py:104:14: Name `char` used when possibly not defined",
    "/src/tomllib/_parser.py:115:14: Name `char` used when possibly not defined",
```

Both of these are false positives (due to the fact our control-flow analysis doesn't yet understand terminal statements), but neither appears to be anything to do with unpacking, as far as I can see:

- https://github.com/python/cpython/blob/591f6754b56cb7f6c31fce8c22528bdf0a99556c/Lib/tomllib/_parser.py#L104
- https://github.com/python/cpython/blob/591f6754b56cb7f6c31fce8c22528bdf0a99556c/Lib/tomllib/_parser.py#L115

I'm okay with keeping the duplicate diagnostics for now if you are!

---

_@AlexWaygood reviewed on 2024-10-25 18:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:296 on 2024-10-25 18:35_

Oh, but now if I try running the benchmark again without the deduplication logic, I don't see it anymore.

Hmm, okay. I think we _did_ have duplicated diagnostics when it was implemented outside of `infer.rs`, but now it's implemented as part of `infer.rs`, we don't anymore. But for some reason I thought we still did, so tried to fix it. Argh.

Anyway, you're right, we don't need any of this deduplication logic. Hooray!

---

_Merged by @AlexWaygood on 2024-10-25 18:40_

---

_Closed by @AlexWaygood on 2024-10-25 18:40_

---

_Branch deleted on 2024-10-25 18:40_

---

_@MichaReiser reviewed on 2024-10-28 08:11_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:2430 on 2024-10-28 08:11_

@AlexWaygood what's the motivation for having two different rule codes? I'm in favor of specific rules, but I'm not sure if we need those fine granular diagnostic codes.

---

_@MichaReiser reviewed on 2024-10-28 08:11_

Thank you @AlexWaygood !

---

_@AlexWaygood reviewed on 2024-10-28 08:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2430 on 2024-10-28 08:39_

Partly the motivation I gave in https://github.com/astral-sh/ruff/pull/13922#issuecomment-2438372482: it's much easier to test them robustly if we have two different codes.

In the long term, I do also think it will be better for users if these are separate codes. `unresolved-reference` should be a code that we have very high confidence in; I would expect it to be seen as a massive code smell if somebody added a `# knot: ignore[unresolved-reference]` comment in their Python code. `possibly-unresolved-reference` will by its nature have a lot more false positives, however, so ignoring the error might sometimes be reasonable.

(For example: a variable might be defined in the body of a loop. From red-knot's perspective, all loops could potentially have zero iterations if the loop loops over an empty sequence — so variables defined as `T` in a loop are always inferred as being `Unbound | T` by red-knot from the perspective of code following the loop. But sometimes the user knows for sure that a certain loop will always have >=1 iterations, meaning they can be certain that the variable is defined after the loop even if red-knot can't be. In that situation, it might be reasonable to suppress the `possibly-unresolved-reference` error that red-knot emits.)

---

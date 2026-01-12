```yaml
number: 10863
title: Fix docs and add overlap test for negated per-file-ignores 
type: pull_request
state: merged
author: carljm
labels:
  - internal
assignees: []
merged: true
base: main
head: cjm/select2
created_at: 2024-04-10T18:23:03Z
updated_at: 2024-04-12T13:31:10Z
url: https://github.com/astral-sh/ruff/pull/10863
synced_at: 2026-01-12T15:55:33Z
```

# Fix docs and add overlap test for negated per-file-ignores 

---

_@carljm_

Refs #3172 

## Summary

Fix a typo in the docs example, and add a test for the case where a negative pattern and a positive pattern overlap.

The behavior here is simple: patterns (positive or negative) are always additive if they hit (i.e. match for a positive pattern, don't match for a negated pattern). We never "un-ignore" previously-ignored rules based on a pattern (positive or negative) failing to hit.

It's simple enough that I don't really see other cases we need to add tests for (the tests we have cover all branches in the ignores_from_path function that implements the core logic), but open to reviewer feedback.

I also didn't end up changing the docs to explain this more, because I think they are accurate as written and don't wrongly imply any more complex behavior. Open to reviewer feedback on this as well!

After some discussion, I think allowing negative patterns to un-ignore rules is too confusing and easy to get wrong; if we need that, we should add `per-file-selects` instead.

## Test Plan

Test/docs only change; tests pass, docs render and look right.

---

_Label `internal` added by @carljm on 2024-04-10 18:23_

---

_Review requested from @BurntSushi by @carljm on 2024-04-10 18:23_

---

_Review comment by @BurntSushi on `crates/ruff/tests/lint.rs`:1252 on 2024-04-10 18:39_

Is this something we are locked into? I can imagine this being somewhat unintuitive since things like `.gitignore` lets you un-ignore things.

---

_@BurntSushi reviewed on 2024-04-10 18:39_

---

_Comment by @github-actions[bot] on 2024-04-10 19:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm reviewed on 2024-04-10 19:15_

---

_Review comment by @carljm on `crates/ruff/tests/lint.rs`:1252 on 2024-04-10 19:15_

Thanks for asking the question! I'm not set on this behavior, just was favoring the simplest option to understand, unless we had a clear need for something more complex. But it's true that it will be hard to change this once shipped, so it's good to think through what we prefer now.

The main difference I see between this case and gitignore is that a negative pattern in gitignore serves exactly one purpose: to un-ignore whatever it matches. It doesn't also mean "ignore everything that doesn't match this pattern."

In our case we do want "ignore everything that doesn't match this pattern" for negative patterns, so there's more risk of overloading the meaning if we also add "un-ignore everything that does match this pattern."

For instance, here's a case to consider:

```
# ignore D rules in all __init__.py files
"__init__.py" = ["D"]
# ignore D rules everywhere outside src/
# - does this also mean "actually, don't ignore in __init__.py files inside src/"?
"!src/**.py" = ["D"]
```

I'm not sure which behavior is more intuitive for this example. But if we did add un-ignoring, and you did want to ignore in all `__init__.py` files inside `src/` too, you can always just switch the order of the two patterns; the rule would be "last one wins." I think between this and the option of adding another more-specific positive pattern after the negative one, there shouldn't be any cases you can't express at all, if we add this.

Initially it seemed odd to me that a negative pattern would un-ignore on failure to hit, while a positive pattern wouldn't do that, but after thinking through some examples, I think that behavior works out OK; for a negative pattern the no-hit cases are more narrowly targeted, so it's more intuitive to un-ignore in that case.

I guess my last concern: is it too confusing if a negative pattern in `extend-per-file-ignores` (in a different config location from `per-file-ignores`) overrides some of what's specified in `per-file-ignores`? (This isn't possible today, since everything is just additive.) Or is this fine?

cc @charliermarsh @zanieb for opinions also

---

_@BurntSushi reviewed on 2024-04-11 11:57_

---

_Review comment by @BurntSushi on `crates/ruff/tests/lint.rs`:1252 on 2024-04-11 11:57_

The typical way I like to do filtering for whitelist/blacklist situations like this is as follows:

* Every rule is either a whitelist or a blacklist rule.
* The rules are ordered. Later rules take priority over earlier rules.
* When there are 1 or more whitelist rules, then in order for a subject to pass the filter, the last matching rule must be a whitelist rule.
* When there are 0 whitelist rules, a subject passes the filter when it matches 0 blacklist rules.
* Otherwise, the filter rejects the subject.

I find that these semantics overall provide a lot of flexibility. But you're right that gitignore doesn't neatly map on to this model. The other complication that seems present here is that the rules aren't just ignore/not-ignore, but they also come with lint rules associated with the filter. I haven't thought through fully how that might impact things.

I think the model above overall matches what you're saying around adding un-ignoring, particularly with the last rule matching being the winner. I think I overall personally like this style. I don't have a good answer for what to do about `extend-per-file-ignores` though.

---

_Renamed from "Add test and fix doc for negated per-file-ignores" to "Allow negated per-file-ignore patterns to un-ignore rules" by @carljm on 2024-04-11 20:08_

---

_Review requested from @BurntSushi by @carljm on 2024-04-11 20:12_

---

_Review comment by @carljm on `crates/ruff/tests/lint.rs`:1252 on 2024-04-11 20:35_

I think the fact that we're not just making a pass/fail ruling for a subject, but building up an ignore-ruleset for the subject, means that the algorithm you outlined doesn't quite work here. Specifically the part that doesn't work (or at least has to be more complex) is "a negative rule can only cause a subject to pass the filter if there are no positive rules." The problem is illustrated by a case like this:

```
"__init__.py" = ["F401"]
"!src/**/*.py" = ["E"]
```

IMO the intent here is clearly that `F401` is ignored in all `__init__.py` files, whereas `E` is ignored in all files outside the `src/` directory. But since there is both a positive pattern and a negative pattern present, according to the algorithm you listed, the negative rule here would be a no-op.

We could try a more complex version of it, where "1 or more positive patterns" and "0 positive patterns" are replaced by "1 or more positive patterns with overlapping ruleset" and "0 positive patterns with overlapping ruleset." But I think this is too subtle.

What I've pushed here is instead a variant of your algorithm. It's still last-one-wins where there are conflicts, but a negative pattern always applies in both directions: it always adds ignores to anything that doesn't match, and removes them from anything that does.

Let me know what you think.

---

_@carljm reviewed on 2024-04-11 20:35_

---

_@carljm reviewed on 2024-04-11 21:28_

---

_Review comment by @carljm on `crates/ruff/tests/lint.rs`:1252 on 2024-04-11 21:28_

After more discussion off-line, it feels like allowing un-ignores is too unpredictable. The case that seems like the most obvious for un-ignores is something like this:

```
[lint.per-file-ignores]
"src/**/*.py" = "[X]"
"!src/foo/bar/**/*.py" = "[X]"
```

But even that is subtly wrong, because it doesn't just make a carve-out from the prior rule, it ends up ignoring `X` _everywhere_ outside `src/foo/bar`, rendering the first rule redundant.

I'm going to roll back this PR to just test the existing purely-additive behavior (which we actually already released this morning); my suspicion is that if we want per-file selects it will be a lot clearer to add a `per-file-selects` config. But we should have more discussion and do that in a separate PR.

---

_Renamed from "Allow negated per-file-ignore patterns to un-ignore rules" to "Fix docs and add overlap test for negated per-file-ignores " by @carljm on 2024-04-11 22:12_

---

_Review requested from @charliermarsh by @carljm on 2024-04-11 22:13_

---

_Comment by @carljm on 2024-04-12 01:30_

Going to go ahead and merge this without further review; we should get the doc fix out, and this is just a minor doc fix and a test that passes.

This doesn't preclude revisiting the behavior of negated patterns, but that can be another PR.

---

_Merged by @carljm on 2024-04-12 01:30_

---

_Closed by @carljm on 2024-04-12 01:30_

---

_Branch deleted on 2024-04-12 01:30_

---

_@charliermarsh reviewed on 2024-04-12 01:35_

👍  Good call, sorry for the delay.

---

_@BurntSushi reviewed on 2024-04-12 13:29_

LGTM

---

_@BurntSushi reviewed on 2024-04-12 13:31_

---

_Review comment by @BurntSushi on `crates/ruff/tests/lint.rs`:1252 on 2024-04-12 13:31_

SGTM. Thanks for thinking through this. :)

---

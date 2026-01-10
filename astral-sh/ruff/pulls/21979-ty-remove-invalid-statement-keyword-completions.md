```yaml
number: 21979
title: "[ty] Remove invalid statement-keyword completions in for-statements"
type: pull_request
state: merged
author: RasmusNygren
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: filter-incorrect-keyword-completions
created_at: 2025-12-14T19:15:14Z
updated_at: 2025-12-15T18:27:36Z
url: https://github.com/astral-sh/ruff/pull/21979
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Remove invalid statement-keyword completions in for-statements

---

_Pull request opened by @RasmusNygren on 2025-12-14 19:15_

In `for x in <CURSOR>` statements it's only valid to provide expressions that eventually evaluate to an iterable. While it's extremely difficult to know if something can evaulate to an iterable in a general case, there are some suggestions we know can never lead to an iterable. Most keywords are such and hence we remove them here.

## Summary
This suppresses statement-keywords from auto-complete suggestions in `for x in <CURSOR>` statements where we know they can never be valid, as whatever is typed has to (at some point) evaluate to an iterable.

It handles the core issue from https://github.com/astral-sh/ty/issues/1774 but there's a lot of related cases that probably has to be handled piece-wise.

## Test Plan
New tests and verifying in the playground.


---

_Comment by @astral-sh-bot[bot] on 2025-12-14 19:17_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-14 19:19_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1218:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5136 diagnostics
+ Found 5137 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `server` added by @AlexWaygood on 2025-12-14 19:25_

---

_Label `ty` added by @AlexWaygood on 2025-12-14 19:25_

---

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:480 on 2025-12-14 20:08_

It might make more sense to make `add_keyword_completions` context-aware and avoid adding the incorrect completions to begin with rather than pruning them here after the fact.

---

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:1655 on 2025-12-14 20:10_

It probably makes sense here to make `find_typed_text` return a struct that contains both the text and the range so we don't have to make these computations again from the bare string.

---

_@RasmusNygren reviewed on 2025-12-14 20:11_

---

_Marked ready for review by @RasmusNygren on 2025-12-14 21:57_

---

_Review requested from @carljm by @RasmusNygren on 2025-12-14 21:57_

---

_Review requested from @MichaReiser by @RasmusNygren on 2025-12-14 21:57_

---

_Review requested from @AlexWaygood by @RasmusNygren on 2025-12-14 21:57_

---

_Review requested from @sharkdp by @RasmusNygren on 2025-12-14 21:57_

---

_Review requested from @dcreager by @RasmusNygren on 2025-12-14 21:57_

---

_Review requested from @BurntSushi by @AlexWaygood on 2025-12-14 21:59_

---

_Review request for @dcreager removed by @MichaReiser on 2025-12-15 07:39_

---

_Review request for @carljm removed by @MichaReiser on 2025-12-15 07:39_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-12-15 07:39_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1655 on 2025-12-15 16:54_

Yeah I think that's a good idea. Doesn't have to be in this PR though.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1648 on 2025-12-15 16:58_

I remain slightly concerned about adding more and more of these "do we match this AST pattern?" checks. I think it's fine for now, but:

1. I think eventually this is going to impact perf.
2. The "check this case, then this case, and then this case" approach already buckled under its own weight for imports. Now we have a "look at the stuff we have _once_, and classify our case based on that."

I don't think there's any action item here right now. Mostly just voicing my thoughts.

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:470 on 2025-12-15 17:01_

I guess we don't technically have to call this unless we know we've added some keywords to the completions. And that only happens for scope based completions. So this will do an unnecessary AST check for `object.attr` and import completions.

But maybe we'll eventually rule out other things here besides keywords that we know can't be iterable. So I'm not sure it's worth trying to optimize this out?

I think part of my problem is that I don't have an intuition for the costs associated with calling `covering_node`. Maybe I'm tilting at windmills.

---

_@BurntSushi approved on 2025-12-15 17:02_

Thank you! I think this LGTM as-is. I left some comments, but I'm not sure there's anything actionable from them right now.

---

_@BurntSushi reviewed on 2025-12-15 17:08_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:480 on 2025-12-15 17:08_

Yeah I'm not quite sure what the right architecture ought to be here. I suppose we could end up ruling out other completions as well.

Maybe having the completion collector "know" what the context is, and then its add methods could choose to ignore completions added that are incompatible with that context. But I think that's a bigger refactor.

---

_Merged by @BurntSushi on 2025-12-15 17:56_

---

_Closed by @BurntSushi on 2025-12-15 17:56_

---

_@RasmusNygren reviewed on 2025-12-15 18:27_

---

_Review comment by @RasmusNygren on `crates/ty_ide/src/completion.rs`:1648 on 2025-12-15 18:27_

Gathering all my thoughts here instead of responding in the various threads.

I share your concerns and in theory I like the approach of building a context (before adding any completions) that has enough information such that we can later determine what completions should be added or ignored. Ignoring that this would be a big refactor, what slightly concerns me with that approach is that the size and the amount of information we would need to capture in the context could get out of hand, in order to support all the different cases where we might want to add or suppress different suggestions. So in practice I'm not quite sure how you would architect that to make it intuitive to work with.

To the extent it's doable, I do agree that we at the very least should stick to something like a single token-based pruning loop and a single AST-traversal pruning loop (I don't see how we can get away without both as long as we do any pruning at all) that either mutate completions in-place or builds some list of predicate functions that can be used to filter completions. This at least gets us away from a whole bunch of individual checks that all do their own AST traversal to remove bad suggestions. I'm not sure this would be quite as simple to do when we add new completions though (without bigger refactors), as that usually requires more work.

At some point a refactor in some direction is probably due though to make this scale nicer (both in terms of performance and readability), but to me it's not obvious what the right refactor to do here is.

---

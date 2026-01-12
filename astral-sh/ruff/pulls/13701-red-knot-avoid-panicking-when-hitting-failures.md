```yaml
number: 13701
title: "[red-knot] Avoid panicking when hitting failures looking up AST information"
type: pull_request
state: closed
author: rtpg
labels:
  - ty
assignees: []
draft: true
base: main
head: never-panic
created_at: 2024-10-10T06:59:05Z
updated_at: 2024-11-14T10:14:40Z
url: https://github.com/astral-sh/ruff/pull/13701
synced_at: 2026-01-12T15:55:45Z
```

# [red-knot] Avoid panicking when hitting failures looking up AST information

---

_@rtpg_

This is a set of changes that allow for `cargo --bin red_knot` to not panic (though report _many_ failures across the repo).

The main idea here is to avoid panicking if, instead, type inference can choose to have "less information". For example, if some entry is not found, we often can return `Unknown`. In a stable system this can lead to `Unknown` spreading in a nasty way, but here this could allow for easier post-mortem debugging (for example, querying the salsa DB ad-hoc to figure out _why_ certain expectations didn't hold).

I tried adding `tracing` logs in places that felt like indications of real failures upstream (in particular partial AST traversal). I'm not well versed in `tracing`, but in Python land I would probably group all of these into a specific logger so that they can be treated as "road to 1.0" stuff.

In this process I changed some ID lookup methods to return `Option`. I believe that the goal here is that this wouldn't be needed (as the inference DB should cover "everything"), but the changeset is so small that it feels worthwhile until AST failures are inbound.

Part of the failures I found were "invalid AST but still constructed by Ruff"-style errors.

An example is `x, y: int = 1, 2`. 

red_knot sees an annotated assignment, and so figures out the types of `1` and `2`, but then trying to assign `1` and `2` to the left side (as a tuple), but an awkward interaction between annotated assignments and tuple assignments (like `x, y = 1, 2`) leads to `x, y` getting defined by `1` and `2`. This hits the "shouldn't have more than one definition for an expression" problem.

Ultimately this is a syntax error to begin with, so then we probably shouldn't traverse this. But if we don't traverse it and "try our best" with the above, suddenly `x` and `y` don't have anything associated.

I think the "right thing" to do above is to try harder (for example, erase the signature in the type inference treatment). But I think this branch shows that the cost of trying to keep the system running instead doesn't have too high of a cost.

In any case, this branch is hopefully an indicator of what kinds of places in the code are triggering panics when running `red_knot` against the `ruff` repo (as artificial of an example that might be).



---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:217 on 2024-10-10 07:00_

There are a bunch of warnings I added just to try and get things working.

In total there are 228 warnings (of which this one is only hit 5 times), so it's pretty surmountable all things considered

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:584 on 2024-10-10 07:07_

if we visit with an invalid target we get downstream errors on re-defining things. There's likely a way to visit here with an invalid target that doesn't generate issues

---

_@MichaReiser reviewed on 2024-10-10 07:08_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:896 on 2024-10-10 07:08_

We limit the use of `tracing::warn` for messages that need the user's attention and they need to address (e.g. when setting up file watching for a directory failed). Here, there's not much a user can do about this and invalid syntax is very common in an LSP. Logging a warning would be very noisy for users. We should use either the `debug` or possibly even `trace` level instead.

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/semantic_model.rs`:66 on 2024-10-10 07:08_

I feel like it would make a lot of sense for there to be something like `Type::Unknown(SourcingInformation)`, where each callsite could embed where that specific one came from.

 That way, if `reveal_type` gives you an `Any`, you could probably trace back why you got that.

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/types/infer.rs`:101 on 2024-10-10 07:09_

copied what `infer_definition_types` did here.

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/types/infer.rs`:216 on 2024-10-10 07:10_

here I went with leaving only `try_expression_ty`, other places I `Option`'d the existing methods. Not sure what pattern makes sense (if any)

---

_Comment by @MichaReiser on 2024-10-10 07:10_

> In this process I changed some ID lookup methods to return Option. I believe that the goal here is that this wouldn't be needed (as the inference DB should cover "everything"), but the changeset is so small that it feels worthwhile until AST failures are inbound.

Could you talk me through the benefit of returning an `Option` in more detail? To me it's not clear how panicking at the call side is much different than panicking in the e.g. `expression` method directly because the call site is visible in the stack frame. 

---

_Review comment by @rtpg on `crates/red_knot_python_semantic/src/types/infer.rs`:3085 on 2024-10-10 07:10_

another place where the failure is around an invalid AST

---

_@rtpg reviewed on 2024-10-10 07:10_

---

_Comment by @github-actions[bot] on 2024-10-10 07:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @rtpg on 2024-10-10 07:48_

@MichaReiser the main point of using Option is that almost all of the call sites end up having a good answer for what to do if they couldn’t find the ID (often, in inference, say “well I guess the type is unknown”).  And in cases where you don’t have a good answer, there’s still the fallback option of panicking. 

The core thing with panicking is you lose all context except the backtrace, right? And stuff gets torn down etc. Meanwhile if you power through as much as possible you can have a partially filled database, and then do something like dump everything that seems missing during a debug session.

I think that pairing this with things like Type::Todo feels like a comfortable strategy up until there’s stability. And panicking due to inference failures means that big reports are “this crashes on my codebase” rather than “this code reports the type as Any”. 

 But maybe everything I’m seeing is all solvable by a handful of changes (in which case “fail hard” will make sure that the DB doesn’t get filled up with spurious inference failures in the future). 

---

_Label `red-knot` added by @AlexWaygood on 2024-10-10 19:48_

---

_Comment by @carljm on 2024-10-16 16:10_

This is very useful, thanks for putting it up! I've created https://github.com/astral-sh/ruff/issues/13778 to track the overall issue of error resilience.

I don't think I want to move forward with landing this exact approach. I'd like to take this project in a more incremental fashion, fixing one cause of panics at a time (so we can consider the tradeoffs in each case, and look at relevant code samples), and ensuring we add a test case (not just "this Python file exists in the ruff repo", but an actual test case that runs in CI) for each case that we fix.

But regardless this PR is a great resource and reference point to have on hand for that work.

---

_Comment by @carljm on 2024-10-16 16:11_

I'm also aware that some issues addressed in this PR might be bugs on valid syntax, not panics on invalid syntax; those should also be addressed in separate PRs.

---

_Comment by @sharkdp on 2024-11-14 10:14_

I'm tentatively closing this with the same reasoning as in https://github.com/astral-sh/ruff/issues/13710#issuecomment-2475926923. Please feel free to comment here if you feel that something from this branch should be integrated into red knot.

---

_Closed by @sharkdp on 2024-11-14 10:14_

---

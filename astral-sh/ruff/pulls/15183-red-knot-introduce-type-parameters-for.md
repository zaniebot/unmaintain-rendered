```yaml
number: 15183
title: "[red-knot] Introduce type parameters for InstanceType"
type: pull_request
state: closed
author: pcmoritz
labels:
  - ty
assignees: []
base: main
head: type-parameters
created_at: 2024-12-30T00:33:06Z
updated_at: 2025-01-26T14:36:47Z
url: https://github.com/astral-sh/ruff/pull/15183
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Introduce type parameters for InstanceType

---

_Pull request opened by @pcmoritz on 2024-12-30 00:33_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR introduces type parameters to InstanceType, so an instance of type like `dict[str, str]` can be represented.

I'm curious for feedback if this is the right way to do it or if there is a better way!

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @carljm by @pcmoritz on 2024-12-30 00:33_

---

_Review requested from @MichaReiser by @pcmoritz on 2024-12-30 00:33_

---

_Review requested from @AlexWaygood by @pcmoritz on 2024-12-30 00:33_

---

_Review requested from @sharkdp by @pcmoritz on 2024-12-30 00:33_

---

_Comment by @github-actions[bot] on 2024-12-30 00:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @MichaReiser on 2024-12-30 08:09_

---

_Comment by @AlexWaygood on 2025-01-05 17:23_

Hey, thanks for this PR! Sorry for the delay in getting back to you on this -- several people on the team were on holiday between Christmas and new year's day :-)

Unfortunately, how best to represent generic type parameters and generic type arguments is quite a big topic that requires a certain amount of care. @carljm has written up a plan for how to do this which has been shared and discussed internally, and it will require a fair amount of refactoring to get all the details right. This is because we need to make sure that e.g. an error is emitted if a user uses `dict[str, int, bytes]` in an annotation (`dict` requires exactly two type arguments).

Additionally, it's quite hard for us to test our implementation of generics until we properly implement attribute access on `Type::Instance` types. This is another huge topic, and one that @sharkdp is planning on tackling -- but it probably needs to be done before generics, unfortunately!

All in all, I think it's probably unlikely that we'll merge this, sadly. It's not a _great_ task for a contributor at the end of the day -- but we have lots of red-knot issues with the "help wanted" label open at the moment; I'd encourage you to take a look at those and see if any take your fancy!

---

_Comment by @carljm on 2025-01-05 19:32_

Thanks for your work on this! As @AlexWaygood suggests, this isn't quite the direction we'll want to take. Specifically, we'll want to represent a "specialized" generic class (that is, a generic class with its type parameters filled in with type arguments) at a layer below this: roughly, a new layer of struct wrapping `Class` and wrapped by `InstanceType`. This is because a specialized generic class can also occur in other places where we currently have a `Class`: not only `InstanceType`, but also `SubclassOfType`, `ClassBase::Class`, and maybe others I'm forgetting at the moment.

Although this isn't the topic I'd have suggested for a first contribution to red-knot (because it will be an invasive change), it's promising that you carried this initial approach this far successfully! If you're highly motivated to work on this contribution, and willing to likely go through several review iterations to get there, I'd be open to sharing the internal design doc with you (which fleshes out the above sketch with specific struct names and more details) and working with you on it as reviewer. Let me know!

---

_Comment by @AlexWaygood on 2025-01-26 14:36_

@pcmoritz, I'm going to close this for now to keep our list of open PRs relatively tidy, since (for the reasons discussed above) I think this probably isn't the way we want to go exactly, and since there are now heavy merge conflicts here anyway. But please do say if you'd still like to help out with adding support for generics! It would be great to have you on board, and we'd be very happy to share our internal design proposal with you :-)

---

_Closed by @AlexWaygood on 2025-01-26 14:36_

---

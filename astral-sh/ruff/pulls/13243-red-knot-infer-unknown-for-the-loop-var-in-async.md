```yaml
number: 13243
title: "[red-knot] Infer `Unknown` for the loop var in `async for` loops"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/redknot-async-for
created_at: 2024-09-04T13:32:50Z
updated_at: 2024-09-04T15:51:10Z
url: https://github.com/astral-sh/ruff/pull/13243
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] Infer `Unknown` for the loop var in `async for` loops

---

_@AlexWaygood_

## Summary

`async for` loops use the asynchronous iteration protocol (involving `__aiter__` and `__anext__`) rather than the synchronous iteration protocol (involving `__iter__` and `__next__`). Understanding `__anext__` calls is beyond our capabilities until we can understanding async call expressions (which is probably blocked on generic coroutine types). For now, inferring `Unknown` for loop vars in `async for` loops is more accurate than erroneously using the synchronous iteration protocol for the types of these definitions.

## Test Plan

`cargo test -p red_knot_python_semantic --lib`


---

_Label `red-knot` added by @AlexWaygood on 2024-09-04 13:32_

---

_Review requested from @carljm by @AlexWaygood on 2024-09-04 13:32_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-09-04 13:32_

---

_@MichaReiser approved on 2024-09-04 13:43_

---

_Comment by @github-actions[bot] on 2024-09-04 13:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @AlexWaygood on 2024-09-04 14:24_

---

_Closed by @AlexWaygood on 2024-09-04 14:24_

---

_Branch deleted on 2024-09-04 14:24_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1060 on 2024-09-04 15:29_

I haven't brought this up in reviews before, but I would prefer if we don't tag our TODOs with a name. The TODOs belong to the project, and anyone might address them in future. I realize it's intended as a way to take responsibility for the issue, but I think in practice it functions more like cookie-licking (https://communitymgt.fandom.com/wiki/Cookie_Licking), discouraging anyone else from contributing an improvement, while the person who created the TODO may not have time for it yet either.

cc @dhruvmanila 

---

_@carljm reviewed on 2024-09-04 15:32_

---

_@AlexWaygood reviewed on 2024-09-04 15:36_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1060 on 2024-09-04 15:36_

Heh. Dhruv just this morning told me via DM that we were encouraged to do this at Astral :) he linked [this](https://gist.github.com/dmnd/ed5d8ef8de2e4cfea174bd5dafcda382#whats-the-point-of-putting-your-name-on-a-todo) by way of explanation

I don't have a strong preference either way!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1060 on 2024-09-04 15:42_

Interesting. I see that rationale, and I am more OK with it if the interpretation is more "this person might have relevant context" and not "you shouldn't work on this unless you check with this person first." But I'm still concerned that the non-cookie-licking interpretation isn't necessarily clear to a new contributor reading the code.

Overall I still think I don't like it. If there's relevant context that's needed to understand the TODO, I'd rather that be in the comment with the TODO, rather than a person's name to go talk to. The case where you really need to go talk to a specific person to understand some code shouldn't be _that_ common, if we are writing reasonably understandable and well-commented code, and when that case arises, `git blame` exists. (That case can just as well arise for changes in an area of the code that doesn't have a TODO comment, so should we just comment every piece of code we write with our name, because `git blame` is too hard to use?)

---

_@carljm reviewed on 2024-09-04 15:42_

---

_@carljm reviewed on 2024-09-04 15:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1060 on 2024-09-04 15:43_

Opened a convo in Discord about this.

---

_@MichaReiser reviewed on 2024-09-04 15:44_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1060 on 2024-09-04 15:44_

@charliermarsh may have more context. He prefers adding the name. 

I agree that `git blame` may give you the same context. But it can be difficult if the code went through multiple refactors or was even moved to a different location. 

---

_@zanieb reviewed on 2024-09-04 15:45_

---

_Review comment by @zanieb on `crates/red_knot_python_semantic/src/types/infer.rs`:1060 on 2024-09-04 15:45_

I don't really have a preference here, though I think it's pretty easy for code to move around enough to make `git blame` useless at our pace of refactoring and development. I wouldn't mind skipping the inclusion of a name.

---

_@carljm reviewed on 2024-09-04 15:51_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1060 on 2024-09-04 15:51_

Before I was sent the above link, it was 100% my assumption that the meaning of the name in the TODO comment was "This is a TODO for me, i.e. I am going to fix it" (and thus, implicitly, nobody else should do it without asking me first.)

We can of course clarify this internally, but that doesn't mean it will be clear to the rest of the world.

---

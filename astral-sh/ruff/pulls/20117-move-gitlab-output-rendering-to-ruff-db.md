```yaml
number: 20117
title: "Move GitLab output rendering to `ruff_db`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/gitlab
created_at: 2025-08-27T16:43:15Z
updated_at: 2025-08-28T12:56:35Z
url: https://github.com/astral-sh/ruff/pull/20117
synced_at: 2026-01-12T15:56:54Z
```

# Move GitLab output rendering to `ruff_db`

---

_@ntBre_

## Summary

This PR is a first step toward adding a GitLab output format to ty. It converts the `GitlabEmitter` from `ruff_linter` to a `GitlabRenderer` in `ruff_db` and updates its implementation to handle non-Ruff files and diagnostics without primary spans. I tried to break up the changes here so that they're easy to review commit-by-commit, or at least in groups of commits:
- [preparatory changes in-place in `ruff_linter` and a `ruff_db` skeleton](https://github.com/astral-sh/ruff/pull/20117/files/0761b73a615537fe75fc54ba8f7d5f52c28b2a2a)
- [moving the code over with no implementation changes mixed in](https://github.com/astral-sh/ruff/pull/20117/files/0761b73a615537fe75fc54ba8f7d5f52c28b2a2a..8f909ea0bb0892107824b311f3982eb3cb1833ed)
- [tidying up the code now in `ruff_db`](https://github.com/astral-sh/ruff/pull/20117/files/9f047c4f9f1c826b963f0fa82ca2411d01d6ec83..e5e217fcd611ab24f516b2af4fa21e25eadc5645)

This wasn't strictly necessary, but I also added some `Serialize` structs instead of calling `json!` to make it a little clearer that we weren't modifying the schema (e4c4bee35d39eb0aff4cae4a62823279b99c6d9c).

I plan to follow this up with a separate PR exposing this output format in the ty CLI, which should be quite straightforward.

## Test Plan

Existing tests, especially the two that show up in the diff as renamed nearly without changes


---

_Label `internal` added by @ntBre on 2025-08-27 16:43_

---

_Label `diagnostics` added by @ntBre on 2025-08-27 16:43_

---

_Comment by @github-actions[bot] on 2025-08-27 16:45_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-27 16:47_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-08-27 17:06_

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

_@ntBre reviewed on 2025-08-27 17:18_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/gitlab.rs`:172 on 2025-08-27 17:18_

I suspect that we could just use our `render::relativize_path` helper here and drop this dependency,  but I kept it around for now. We don't have any existing tests covering the `CI_PROJECT_DIR` case.

---

_Marked ready for review by @ntBre on 2025-08-27 17:19_

---

_Review requested from @carljm by @ntBre on 2025-08-27 17:19_

---

_Review requested from @MichaReiser by @ntBre on 2025-08-27 17:19_

---

_Review requested from @sharkdp by @ntBre on 2025-08-27 17:19_

---

_Review requested from @dcreager by @ntBre on 2025-08-27 17:19_

---

_Review request for @dcreager removed by @ntBre on 2025-08-27 17:19_

---

_Review request for @carljm removed by @ntBre on 2025-08-27 17:19_

---

_Review request for @sharkdp removed by @ntBre on 2025-08-27 17:19_

---

_Review requested from @BurntSushi by @ntBre on 2025-08-27 17:19_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render/gitlab.rs`:43 on 2025-08-27 19:05_

How come `serde_json::json!` here instead of `serde_json::to_string`? I'd expect the latter to be faster since it doesn't have to construct an intermediate `serde_json::Value`, but that's just a wild guess.

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render/gitlab.rs`:43 on 2025-08-27 19:08_

I guess it's a shame we can't just stream this right to a `std::fmt::Write` without going through some intermediate value first.

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render/gitlab.rs`:172 on 2025-08-27 19:09_

Dropping a dependency sounds great. :-)

---

_@BurntSushi approved on 2025-08-27 19:11_

LGTM.

Possible idea for next time: if you have 3 logical groupings of commits, it might make sense to just squash that into 3 commits. Then you can write a commit message for that grouping and it should hopefully make review easier. (And avoid needing to write out the links for the individual groups of commits. Which I assume will go bad if you rebase.)

---

_@ntBre reviewed on 2025-08-27 19:27_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/gitlab.rs`:43 on 2025-08-27 19:27_

`json!` hides the `unwrap` call in the macro and we used it in the other JSON formats, but those aren't very good reasons :grimacing: 

But yeah, the `Emitter`s in `ruff_linter` take `std::io::Write`s instead of `std::fmt::Write`s, making this a little easier. We added an adapter in the `junit` PR, but I haven't used it any of the other formats yet:

https://github.com/astral-sh/ruff/blob/ef0586d0da0f04b73e0f53b375497c99eeb6ef18/crates/ruff_db/src/diagnostic/render/junit.rs#L150-L154

---

_Comment by @ntBre on 2025-08-27 19:29_

Thanks for the review!

> Possible idea for next time: if you have 3 logical groupings of commits, it might make sense to just squash that into 3 commits. Then you can write a commit message for that grouping and it should hopefully make review easier. (And avoid needing to write out the links for the individual groups of commits. Which I assume will go bad if you rebase.)

Will do, sorry about that. I thought each commit would be helpful initially, but then they got a bit too small. I should have squashed after identifying the groups. It's definitely a pain to maintain the links!


---

_@ntBre reviewed on 2025-08-27 20:44_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/gitlab.rs`:43 on 2025-08-27 20:44_

I switched it over to `to_string` for now!

---

_@ntBre reviewed on 2025-08-27 20:44_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/gitlab.rs`:172 on 2025-08-27 20:44_

I think I'll open a follow-up issue here. I'm _pretty_ sure that we can drop this entirely and just use the CWD-relative path, but I'm not 100% sure and don't want to break GitLab users.

It's a [one-function](https://docs.rs/pathdiff/0.2.3/src/pathdiff/lib.rs.html#43-86) dependency, so we could also vendor it. The behavior seems a bit different from our `relativize_path` if the `project_root` isn't a prefix, if we really need that.

---

_Merged by @ntBre on 2025-08-28 12:56_

---

_Closed by @ntBre on 2025-08-28 12:56_

---

_Branch deleted on 2025-08-28 12:56_

---

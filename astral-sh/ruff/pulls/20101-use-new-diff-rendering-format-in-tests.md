```yaml
number: 20101
title: Use new diff rendering format in tests
type: pull_request
state: merged
author: ntBre
labels:
  - testing
assignees: []
merged: true
base: main
head: brent/diff-rendering
created_at: 2025-08-26T16:33:00Z
updated_at: 2025-08-28T14:57:01Z
url: https://github.com/astral-sh/ruff/pull/20101
synced_at: 2026-01-12T15:56:54Z
```

# Use new diff rendering format in tests

---

_@ntBre_

## Summary

I spun this off from #19919 to separate the rendering code change and snapshot updates from the (much smaller) changes to expose this in the CLI. I grouped all of the `ruff_linter` snapshot changes in the final commit in an effort to make this easier to review. The code changes are in [this range](https://github.com/astral-sh/ruff/pull/20101/files/619395eb417d2030e31fbb0e487a72dac58902db).

I went through all of the snapshots, albeit fairly quickly, and they all looked correct to me. In the last few commits I was trying to resolve an existing issue in the alignment of the line number separator:

https://github.com/astral-sh/ruff/blob/73720c73be981df6f71ce837a67ca1167da0265e/crates/ruff_linter/src/rules/flake8_comprehensions/snapshots/ruff_linter__rules__flake8_comprehensions__tests__C409_C409.py.snap#L87-L89

In the snapshot above on `main`, you can see that a double-digit line number at the end of the context lines for a snippet was causing a misalignment with the other separators. That's now resolved. The one downside is that this can lead to a mismatch with the diagnostic above:

```
C409 [*] Unnecessary list literal passed to `tuple()` (rewrite as a tuple literal)
 --> C409.py:4:6
  |
2 |   t2 = tuple([1, 2])
3 |   t3 = tuple((1, 2))
4 |   t4 = tuple([
  |  ______^
5 | |     1,
6 | |     2
7 | | ])
  | |__^
8 |   t5 = tuple(
9 |       (1, 2)
  |
help: Rewrite as a tuple literal
1  | t1 = tuple([])
2  | t2 = tuple([1, 2])
3  | t3 = tuple((1, 2))
   - t4 = tuple([
4  + t4 = (
5  |     1,
6  |     2
   - ])
7  + )
8  | t5 = tuple(
9  |     (1, 2)
10 | )
note: This is an unsafe fix and may remove comments or change runtime behavior
```

But I don't think we can avoid that without really reworking this rendering to make the diagnostic and diff rendering aware of each other. Anyway, this should only happen in relatively rare cases where the diagnostic is near a digit boundary and also near a context boundary. Most of our diagnostics line up nicely.

Another potential downside of the new rendering format is its handling of long stretches of `+` or `-` lines:

```
help: Replace with `Literal[...] | None`
21 |     ...
22 |
23 |
   - def func6(arg1: Literal[
   -     "hello",
   -     None  # Comment 1
   -     , "world"
   -     ]):
24 + def func6(arg1: Literal["hello", "world"] | None):
25 |     ...
26 |
27 |
note: This is an unsafe fix and may remove comments or change runtime behavior
```

To me it just seems a little hard to tell what's going on with just a long streak of `-`-prefixed lines. I saw an even more exaggerated example at some point, but I think this is also fairly rare. Most of the snapshots seem more like the examples we looked at on Discord with plenty of `|` lines and pairs of `+` and `-` lines. 

## Test Plan

Existing tests plus one new test in `ruff_db` to isolate a line separator alignment issue


---

_Comment by @github-actions[bot] on 2025-08-26 16:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-08-26 16:49_

---

_Review requested from @carljm by @ntBre on 2025-08-26 16:49_

---

_Review requested from @MichaReiser by @ntBre on 2025-08-26 16:49_

---

_Review requested from @sharkdp by @ntBre on 2025-08-26 16:49_

---

_Review requested from @dcreager by @ntBre on 2025-08-26 16:49_

---

_Review requested from @AlexWaygood by @ntBre on 2025-08-26 16:49_

---

_Review request for @dcreager removed by @ntBre on 2025-08-26 16:49_

---

_Review request for @carljm removed by @ntBre on 2025-08-26 16:49_

---

_Review request for @sharkdp removed by @ntBre on 2025-08-26 16:49_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-08-26 16:49_

---

_Review requested from @zanieb by @ntBre on 2025-08-26 16:49_

---

_Review requested from @dhruvmanila by @ntBre on 2025-08-26 16:49_

---

_Label `testing` added by @ntBre on 2025-08-26 16:49_

---

_@dhruvmanila approved on 2025-08-28 10:47_

This looks pretty straightforward change, thanks for taking the time to go through the snapshot changes!

It would be useful for @zanieb to take a brief look as I think they've been heavily involved in the design process.

---

_@zanieb reviewed on 2025-08-28 14:07_

---

_Review comment by @zanieb on `crates/ruff_db/src/diagnostic/render/full.rs`:236 on 2025-08-28 14:07_

I might just say "change behavior"? I guess we could keep "may remove comments" for now, but I'm wary to encode that into the meaning of "unsafe" (I think it's sort of vague these days). The silly part is that we should _know_ if we've dropped a comment, e.g., by inspecting the diff? That's separate work though :)

---

_@zanieb approved on 2025-08-28 14:09_

---

_@ntBre reviewed on 2025-08-28 14:22_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/full.rs`:236 on 2025-08-28 14:22_

Sounds good! Those are the two main criteria in our docs, but it does feel a bit too specific to include in each diagnostic message.

---

_Merged by @ntBre on 2025-08-28 14:56_

---

_Closed by @ntBre on 2025-08-28 14:56_

---

_Branch deleted on 2025-08-28 14:57_

---

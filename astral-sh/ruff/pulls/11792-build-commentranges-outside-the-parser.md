```yaml
number: 11792
title: "Build `CommentRanges` outside the parser"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/lazy-comment-ranges
created_at: 2024-06-07T04:14:50Z
updated_at: 2024-06-09T10:11:10Z
url: https://github.com/astral-sh/ruff/pull/11792
synced_at: 2026-01-10T21:56:00Z
```

# Build `CommentRanges` outside the parser

---

_Pull request opened by @dhruvmanila on 2024-06-07 04:14_

## Summary

This PR updates the parser to remove building the `CommentRanges` and instead it'll be built by the linter and the formatter when it's required.

For the linter, it'll be built and owned by the `Indexer` while for the formatter it'll be built from the `Tokens` struct and passed as an argument.

## Test Plan

`cargo insta test`


---

_@dhruvmanila reviewed on 2024-06-07 05:08_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:524 on 2024-06-07 05:08_

I think this is a better approach to create `CommentRanges` instead of `CommentRangesBuilder`. This is the primary way of building the `CommentRanges` outside the linter. In the linter, the `Indexer` creates it because it's already looping over the tokens.

---

_Comment by @github-actions[bot] on 2024-06-07 05:19_

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

_Label `internal` added by @dhruvmanila on 2024-06-07 09:14_

---

_Marked ready for review by @dhruvmanila on 2024-06-07 09:15_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-07 09:15_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:1 on 2024-06-07 10:02_

Now that `CommentRange`'s is now longer built as part of the `Parsed` output. Should we move it's definition outside the parser?

---

_Review comment by @MichaReiser on `crates/ruff_python_index/src/indexer.rs`:97 on 2024-06-07 10:04_

I think the advantage of the `CommentsBuilder` was to enforce that the tokens are guaranteed to be in sorted order.  This is now no longer guaranteed where `CommentRange`s can be built by "anyone". 

I would probably keep the builder and have two methods:

```
CommentRangesBuilder::default().from_tokens(tokens)

// or

CommentRangesBuilder::default().push_token(token).push_token(token...).finish()
```

---

_@MichaReiser approved on 2024-06-07 10:04_

---

_@dhruvmanila reviewed on 2024-06-07 11:13_

---

_Review comment by @dhruvmanila on `crates/ruff_python_index/src/indexer.rs`:97 on 2024-06-07 11:13_

I don't think that invariant was checked earlier: https://github.com/astral-sh/ruff/blob/246a3388ee1aafe5ba685abf3f971e3304efeb44/crates/ruff_python_index/src/comment_ranges.rs#L15-L19, although I think it makes sense.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:1 on 2024-06-07 11:13_

I'm confused here as `CommentRanges` is defined in `ruff_python_trivia`. Do you mean something else?

---

_@dhruvmanila reviewed on 2024-06-07 11:13_

---

_@MichaReiser reviewed on 2024-06-07 12:14_

---

_Review comment by @MichaReiser on `crates/ruff_python_index/src/indexer.rs`:97 on 2024-06-07 12:14_

I think we could add it as a debug assertion

---

_@MichaReiser reviewed on 2024-06-07 12:14_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:1 on 2024-06-07 12:14_

Ah lol, I incorrectly assumed it's in the parser because of the `From<Tokens>` implementation. Never mind

---

_@dhruvmanila reviewed on 2024-06-07 13:44_

---

_Review comment by @dhruvmanila on `crates/ruff_python_index/src/indexer.rs`:97 on 2024-06-07 13:44_

Not sure what's the advantage of `CommentRangesBuilder.from_tokens` is because `Tokens` is guaranteed in sorted order otherwise everything would just break 😅 

---

_@MichaReiser reviewed on 2024-06-07 14:15_

---

_Review comment by @MichaReiser on `crates/ruff_python_index/src/indexer.rs`:97 on 2024-06-07 14:15_

That's true. Lol, I should not make things complicated. Maybe keep what you had 

---

_Merged by @dhruvmanila on 2024-06-09 09:55_

---

_Closed by @dhruvmanila on 2024-06-09 09:55_

---

_Branch deleted on 2024-06-09 09:55_

---

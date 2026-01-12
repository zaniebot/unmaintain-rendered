```yaml
number: 7360
title: Use CommentRanges in backwards lexing
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: remove-backwards-lexing
created_at: 2023-09-13T18:20:29Z
updated_at: 2023-09-16T03:28:54Z
url: https://github.com/astral-sh/ruff/pull/7360
synced_at: 2026-01-12T02:39:09Z
```

# Use CommentRanges in backwards lexing

---

_Pull request opened by @konstin on 2023-09-13 18:20_

## Summary

The tokenizer was split into a forward and a backwards tokenizer. The backwards tokenizer uses the same names as the forwards ones (e.g. `next_token`). The backwards tokenizer gets the comment ranges that we already built to skip comments.

---

_Label `internal` added by @konstin on 2023-09-13 18:20_

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:521 on 2023-09-13 19:09_

Nit: We should avoid doing a binary search on every newline. We can instead do a binary search to find the end position and then use take_if from the back 

---

_@MichaReiser reviewed on 2023-09-13 19:10_

I really like the idea. It will simplify the tokenizer a lot. 

I think I would overall prefer to split the backward lexing out of the simple tokenizer because the changes now complicate the use of `SimpleTokenizer` even when backward lexing isn't necessary. This will also allow us to support fewer tokens for backward lexing. 

---

_Comment by @github-actions[bot] on 2023-09-14 13:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review comment by @konstin on `crates/ruff_python_trivia/src/tokenizer.rs`:521 on 2023-09-15 10:20_

i've implemented this but it feels like i added more complexity than appropriate for that small perf :/ 

---

_@konstin reviewed on 2023-09-15 10:20_

---

_Marked ready for review by @konstin on 2023-09-15 10:28_

---

_@MichaReiser reviewed on 2023-09-15 11:35_

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/snapshots/ruff_python_trivia__tokenizer__tests__tokenize_bogus.snap`:24 on 2023-09-15 11:35_

Nit: Would you mind moving this change out of this PR and make a separate PR for it? It eases reviewing and gives us higher confidence that the behavior doesn't change with your refactor.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/parenthesize.rs`:12 on 2023-09-15 12:06_

I think I would prefer passing `CommentRange`s. While not strictly necessary, it makes it easier to search for usages where the comment ranges are required and would allow for changes to the internal representation. 

We can implement `Default` for CommentRanges` for the use case of that one rule that doesn't need comment ranges.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/mod.rs`:331 on 2023-09-15 12:09_

```suggestion
    pub(crate) fn ranges(&self) -> &'a CommentRanges {
        self.comment_ranges
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/mod.rs`:327 on 2023-09-15 12:10_

Could we instead implement `default` for `CommentRanges` ?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/binary_like.rs`:187 on 2023-09-15 12:11_

Nit: We could also consider passing `&PyFormatContext` to have fewer arguments

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/binary_like.rs`:884 on 2023-09-15 12:12_

```suggestion
            f.context().comments().ranges(),
```

Cloning shouldn't be necessary here because we aren't mutating `f`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:110 on 2023-09-15 12:13_

I think passing `&PyFormatContext` instead could help cleaning up the call sites. 

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:483 on 2023-09-15 12:14_

I think we can remove this field as well. It is only used for backward lexing

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:529 on 2023-09-15 12:15_

Nit: `self.cursor = Cursor::new("")`

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:749 on 2023-09-15 12:17_

Could we move `CommentRanges` to `ruff_python_trivia`

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:751 on 2023-09-15 12:18_

Nit: Storing a slice and updating the slice when needed should give you the same

```suggestion
    comment_ranges: &'a [TextRange],
```

In the next function:

```rust
while self
                .comment_ranges
                .last()
                .is_some_and(|comment| comment.start() > self.back_offset)
            {
                self.comment_ranges = &self.comment_ranges.split_last().unwrap().1;
            }

            // Inside of a comment
            if self.comment_ranges
                .last()
                .is_some_and(|comment| comment.contains_inclusive(self.back_offset))
            {
            } else {
            }
```

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:763 on 2023-09-15 12:19_

Could we remove this field now. It was mainly necessary to avoid re-lexing the entire line. But that is now longer required, instead a simple check whether the last comment in `comment_ranges` includes the current position is sufficient to know whether we are in a comment or not.

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:845 on 2023-09-15 12:20_

```suggestion
            self.cursor = Cursor::new("");
```

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:868 on 2023-09-15 12:22_

This could potentially happen if we start lexing before the `expr` in 

```python
f"#{expr}"
```

Let's return `Other` in this case.

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:1033 on 2023-09-15 12:24_

Nit
```suggestion
            let comment_ranges: Vec<_> = lex(self.source, Mode::Module).filter_map(|result| {
	             let (token, range) = result.expect("Input to be a valid python program.");
                if matches!(token, Tok::Comment(_)) {
                    Some(range)
                } else {
                	None
              	}
            }).collect()
```

---

_@MichaReiser approved on 2023-09-15 12:30_

---

_Comment by @MichaReiser on 2023-09-15 12:31_

:guitar: 

---

_@konstin reviewed on 2023-09-15 13:38_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/mod.rs`:327 on 2023-09-15 13:38_

that doesn't work unfortunately because we store only a reference an the reference needs to outlive this function call

---

_@konstin reviewed on 2023-09-15 13:39_

---

_Review comment by @konstin on `crates/ruff_python_trivia/src/tokenizer.rs`:529 on 2023-09-15 13:39_

is that fine? i wasn't sure if i could do that given that now the cursor and the tokenizer disagree about source

---

_@konstin reviewed on 2023-09-15 13:42_

---

_Review comment by @konstin on `crates/ruff_python_trivia/src/tokenizer.rs`:763 on 2023-09-15 13:42_

this does mean we have to do the binary search when instantiating the tokenizer independent of whether we hit a newline, do you still prefer this solution? (i've also been thinking about switching)

---

_@MichaReiser reviewed on 2023-09-15 13:56_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/mod.rs`:327 on 2023-09-15 13:56_

On that note... we should probably move `CommentRange`s into the RC or `clone` to avoid that `clone` becomes more expensive. 

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:529 on 2023-09-15 13:57_

The cursor only stores the remainder string, which is empty at this point.

---

_@MichaReiser reviewed on 2023-09-15 13:57_

---

_@MichaReiser reviewed on 2023-09-15 13:57_

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/tokenizer.rs`:763 on 2023-09-15 13:57_

Yeah, makes sense.

---

_@konstin reviewed on 2023-09-15 20:53_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/mod.rs`:110 on 2023-09-15 20:53_

that fails because it counts as an immutable borrow of `f`

---

_@MichaReiser reviewed on 2023-09-15 20:59_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:110 on 2023-09-15 20:59_

Where is this a problem? Is expression parenthesized returns a bool without any lifetimes 

---

_@konstin reviewed on 2023-09-15 21:06_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/mod.rs`:110 on 2023-09-15 21:06_

it's not there but other call sites of `is_expression_parenthesized` or their parents which e.g. call it in closures

---

_@konstin reviewed on 2023-09-15 21:08_

---

_Review comment by @konstin on `crates/ruff_python_trivia/src/tokenizer.rs`:868 on 2023-09-15 21:08_

ok, but it's really bad if you're doing that, the output of the tokenizer is completely wrong when you're lexing string content as rust 

---

_@konstin reviewed on 2023-09-15 21:54_

---

_Review comment by @konstin on `crates/ruff_python_trivia/src/tokenizer.rs`:749 on 2023-09-15 21:54_

done, it's a bit odd though because the builder depends on `Tok` so it has to stay

---

_Comment by @konstin on 2023-09-15 22:02_

Current dependencies on/for this PR:
* main
  * **PR #7360** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7360" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #7425** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7425" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/7360?utm_source=stack-comment).

---

_Merged by @konstin on 2023-09-16 03:21_

---

_Closed by @konstin on 2023-09-16 03:21_

---

_Branch deleted on 2023-09-16 03:21_

---

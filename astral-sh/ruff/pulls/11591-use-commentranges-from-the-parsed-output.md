```yaml
number: 11591
title: "Use `CommentRanges` from the parsed output"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/comment-ranges
created_at: 2024-05-29T01:50:12Z
updated_at: 2024-05-29T08:34:31Z
url: https://github.com/astral-sh/ruff/pull/11591
synced_at: 2026-01-12T15:55:38Z
```

# Use `CommentRanges` from the parsed output

---

_@dhruvmanila_

## Summary

This PR updates the usages of `CommentRanges` from the `Indexer` to the parsed output.

First, the `CommentRanges` are now built during the parsing step. In the future, depending on the performance numbers, it could be updated to be done after the parsing step. Nevertheless, the struct will be owned by the parsed output. So, all references should now use the struct from the parsed output instead of the indexer.

Now, the motivation to build `CommentRanges` while parsing is so that it can be shared between the linter and the formatter as both requires it. This also removes the need to have a dedicated method (`tokens_and_ranges`).

There are two main updates:
1. The functions which only require `CommentRanges` are passed in the type directly
2. If the functions require other fields from `Program`, then the program is passed instead

---

_Label `internal` added by @dhruvmanila on 2024-05-29 01:50_

---

_Marked ready for review by @dhruvmanila on 2024-05-29 06:01_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-05-29 06:01_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-29 06:10_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/tokens.rs`:46 on 2024-05-29 06:40_

Nit: You could assigned `tokens` and `comment_ranges` to local variables to reduce the diff size. 

```rust

			let tokens = program.tokens();
			let comment_ranges = program.comment_ranges();

			BlankLinesChecker::new(locator, stylist, settings, source_type, cell_offsets)
            .check_lines(tokens, &mut diagnostics);
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:240 on 2024-05-29 06:40_

Is the change from `UnsafeFix` to `SafeFix` intentional?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_with.rs`:172 on 2024-05-29 06:44_

I think this is a case where it could make sense to simply expose `comment_ranges` on `checker` so that it matters less where the comment ranges are actually stored. I don't think it is necessary to change now but it would e.g. help if we move comment ranges in the future again. 

Overall, I think this is a weakness of our `checker` API. Lint rules need to know too much about individual representations. I think it would be better if there's a single `context` and the rules could simply ask:

* Give me the comment ranges
* give me the indexer
* give me the line index 


etc. without having to do deep drilling. 

---

_@MichaReiser approved on 2024-05-29 06:47_

>  Nevertheless, the struct will be owned by the parsed output. So, all references should now use the struct from the parsed output instead of the indexer.

I'm not sure if this will remain true if we build the comment ranges after parsing. One of the benefits of building it after parsing is that we can make it optional and only run it when the comment ranges are needed. 

---

_@dhruvmanila reviewed on 2024-05-29 08:31_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/linter.rs`:240 on 2024-05-29 08:31_

No, it's the way diff is being rendered. I've updated the code to assign it to a variable which reduces the diff.

---

_@dhruvmanila reviewed on 2024-05-29 08:32_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_simplify/rules/ast_with.rs`:172 on 2024-05-29 08:32_

Yeah, I completely agree with your sentiment.

---

_Merged by @dhruvmanila on 2024-05-29 08:34_

---

_Closed by @dhruvmanila on 2024-05-29 08:34_

---

_Branch deleted on 2024-05-29 08:34_

---

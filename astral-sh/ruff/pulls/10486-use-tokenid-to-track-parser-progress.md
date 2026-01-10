```yaml
number: 10486
title: "Use `TokenId` to track parser progress"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/token-id
created_at: 2024-03-20T09:06:37Z
updated_at: 2024-03-20T12:09:17Z
url: https://github.com/astral-sh/ruff/pull/10486
synced_at: 2026-01-10T22:47:02Z
```

# Use `TokenId` to track parser progress

---

_Pull request opened by @dhruvmanila on 2024-03-20 09:06_

## Summary

This PR updates the parser progress mechanism to use a token id instead of the token kind and range. The ID is stored on the Parser and is incremented every time the `next_token` method is called.

The old logic would lead to panic if there were multiple levels of indentation before the program ends when the lexer would emit `Dedent` tokens for the same range. This makes it seem that the parser isn't progressing.

## Test Plan

Tested it with the following program which gets stuck on `dhruv/parser` because the parser isn't able to recover from the invalid mapping key pattern:

```py
match subject:
    case {*key}:
        pass
```

The reason it's only failing for invalid programs is because otherwise the parsing logic (`parse_block`) would consume the `Dedent` token.

---

_Label `parser` added by @dhruvmanila on 2024-03-20 09:06_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-20 09:06_

---

_Comment by @MichaReiser on 2024-03-20 09:19_

Does this have any impact on performance?

---

_@MichaReiser approved on 2024-03-20 09:22_

---

_Comment by @github-actions[bot] on 2024-03-20 09:26_

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

_Comment by @dhruvmanila on 2024-03-20 11:13_

> Does this have any impact on performance?

No, I don't see any impact running the benchmarks locally.

---

_Merged by @dhruvmanila on 2024-03-20 11:13_

---

_Closed by @dhruvmanila on 2024-03-20 11:13_

---

_Branch deleted on 2024-03-20 11:13_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/progress.rs`:10 on 2024-03-20 11:40_

Hmm, I'm confused. I'm pretty sure that I wrote a comment when I reviewed this PR but it seems gone. 

Anyway, it's a Nit but I don't think it's necessary to use `checked_add` here because we only need to know if it's still the same token. True, it would be possible that `ParserProgress` doesn't recognize when an iteration exactly processed `u32::MAX` items but this isn't a really issue because of what you outline in the SAFETY comment

```suggestion
self.0 = self.0.wrapping_add(1)
```

---

_@MichaReiser reviewed on 2024-03-20 11:40_

---

_@dhruvmanila reviewed on 2024-03-20 12:09_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/progress.rs`:10 on 2024-03-20 12:09_

Oh right. I need more coffee :)

---

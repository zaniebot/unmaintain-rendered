```yaml
number: 9466
title: "Remove type parameter from `parse_*` methods"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - parser
assignees: []
merged: true
base: main
head: token-source
created_at: 2024-01-11T11:41:41Z
updated_at: 2024-01-11T18:41:20Z
url: https://github.com/astral-sh/ruff/pull/9466
synced_at: 2026-01-10T22:57:09Z
```

# Remove type parameter from `parse_*` methods

---

_Pull request opened by @MichaReiser on 2024-01-11 11:41_

This PR removes the `I` (tokens iterator) type parameter from all `parse_*` methods. This is done in preparation for https://github.com/astral-sh/ruff/pull/9152 to avoid accidentally monomorphizing the parser more than once. 

The `Parser` struct defined in https://github.com/astral-sh/ruff/pull/9152 is parametrized by the underlying tokens iterator. This is dangerous because Rust will monomorphize the entire parser for each distinct `I` type parameter. 

This PR removes the `I` type parameters from all `parse_*` methods and introduces a new `TokenSource` type in the parser that:

* Filters out trivia tokens
* Allows lookahead without the need for `MultiPeek` (which has an overhead when reading tokens)
* Is a single type used by our `Parser` to read tokens. 

The downside is that all `parse_` methods now take a `Vec<LexResult>`, which requires collecting the lexer result into a Vec before parsing. Our micro-benchmarks show that this is slower than streaming the tokens one by one. 

However, it turns out that both the linter and formatter both collect the tokens before parsing them to an AST. That means the path tested by the micro-benchmark isn't used in the actual product and the introduced regression doesn't affect users. 

You may wonder why this is only a problem now but hasn't been a problem with lalrpop. The problem existed with lalrpop as well but to a much smaller extent because lalrpop separates the parser into two parts:

* A state machine parametrized by `I` that consumes the tokens and calls into the parser methods (only passing the tokens)
* The actual parsing methods

Only the state machine gets monomorphized, which is fine because it is limited in size. Another way to think about it is that the lalrpop pushes the tokens into the parser (and, therefore, the parser is decoupled from I). In contrast, the handwritten parser pulls the tokens (and, therefore, is generic over I).

## Alternatives

Alternatives that may be less affected by the performance regression are

* A `TokenSource` enum with two variants: One storing the lexed tokens and the other storing the lexer to consume the tokens lazily.
* Storing a `Box<impl Iterator>` in the hand written parser

I went with the above approach because we don't need the flexibility of either lexing lazily or eagerly in Ruff outside the parser benchmark. Thus, using a `Vec<LexResult>` seemed the easiest solution.

CC: @LaBatata101 

---

_Comment by @codspeed-hq[bot] on 2024-01-11 12:01_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/token-source)

### Merging #9466 will **degrade performances by 6.76%**

<sub>Comparing <code>token-source</code> (9d17580) with <code>main</code> (14d3fe6)</sub>



### Summary

`❌ 5 (👁 5)` regressions
`✅ 25` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `token-source` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `parser[unicode/pypinyin.py]` | 4 ms | 4.3 ms | -6.76% |
| 👁 | `parser[pydantic/types.py]` | 25.7 ms | 27.3 ms | -5.9% |
| 👁 | `parser[numpy/globals.py]` | 1.1 ms | 1.1 ms | -4.87% |
| 👁 | `parser[numpy/ctypeslib.py]` | 11.6 ms | 12.2 ms | -5.49% |
| 👁 | `parser[large/dataset.py]` | 67.4 ms | 70.8 ms | -4.82% |


---

_Comment by @github-actions[bot] on 2024-01-11 12:43_

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

_Renamed from "Add TokenSource" to "Remove type parameter from `parse_*` methods" by @MichaReiser on 2024-01-11 13:41_

---

_Label `internal` added by @MichaReiser on 2024-01-11 13:42_

---

_Label `parser` added by @MichaReiser on 2024-01-11 13:42_

---

_Marked ready for review by @MichaReiser on 2024-01-11 13:52_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-01-11 13:52_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-01-11 13:52_

---

_Review requested from @BurntSushi by @MichaReiser on 2024-01-11 13:52_

---

_@charliermarsh approved on 2024-01-11 14:47_

This seems reasonable to me given the explanation (especially around the perceived regression) in the summary.

---

_Review comment by @BurntSushi on `crates/ruff_python_parser/src/parser.rs`:214 on 2024-01-11 15:10_

I was trying to figure out why this is a dedicated type instead of a `tokens.into_iter().filter(...)` but couldn't come up with a reason. (Usually it's because you want to name the type somewhere, but maybe I'm missing that.)

---

_@BurntSushi approved on 2024-01-11 15:10_

I buy what you're selling.

---

_@MichaReiser reviewed on 2024-01-11 15:50_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser.rs`:214 on 2024-01-11 15:50_

It's mainly to have a named type in https://github.com/astral-sh/ruff/pull/9152 and a place where we can implement lookahead without using `PeekMany`

---

_@BurntSushi reviewed on 2024-01-11 15:51_

---

_Review comment by @BurntSushi on `crates/ruff_python_parser/src/parser.rs`:214 on 2024-01-11 15:51_

Ah gotya, don't mind me then. :)

---

_Comment by @LaBatata101 on 2024-01-11 18:28_

LGTM. I was actually going to something like this later, glad you saved me the work 😊

---

_Merged by @MichaReiser on 2024-01-11 18:41_

---

_Closed by @MichaReiser on 2024-01-11 18:41_

---

_Branch deleted on 2024-01-11 18:41_

---

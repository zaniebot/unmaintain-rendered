```yaml
number: 10412
title: Apply NFKC normalization to unicode identifiers in the lexer
type: pull_request
state: merged
author: AlexWaygood
labels:
  - parser
assignees: []
merged: true
base: main
head: unicode-normalization-2
created_at: 2024-03-14T17:34:50Z
updated_at: 2024-03-18T11:57:09Z
url: https://github.com/astral-sh/ruff/pull/10412
synced_at: 2026-01-12T15:55:32Z
```

# Apply NFKC normalization to unicode identifiers in the lexer

---

_@AlexWaygood_

## Summary

A second attempt to fix #5003, _hopefully_ without the performance problems that https://github.com/astral-sh/ruff/pull/10381 suffered from.

Python [applies NFKC normalization to identifiers that use unicode characters](https://docs.python.org/3/reference/lexical_analysis.html#identifiers). That means that F821 should not be emitted if ruff encounters the following snippet (but on `main`, it is), as from Python's perspective, these are all the same identifier:

```py
𝒞 = 500
print(𝒞)
print(C + 𝒞)  # ruff says `C` isn't defined
print(C / 𝒞)
print(C == 𝑪 == 𝒞 == 𝓒 == 𝕮)  # ruff says `C`, `𝑪`, ... isn't defined
```

This PR fixes that false positive by NFKC-normalizing identifers as they are encountered in the lexer.

## Test Plan

`cargo test`


---

_Renamed from "fix the bug" to "Apply NFKC normalization to unicode identifiers in the lexer" by @AlexWaygood on 2024-03-14 17:35_

---

_@AlexWaygood reviewed on 2024-03-14 17:38_

---

_Review comment by @AlexWaygood on `crates/ruff_python_formatter/src/expression/expr_name.rs`:22 on 2024-03-14 17:38_

This debug assertion no longer passes as-is, since the AST node holds the NFKC-normalized identifier, whereas the source code gives us the original identifier.

If we think this assertion is valuable, we could put it in a `#[cfg(test)]` block, make the `unicode-normalization` crate a test-dependency of the formatter, and assert that the NFKC-normalized source code is equal to the AST `id`. I don't know if that's really worth it, though?

---

_Marked ready for review by @AlexWaygood on 2024-03-14 17:42_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-03-14 17:42_

---

_Comment by @github-actions[bot] on 2024-03-14 17:54_

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

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:173 on 2024-03-15 18:16_

nit: maybe `is_first_char_ascii` / `is_first_ascii`?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1594 on 2024-03-15 18:19_

nit: sorry for name recommendations but maybe `is_identifier_ascii_only`?

---

_@dhruvmanila reviewed on 2024-03-15 18:23_

This is great with such a minimal change. Can you please write / update documentation around this change? Maybe the `lex_identifier` or the `Name` token?

It's good to go from my side but I'll let @MichaReiser give the final approval.

---

_@AlexWaygood reviewed on 2024-03-15 18:42_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/lexer.rs`:173 on 2024-03-15 18:42_

I went for `first_char_is_ascii` in https://github.com/astral-sh/ruff/pull/10412/commits/d785be50606df110d848606165e2f7a9aa406835

---

_@AlexWaygood reviewed on 2024-03-15 18:42_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/lexer.rs`:1594 on 2024-03-15 18:42_

I went for `identifier_is_ascii_only` in https://github.com/astral-sh/ruff/pull/10412/commits/d785be50606df110d848606165e2f7a9aa406835

---

_Comment by @AlexWaygood on 2024-03-15 18:43_

> Can you please write / update documentation around this change? Maybe the `lex_identifier` or the `Name` token?

Thanks! I had a go at writing some docs in https://github.com/astral-sh/ruff/pull/10412/commits/d785be50606df110d848606165e2f7a9aa406835 -- how does that look to you?

---

_Comment by @dhruvmanila on 2024-03-16 02:32_

> Thanks! I had a go at writing some docs in [d785be5](https://github.com/astral-sh/ruff/commit/d785be50606df110d848606165e2f7a9aa406835) -- how does that look to you?

Looks good. Thank you!

---

_@charliermarsh reviewed on 2024-03-17 21:36_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/token.rs`:22 on 2024-03-17 21:36_

I was eventually hoping to remove all owned data from the lexer tokens (e.g., prior to this change, we could've conceivably removed this field altogether; if we remove more similar fields from other tokens, we can eventually reduce the size of the `Tok` enum, which could be great for performance). This now precludes us from doing so. But I don't have enough context on the future design of the lexer-parser to know if it matters.

---

_@charliermarsh reviewed on 2024-03-17 21:36_

Looks reasonable to me, I'll defer to Micha and Dhruv to approve.

---

_Label `parser` added by @MichaReiser on 2024-03-18 09:33_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pyflakes/F821_28.py`:1 on 2024-03-18 09:35_

Nice. Could you add a trimmed down version of this as a lexer or parser test?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_name.rs`:22 on 2024-03-18 09:36_

I think it's fine to remove it. 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token.rs`:22 on 2024-03-18 09:42_

I think we can still accomplish this and `Name` isn't the only token that must return the parsed value (e.g. FStrings etc).

What I have in mind is to:

* Replace `Tok` with `TokenKind` (that holds no data)
* Add a `take_value()` method on `Lexer` that "takes" the current value (`enum` over `Name`, `Int` etc.). 

This design also fits better into our new parser that already does exactly this internally (except that it "takes" the value from `Tok`). The advantage of this is that we only pay the overhead of reading or writting a value if it is a value token (and we're interested in the value). 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:176 on 2024-03-18 09:44_

Nit: I would call `first.is_ascii()` inside of the method. It avoids leaking the abstraction and the cost of determining if a character is ascii is a simple range comparision that might as well be cheaper than passing a value to a function call (requires pushing and poping from the stack).

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:216 on 2024-03-18 09:45_

Nit: We can skip the entire `match` for `non-ascii` text. The old code didn't do this to avoid branching but we might as well do it now, where we branch on the flag anyway.

---

_@MichaReiser approved on 2024-03-18 09:46_

---

_@dhruvmanila reviewed on 2024-03-18 09:47_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/token.rs`:22 on 2024-03-18 09:47_

This is a good point. We could potentially move this to the `parse_identifier` method in the parser as that's the final destination for where this value needs to be. I could come back to this once the new parser is merged and I'm looking into the feedback loop between the lexer and parser.

---

_Merged by @AlexWaygood on 2024-03-18 11:56_

---

_Closed by @AlexWaygood on 2024-03-18 11:56_

---

_Branch deleted on 2024-03-18 11:56_

---

_Comment by @AlexWaygood on 2024-03-18 11:57_

Thanks all!

---

```yaml
number: 11361
title: "Add `Tokens` newtype wrapper, `TokenKind` iterator"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - parser
assignees: []
merged: true
base: main
head: dhruv/tokens
created_at: 2024-05-10T13:09:09Z
updated_at: 2024-05-14T16:57:22Z
url: https://github.com/astral-sh/ruff/pull/11361
synced_at: 2026-01-12T15:55:37Z
```

# Add `Tokens` newtype wrapper, `TokenKind` iterator

---

_@dhruvmanila_

## Summary

Alternative to #11237 

This PR adds a new `Tokens` struct which is a newtype wrapper around a vector of lexer output. This allows us to add a `kinds` method which returns an iterator over the corresponding `TokenKind`. This iterator is implemented as a separate `TokenKindIter` struct to allow using the type and provide additional methods like `peek` directly on the iterator.

This exposes the linter to access the stream of `TokenKind` instead of `Tok`.

Edit: I've made the necessary downstream changes and plan to merge the entire stack at once.


---

_Label `parser` added by @dhruvmanila on 2024-05-10 13:09_

---

_@MichaReiser approved on 2024-05-10 13:19_

---

_Comment by @github-actions[bot] on 2024-05-10 13:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
Failed to pull: From https://github.com/lnbits/lnbits
 + 365f9a39...e9e69d9d main       -> origin/main  (forced update)
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint: 
hint:   git config pull.rebase false  # merge
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint: 
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
fatal: Need to specify how to reconcile divergent branches.
```

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Renamed from "Add `Tokens` newtype wrapper" to "Add `Tokens` newtype wrapper, `TokenKind` iterator" by @dhruvmanila on 2024-05-14 03:12_

---

_Label `internal` added by @dhruvmanila on 2024-05-14 03:12_

---

_Marked ready for review by @dhruvmanila on 2024-05-14 03:13_

---

_Comment by @dhruvmanila on 2024-05-14 04:00_

I might possibly need to maintain a `Vec<TokenKind>` because certain references utilizes `lex_starts_at` which returns the tokens within a certain range. An iterator would make it a `O(n)` while a vector would allow us to do a binary search. I could possibly use a `BTreeMap` of token start to the `(TokenKind, TextRange)` to directly use the `range` method.

Edit: I'm using binary search to get the start and end index.

---

_@dhruvmanila reviewed on 2024-05-14 12:40_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:378 on 2024-05-14 12:40_

So, this isn't currently being used anywhere. This would basically replace the usages of `lex_starts_at` but it turns out all of the usages of the function is being done in the AST checker where we don't have access to the token stream.

One solution here would be to store a `TokenKinds` struct which contains `Vec<(TokenKind, TextRange)>` on the `Checker`. This way the rules which utilizes the `lex_starts_at` or `lex` function can still get the tokens.

Another would be to club this change with the parser. I'm leaning more towards this.

---

_Merged by @dhruvmanila on 2024-05-14 16:45_

---

_Closed by @dhruvmanila on 2024-05-14 16:45_

---

_Branch deleted on 2024-05-14 16:45_

---

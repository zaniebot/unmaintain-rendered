```yaml
number: 16478
title: "[syntax-errors] `type` statements before Python 3.12"
type: pull_request
state: merged
author: ntBre
labels:
  - parser
  - preview
assignees: []
merged: true
base: main
head: brent/syn-type-stmt
created_at: 2025-03-03T18:59:06Z
updated_at: 2025-03-04T17:24:22Z
url: https://github.com/astral-sh/ruff/pull/16478
synced_at: 2026-01-12T15:55:55Z
```

# [syntax-errors] `type` statements before Python 3.12

---

_@ntBre_

Summary
--
Another simple one, just detect standalone `type` statements. I limited the diagnostic to `type` itself like [pyright]. That probably makes the most sense for more complicated examples.

Test Plan
--
Inline tests.

[pyright]: https://pyright-play.net/?pythonVersion=3.8&strict=true&code=C4TwDgpgBAHlC8UCWA7YQ

---

_Label `parser` added by @ntBre on 2025-03-03 18:59_

---

_Label `preview` added by @ntBre on 2025-03-03 18:59_

---

_Review requested from @MichaReiser by @ntBre on 2025-03-03 18:59_

---

_Review requested from @dhruvmanila by @ntBre on 2025-03-03 18:59_

---

_Comment by @github-actions[bot] on 2025-03-03 19:08_

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

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:452 on 2025-03-04 08:32_

Should we use type alias instead of type statement? I would find this more approachable as a user, unless the term is ambiguous. 

```suggestion
    TypeAlias,
```

https://docs.python.org/3/library/typing.html#type-aliases

If not `TypeAlias`, then rename to `TypeStatement`

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:894 on 2025-03-04 08:34_

Using `node_range` to get the range of `type` is a bit confusing because it isn't a node
```suggestion
				let type_range = self.current_token_range();
        self.bump(TokenKind::Type);

```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:923 on 2025-03-04 08:35_

I would probably move this up right after where we parsed the `type` keyword. Unless you want to suppress the error if there are any other syntax errors.

---

_@MichaReiser approved on 2025-03-04 08:35_

---

_@dhruvmanila approved on 2025-03-04 10:59_

---

_@ntBre reviewed on 2025-03-04 16:57_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:452 on 2025-03-04 16:57_

pyright calls them "type alias statements," which I think sounds good for the user-facing message. For the variant name, I don't have strong feelings either way. They do call it a [`type_stmt`](https://docs.python.org/3/reference/simple_stmts.html#the-type-statement) in the Python reference, so I might lean toward `TypeStatement` if you prefer that to `Stmt`. 

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:894 on 2025-03-04 16:58_

Thanks, I overlooked `current_token_range` before.

---

_@ntBre reviewed on 2025-03-04 16:58_

---

_@ntBre reviewed on 2025-03-04 17:07_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/error.rs`:452 on 2025-03-04 17:07_

I'll just call it `TypeAliasStatement`, I see that's what we call the parser function now.

---

_@ntBre reviewed on 2025-03-04 17:15_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/statement.rs`:923 on 2025-03-04 17:15_

Oh right, I think it was down there from when I was using the whole statement range. That makes sense now!

---

_Merged by @ntBre on 2025-03-04 17:20_

---

_Closed by @ntBre on 2025-03-04 17:20_

---

_Branch deleted on 2025-03-04 17:20_

---

```yaml
number: 15049
title: Modify parsing of raise with cause when exception is absent
type: pull_request
state: merged
author: dylwil3
labels:
  - parser
assignees: []
merged: true
base: main
head: recover-exc-from
created_at: 2024-12-18T19:35:51Z
updated_at: 2024-12-19T13:40:56Z
url: https://github.com/astral-sh/ruff/pull/15049
synced_at: 2026-01-10T20:42:27Z
```

# Modify parsing of raise with cause when exception is absent

---

_Pull request opened by @dylwil3 on 2024-12-18 19:35_

When confronted with `raise from exc` the parser will now create a `StmtRaise` that has `None` for the exception and `exc` for the cause. 

Before, the parser created a `StmtRaise` with `from` for the exception, no cause, and a spurious expression `exc` afterwards.


---

_Review requested from @MichaReiser by @dylwil3 on 2024-12-18 19:35_

---

_Review requested from @dhruvmanila by @dylwil3 on 2024-12-18 19:35_

---

_Label `parser` added by @dylwil3 on 2024-12-18 19:35_

---

_Comment by @github-actions[bot] on 2024-12-18 19:44_

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

_@dhruvmanila reviewed on 2024-12-19 07:18_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:416 on 2024-12-19 07:18_

nit: with multiple `self.at` calls, I'd normally move it to a `match` instead:

```rs
let exc = match self.current_token_kind() {
	TokenKind::Newline => {}
	TokenKind::From => {}
	_ => {}
};
```

---

_@dhruvmanila approved on 2024-12-19 07:19_

Thank you!

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:416 on 2024-12-19 07:26_

Now we're bikeshedding. So feel free to disregard. I'd probably move the check into the `let clause = self.eat` by adding a `if exc.is_none() { // add exception here`

---

_@MichaReiser approved on 2024-12-19 07:26_

---

_@dylwil3 reviewed on 2024-12-19 13:33_

---

_Review comment by @dylwil3 on `crates/ruff_python_parser/src/parser/statement.rs`:416 on 2024-12-19 13:33_

I went with the `match`, but next bikeshed color is yours, Micha, I promise!

---

_Merged by @dylwil3 on 2024-12-19 13:36_

---

_Closed by @dylwil3 on 2024-12-19 13:36_

---

_Branch deleted on 2024-12-19 13:37_

---

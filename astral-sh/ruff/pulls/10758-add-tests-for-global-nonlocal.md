```yaml
number: 10758
title: "Add tests for `global`, `nonlocal`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/init-simple-stmt
created_at: 2024-04-03T14:06:11Z
updated_at: 2024-04-04T10:09:32Z
url: https://github.com/astral-sh/ruff/pull/10758
synced_at: 2026-01-10T22:47:03Z
```

# Add tests for `global`, `nonlocal`

---

_Pull request opened by @dhruvmanila on 2024-04-03 14:06_

## Summary

This PR does two things:
1. Adds documentation for `continue`, `break`, and `pass` parsing functions
2. Adds documentation and tests for `nonlocal` and `global` parsing functions
3. Adds new error type for empty global and nonlocal statement 	


---

_Label `parser` added by @dhruvmanila on 2024-04-03 14:06_

---

_Label `testing` added by @dhruvmanila on 2024-04-03 14:06_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-03 14:06_

---

_Comment by @github-actions[bot] on 2024-04-03 14:24_

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

_@charliermarsh reviewed on 2024-04-03 20:19_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/parser/statement.rs`:546 on 2024-04-03 20:19_

Have you been doing this kind of thing elsewhere? Just seems hard to keep in-sync. If it's already an established pattern, no worries. Otherwise, I probably would suggest just documenting the cases, but not the test name, like:

```
// For example:
// ```python
// nonlocal
// ```
```

---

_@charliermarsh approved on 2024-04-03 20:19_

---

_@dhruvmanila reviewed on 2024-04-04 02:57_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:546 on 2024-04-04 02:57_

These are inline tests which gets extracted automatically into their own test file and gets tested. 

Refer #10681 and [documentation](https://github.com/astral-sh/ruff/blob/dhruv/parser/crates/ruff_python_parser/CONTRIBUTING.md#inline-tests).

---

_@charliermarsh reviewed on 2024-04-04 03:18_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/parser/statement.rs`:546 on 2024-04-04 03:18_

That’s really cool!

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/statement.rs`:539 on 2024-04-04 07:26_

Unrelated to this PR. I just remembered @BurntSushi's codeblock formatter PR and that markdown allows an info string. So we could consider changing this to:


```suggestion
        // ```python test_err nonlocal_stmt_trailing_comma
        // nonlocal ,
        // nonlocal x,
        // nonlocal x, y,
        // ```

        // ```python test_err nonlocal_stmt_expression
        // nonlocal x + 1
        // ```
```

It has the obvious downside that it requires an extra line and is probably only useful if editors would start highlighting code blocks in comments :(

---

_@MichaReiser approved on 2024-04-04 07:29_

We might soon have the parser tests with the highest coverage!

---

_@dhruvmanila reviewed on 2024-04-04 09:58_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/statement.rs`:539 on 2024-04-04 09:58_

Yeah that's a good idea. I mean usually editor would allow custom query to highlight certain part of the code in a different way. I know Neovim allows it with treesitter ;)

---

_Merged by @dhruvmanila on 2024-04-04 09:58_

---

_Closed by @dhruvmanila on 2024-04-04 09:58_

---

_Branch deleted on 2024-04-04 09:58_

---

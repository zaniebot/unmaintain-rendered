```yaml
number: 10757
title: Add tests for string and f-string literals
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/string-expr
created_at: 2024-04-03T12:26:51Z
updated_at: 2024-04-04T10:08:41Z
url: https://github.com/astral-sh/ruff/pull/10757
synced_at: 2026-01-12T15:55:33Z
```

# Add tests for string and f-string literals

---

_@dhruvmanila_

## Summary

This PR provides additional test cases for string and f-string parsing.

There are a few things the parser does for strings which isn't really ideal for error recovery. This PR adds test cases to showcase them (along with TODO comments) which will be helpful when working on error recovery. This requires some changes on the lexer side especially w.r.t. unterminated strings.

---

_Label `parser` added by @dhruvmanila on 2024-04-03 12:26_

---

_Label `testing` added by @dhruvmanila on 2024-04-03 12:26_

---

_Comment by @codspeed-hq[bot] on 2024-04-03 12:31_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/string-expr)

### Merging #10757 will **not alter performance**

<sub>Comparing <code>dhruv/string-expr</code> (885f65e) with <code>dhruv/parser</code> (faae192)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-04-03 12:46_

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

_Comment by @dhruvmanila on 2024-04-03 12:52_

(I need to rebase the parser stack for the ecosystem check to be in sync, I'll do it later.)

---

_Marked ready for review by @dhruvmanila on 2024-04-03 12:56_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-03 12:56_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1189 on 2024-04-04 07:59_

I think this is only true in debug mode. Should we change the assertion to `assert` (should be pretty cheap)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1248 on 2024-04-04 08:02_

Github doesn't allow me to comment on the relevant line: 

```rust
                    for string in strings {
                        values.push(match string {
                            StringType::Bytes(value) => value,
                            _ => ast::BytesLiteral::invalid(string.range()),
                        });
                    }
```

For the above code, isn't it impossible to have an element that isn't `StringType::Bytes`, given that `bytes_literal_count == strings.len()`?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:1221 on 2024-04-04 08:05_

Yeah. We probably want to convert the node to a string and mark it as "errornous". We might also need to be slightly more clever by either marking the string or byte literals as errors, depending on which nodes are fewer. But that's something we can do later.

---

_@MichaReiser approved on 2024-04-04 08:07_

---

_@dhruvmanila reviewed on 2024-04-04 09:47_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1248 on 2024-04-04 09:47_

Yeah, that's correct. I'll change it. Thanks for noticing!

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:1221 on 2024-04-04 09:48_

Yeah, I had a similar idea. I'll add this to the TODO comment.


---

_@dhruvmanila reviewed on 2024-04-04 09:48_

---

_Merged by @dhruvmanila on 2024-04-04 09:50_

---

_Closed by @dhruvmanila on 2024-04-04 09:50_

---

_Branch deleted on 2024-04-04 09:50_

---

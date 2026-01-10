```yaml
number: 19338
title: "(fix): properly handle empty comments on next physical line after line continuation"
type: pull_request
state: closed
author: soundsonacid
labels:
  - bug
  - fixes
assignees: []
base: main
head: main
created_at: 2025-07-14T19:58:52Z
updated_at: 2025-07-17T16:19:07Z
url: https://github.com/astral-sh/ruff/pull/19338
synced_at: 2026-01-10T17:58:13Z
```

# (fix): properly handle empty comments on next physical line after line continuation

---

_Pull request opened by @soundsonacid on 2025-07-14 19:58_

fixes #19326 

---

_Marked ready for review by @soundsonacid on 2025-07-14 19:59_

---

_Comment by @github-actions[bot] on 2025-07-14 20:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @ntBre on 2025-07-14 21:15_

---

_Label `bug` added by @ntBre on 2025-07-14 21:15_

---

_Label `fixes` added by @ntBre on 2025-07-14 21:15_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/empty_comment.rs`:121 on 2025-07-16 20:53_

I'm not sure, but this looks like it could cause problems on systems with different line endings. I think the easier solution would be to delete up to `line.end()` just like in the case above:


```suggestion
                    Edit::deletion(first_hash_col, line.end())
```

I'm also a bit surprised that clippy doesn't complain about this `if` nested inside of the `else`. I think we could collapse these checks like:

```rust
            if let Some(deletion_start_col) = deletion_start_col {
                Edit::deletion(line.start() + deletion_start_col, line.end())
            } else if is_on_same_logical_line {
                Edit::deletion(first_hash_col, line.end())
            } else {
                 Edit::range_deletion(locator.full_line_range(first_hash_col))
            }
```

I haven't tested this, so I could be missing an important detail. Just an idea

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/empty_comment.rs`:106 on 2025-07-16 21:23_

I found a very promising existing method called [`Indexer::preceded_by_continuation`](https://github.com/astral-sh/ruff/blob/6403eba9e907cfa1e5e0d16ad99b8d998a6750ae/crates/ruff_python_index/src/indexer.rs#L162). We should have an `Indexer` available in `check_tokens`, which is where this rule's `empty_comments` function is called. I think we should try to use that instead of manually looking at characters. And here's an existing use of that method:

https://github.com/astral-sh/ruff/blob/6403eba9e907cfa1e5e0d16ad99b8d998a6750ae/crates/ruff_linter/src/rules/pycodestyle/rules/trailing_whitespace.rs#L109-L119

I'm not sure if that will mesh well with my other suggestion, but this looks like a nice approach.

---

_@ntBre reviewed on 2025-07-16 21:25_

Thanks for working on this! I had a small suggestion and also found a helpful-looking `Indexer` method that might simplify things a bit.

---

_Closed by @soundsonacid on 2025-07-17 16:19_

---

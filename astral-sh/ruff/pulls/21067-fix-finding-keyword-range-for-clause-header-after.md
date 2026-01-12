```yaml
number: 21067
title: Fix finding keyword range for clause header after statement ending with semicolon
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: clause-semicolon
created_at: 2025-10-24T17:34:29Z
updated_at: 2025-10-27T14:52:18Z
url: https://github.com/astral-sh/ruff/pull/21067
synced_at: 2026-01-12T15:57:15Z
```

# Fix finding keyword range for clause header after statement ending with semicolon

---

_@dylwil3_

When formatting clause headers for clauses that are not their own node, like an `else` clause or `finally` clause, we begin searching for the keyword at the end of the previous statement. However, if the previous statement ended in a semicolon this caused a panic because we only expected trivia between the end of the last statement and the keyword.

This PR adjusts the starting point of our search for the keyword to begin after the optional semicolon in these cases.

Closes #21065


---

_Review requested from @MichaReiser by @dylwil3 on 2025-10-24 17:34_

---

_Label `bug` added by @dylwil3 on 2025-10-24 17:34_

---

_Label `formatter` added by @dylwil3 on 2025-10-24 17:34_

---

_@dylwil3 reviewed on 2025-10-24 17:35_

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/statement/clause.rs`:447 on 2025-10-24 17:35_

This isn't related to this PR but this doc-comment looked incorrect to me - there was nothing in the code that allowed an open parentheses, as far as I could tell.

---

_Comment by @github-actions[bot] on 2025-10-24 17:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/clause.rs`:480 on 2025-10-27 08:05_

I'm inclined to instead add an argument to `find_keyword` to force every caller to think about whether the body can contain a semicolon or not that may need to be skipped. It also avoids the need to lex the token at `end_of_statement` twice in the common case, just so that we can skip the semi.

The way I'd do it is to add a new `enum SkipSemicolon` which also gives us the opportunity to document when a semicolon needs to be skipped.

A maybe simpler API would be to change the `start_position` of `find_keywrod` to an enum `StartOffset` that has two variants

* ClauseStart(TextSize)
* LastStatement(TextSize)


The advantage of this API is that it doesn't introduce more arguments and it also allows `find_keyword` to just do the right thing. 

This might even allow us to remove some of the `unwrap` by adding a `StartOffset::end_of_last_statement(&suite)` and `StartOffset::clause_start(impl Ranged)`

---

_@MichaReiser reviewed on 2025-10-27 08:05_

---

_@MichaReiser approved on 2025-10-27 08:10_

Thanks for fixing this. I've a suggestion on how we can make this more error resilient against a future me forgeting to call `after_optional_semicolon`

---

_Review comment by @dylwil3 on `crates/ruff_python_formatter/src/statement/clause.rs`:480 on 2025-10-27 14:17_

Nice! I added `StartOffset::clause_start(impl Ranged)` but not `StartOffset::end_of_last_statement(&suite)` because it looks like each of the "last statement" examples required some custom logic depending on the statement type. I removed `after_optional_semicolon` and replaced it with inlined logic that does not re-lex the keyword in the common case.

---

_@dylwil3 reviewed on 2025-10-27 14:17_

---

_Merged by @dylwil3 on 2025-10-27 14:52_

---

_Closed by @dylwil3 on 2025-10-27 14:52_

---

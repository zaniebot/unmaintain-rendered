```yaml
number: 11732
title: Lexer should consider BOM for the start offset
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/lexer-bom-offset
created_at: 2024-06-04T07:00:35Z
updated_at: 2024-06-04T09:01:30Z
url: https://github.com/astral-sh/ruff/pull/11732
synced_at: 2026-01-12T15:55:38Z
```

# Lexer should consider BOM for the start offset

---

_@dhruvmanila_

## Summary

This PR fixes a bug where the lexer didn't consider the BOM into the start offset.

fixes: #11731

## Test Plan

Add multiple test cases which involves BOM character in the source for the lexer and verify the snapshot.


---

_Label `bug` added by @dhruvmanila on 2024-06-04 07:00_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-04 07:00_

---

_Comment by @github-actions[bot] on 2024-06-04 07:20_

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

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:114 on 2024-06-04 08:36_

I think we can simplify this by only lexing the bom if the start offset is 0. This removes the need for the subtraction (which confused me a bit).

```suggestion
        if start_offset == TextSize::new(0) {
					lexer.cursor_eat_char(BOM);
				} else {
					lexer.cursor.skip_bytes(start_offset.to_usize());
			  }
```

---

_@MichaReiser approved on 2024-06-04 08:37_

---

_Merged by @dhruvmanila on 2024-06-04 08:45_

---

_Closed by @dhruvmanila on 2024-06-04 08:45_

---

_Branch deleted on 2024-06-04 08:45_

---

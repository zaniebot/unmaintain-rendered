```yaml
number: 11716
title: Re-order lexer methods
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/reorder-lexer-methods
created_at: 2024-06-03T09:06:30Z
updated_at: 2024-06-03T13:14:14Z
url: https://github.com/astral-sh/ruff/pull/11716
synced_at: 2026-01-12T15:55:38Z
```

# Re-order lexer methods

---

_@dhruvmanila_

## Summary

This PR re-orders the lexer methods in the following order:

1. `next_token`
2. `lex_token`
3. `eat_indentation`
4. `handle_indentation`
5. `skip_whitespace`
6. `consume_ascii_character`
7. `try_single_char_prefix`
8. `try_double_char_prefix`
9. `lex_identifier`
10. `lex_fstring_start`
11. `lex_fstring_middle_or_end`
12. `lex_string`
13. `lex_number`
14. `lex_number_radix`
15. `lex_decimal_number`
16. `radix_run`
17. `lex_comment`
18. `lex_ipython_escape_command`
19. `consume_end`

Following was considered for the ordering:
* 1 is the main entry point which delegates to 2
* 3, 4, 5 are all related to whitespace which is done first
* 6 is the entrypoint for an ascii character which delegates to 9, 12, 13, 17, 18, 19
* Others are grouped around similar kind of methods

---

_Label `internal` added by @dhruvmanila on 2024-06-03 09:06_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-03 09:06_

---

_@MichaReiser approved on 2024-06-03 09:13_

Thank you. 

I would consider reordering:

* 7, 8, 9: Make 9 the first method. Similar to `consume_ascii_character` that later calls into the specific lexing methods, `lex_identifier` calls into `8` or `9`. This also aligns it, I think, with the number lexing
* 10, 11, 12: I'm not familiar enough to know this from top of my head but I would make 12 come first if it calls into 10 and/or 11. 

---

_Comment by @github-actions[bot] on 2024-06-03 09:26_

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

_Review requested from @carljm by @dhruvmanila on 2024-06-03 12:53_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-06-03 12:53_

---

_Review request for @carljm removed by @dhruvmanila on 2024-06-03 12:54_

---

_Review request for @AlexWaygood removed by @dhruvmanila on 2024-06-03 12:54_

---

_Merged by @dhruvmanila on 2024-06-03 12:58_

---

_Closed by @dhruvmanila on 2024-06-03 12:58_

---

_Branch deleted on 2024-06-03 12:58_

---

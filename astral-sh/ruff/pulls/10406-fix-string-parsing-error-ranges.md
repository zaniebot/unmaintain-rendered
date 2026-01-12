```yaml
number: 10406
title: Fix string parsing error ranges
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/fix-string-error-pos
created_at: 2024-03-14T04:13:32Z
updated_at: 2024-03-19T14:12:09Z
url: https://github.com/astral-sh/ruff/pull/10406
synced_at: 2026-01-12T15:55:32Z
```

# Fix string parsing error ranges

---

_@dhruvmanila_

## Summary

This PR fixes the error location report for string parsing.

In a lot of places we need to use an empty range because otherwise it could panic when trying to index a multibyte unicode characters.

## Test Plan

Add test cases and update the snapshots.


---

_Label `parser` added by @dhruvmanila on 2024-03-14 04:13_

---

_Closed by @dhruvmanila on 2024-03-14 09:12_

---

_Reopened by @dhruvmanila on 2024-03-14 09:12_

---

_Marked ready for review by @dhruvmanila on 2024-03-14 09:14_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-14 09:14_

---

_Comment by @github-actions[bot] on 2024-03-14 09:32_

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

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:121 on 2024-03-18 09:56_

Could you explain why we create empty ranges here instead of a range covering `c`?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:186 on 2024-03-18 09:56_

Nit
```suggestion
                TextRange::new(start_pos, self.compute_position(self.cursor - '}'.len_utf8())),
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:230 on 2024-03-18 09:57_

Nit: Should the range cover `first_char` instead of being an empty range?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:299 on 2024-03-18 09:57_

Nit: Should the range cover `ch` instead of being an empty range?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:333 on 2024-03-18 09:58_

Should this be a range covering the problematic non-ascii char instead of an empty range?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:82 on 2024-03-18 10:02_

The mix of `usize` and `TextSize` in this file results in some awkward conversions. It might be good to use `TextSize` consistently throughout the file (doesn't need to be part of this PR)

---

_@MichaReiser approved on 2024-03-18 10:02_

Looks good to me. I think it would be good to change some empty ranges to cover the actual character to get better error messages.

---

_@dhruvmanila reviewed on 2024-03-19 13:09_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/string.rs`:121 on 2024-03-19 13:09_

I didn't think of `len_utf8` which actually would give the correct value. I'll update it wherever possible.

---

_@dhruvmanila reviewed on 2024-03-19 13:16_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/string.rs`:230 on 2024-03-19 13:16_

Actually, I don't even think this code is reachable because we check for non-ascii literals at the start of parsing the byte literals:

https://github.com/astral-sh/ruff/blob/12368a29ff3cb16010de090d9537d0e7b384a9fa/crates/ruff_python_parser/src/string.rs#L329-L335

I'll remove this.

---

_@dhruvmanila reviewed on 2024-03-19 13:31_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/string.rs`:299 on 2024-03-19 13:31_

Correct me if I'm wrong but I don't think this branch is even reachable because the `memchr3` call only tries to find either `{`, `}`, or `\` which the match statement covers up.

---

_@dhruvmanila reviewed on 2024-03-19 13:40_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/string.rs`:333 on 2024-03-19 13:40_

Yes, I think we can get it using `self.source.chars().nth(index).unwrap().len_utf8()`

---

_Renamed from "Fix string error positions" to "Fix string parsing error positions" by @dhruvmanila on 2024-03-19 13:45_

---

_Renamed from "Fix string parsing error positions" to "Fix string parsing error ranges" by @dhruvmanila on 2024-03-19 13:45_

---

_@dhruvmanila reviewed on 2024-03-19 14:10_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/string.rs`:299 on 2024-03-19 14:10_

Ok, so this code _is_ reachable but still the `ch` will always be `\` because the previous match case is something like:

```rs
	'\\' if !self.kind.is_raw_string() && self.peek_byte().is_some() => ...
	ch => ...
```

I can confirm that by running the entire test suite.

---

_Merged by @dhruvmanila on 2024-03-19 14:12_

---

_Closed by @dhruvmanila on 2024-03-19 14:12_

---

_Branch deleted on 2024-03-19 14:12_

---

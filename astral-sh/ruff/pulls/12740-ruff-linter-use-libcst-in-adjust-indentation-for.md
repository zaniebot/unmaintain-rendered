```yaml
number: 12740
title: "[`ruff_linter`] - Use LibCST in `adjust_indentation` for mixed whitespace"
type: pull_request
state: merged
author: diceroll123
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: fix-RET505-indent-errors
created_at: 2024-08-08T06:26:51Z
updated_at: 2024-08-08T08:57:40Z
url: https://github.com/astral-sh/ruff/pull/12740
synced_at: 2026-01-12T15:55:42Z
```

# [`ruff_linter`] - Use LibCST in `adjust_indentation` for mixed whitespace

---

_@diceroll123_

## Summary

The `dedent_to` function has a difficult time with mixed whitespace.

The code now conditionally uses the LibCST method if there is mixed whitespace.

Fixes #12707

## Test Plan

`cargo test`

---

_Comment by @codspeed-hq[bot] on 2024-08-08 06:32_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/diceroll123:fix-RET505-indent-errors)

### Merging #12740 will **not alter performance**

<sub>Comparing <code>diceroll123:fix-RET505-indent-errors</code> (d922997) with <code>main</code> (df7345e)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-08-08 06:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-08-08 07:02_

This seems like a pretty big regression. I think I would prefer just to disable the fix in case there's mixed-indentation than regressing performance by that much.

---

_Comment by @diceroll123 on 2024-08-08 07:15_

Yeah, that is not ideal as is.

I'd like to try one more alternative approach, which does already include detecting mixed indentation.

---

_Renamed from "[`ruff_linter`] - Remove `dedent_to` usage from `adjust_indentation` helper" to "[`ruff_linter`] - Use LibCST in `adjust_indentation` for mixed whitespace" by @diceroll123 on 2024-08-08 07:48_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/fix/edits.rs`:321 on 2024-08-08 08:09_

The multiline comment is now misplaced

I would probably do:

```rust
    let contents = locator.slice(range);

    // If the range includes a multi-line string, use LibCST to ensure that we don't adjust the
    // whitespace _within_ the string.
		let contains_multiline_string = indexer.multiline_ranges().intersects(range)
        || indexer.fstring_ranges().intersects(range);

		let mixed_indentation = ...

		if mixed_indentation || contains_multiline_string {
			...
		}
```

---

_@MichaReiser approved on 2024-08-08 08:13_

Nice! 

---

_Label `bug` added by @MichaReiser on 2024-08-08 08:49_

---

_Label `preview` added by @MichaReiser on 2024-08-08 08:49_

---

_Merged by @MichaReiser on 2024-08-08 08:49_

---

_Closed by @MichaReiser on 2024-08-08 08:49_

---

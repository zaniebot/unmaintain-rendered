```yaml
number: 21329
title: "[`configuration`] Fix unclear error messages for line-length values exceeding `u16::MAX`"
type: pull_request
state: merged
author: danparizher
labels: []
assignees: []
merged: true
base: main
head: fix-21328
created_at: 2025-11-07T23:15:08Z
updated_at: 2025-11-10T18:45:29Z
url: https://github.com/astral-sh/ruff/pull/21329
synced_at: 2026-01-12T15:57:21Z
```

# [`configuration`] Fix unclear error messages for line-length values exceeding `u16::MAX`

---

_@danparizher_

## Summary

Fixed unclear error messages when `line-length` configuration values exceed `u16::MAX` (65535). Previously, values > 65535 would produce a cryptic TOML parsing error "expected u16" instead of a clear validation message. Now all invalid values produce consistent, user-friendly error messages.

Fixes #21328

## Problem Analysis

The `LineLength` type's `Deserialize` implementation attempted to deserialize directly as `u16`. When TOML contained values exceeding `u16::MAX` (65535), the TOML deserializer would fail before reaching the validation logic, resulting in unclear error messages:

- Values > 65535: `"invalid value: integer `65536`, expected u16"` (unclear)
- Values 321-65535: `"line-length must be between 1 and 320 (got 500)"` (clear)

This inconsistency violated the TOML spec's requirement to accept arbitrary 64-bit signed integers and handle validation errors gracefully.

## Approach

Modified the `Deserialize` implementation for `LineLength` in `crates/ruff_linter/src/line_width.rs` to:

1. **Deserialize as `u64` first**: Accept any valid TOML integer value
2. **Validate u16 bounds**: Check if the value exceeds `u16::MAX` before conversion
3. **Provide consistent errors**: All out-of-range values now produce the same clear error message format: `"line-length must be between 1 and 320 (got {value})"`

Added test cases covering:
- Boundary case: `line-length = 65535` (at u16::MAX)
- Exceeds u16::MAX: `line-length = 65536`
- Far exceeds u16::MAX: `line-length = 99_999`

All tests verify that the error messages are clear and consistent regardless of whether the value exceeds u16::MAX or just exceeds the valid range (1-320).

---

_Renamed from "[`configuration`] Fix unclear error messages for line-length values exceeding u16::MAX" to "[`configuration`] Fix unclear error messages for line-length values exceeding `u16::MAX`" by @danparizher on 2025-11-07 23:15_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/line_width.rs`:66 on 2025-11-10 09:44_

```suggestion
        // Convert to u16 and validate range
        // Safe to unwrap since we've already checked value <= u16::MAX
        let value = u16::try_from(value).map_err(|_| serde::de::Error::custom(format!(
            "line-length must be between 1 and {} (got {value})",
            Self::MAX,
        )))?;
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/line_width.rs`:66 on 2025-11-10 09:46_

But I suspect we can even merge this with the next branch by doing something like this
```suggestion
				u16::try_from(value)
					.and_then(|value| Self::try_from(value))
					.map_err(|_| serde::de::Error::custom(format!(
                "line-length must be between 1 and {} (got {value})",
                Self::MAX,
          )))
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/line_width.rs`:54 on 2025-11-10 09:46_

Should we use an `i64` here to also handle `-5`

---

_@MichaReiser requested changes on 2025-11-10 09:46_

---

_@MichaReiser approved on 2025-11-10 18:24_

---

_Comment by @MichaReiser on 2025-11-10 18:27_

Thank you. 

For us reviewers, it's very helpful to get feedback when suggestions don't work (I obviously didn't realize that Rust won't like `and_then` over different error types) and/or resolve suggestions. It helps us understand which comments were addressed and, if not, why not (without having to clone the PR and applying the changes manually). 


---

_Merged by @MichaReiser on 2025-11-10 18:29_

---

_Closed by @MichaReiser on 2025-11-10 18:29_

---

_Branch deleted on 2025-11-10 18:45_

---

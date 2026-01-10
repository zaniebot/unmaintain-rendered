```yaml
number: 21072
title: "Clearer error message when `line-length` goes beyond threshold"
type: pull_request
state: merged
author: ShaharNaveh
labels: []
assignees: []
merged: true
base: main
head: invalid-line-len-err
created_at: 2025-10-25T10:54:38Z
updated_at: 2025-10-27T07:49:04Z
url: https://github.com/astral-sh/ruff/pull/21072
synced_at: 2026-01-10T16:59:49Z
```

# Clearer error message when `line-length` goes beyond threshold

---

_Pull request opened by @ShaharNaveh on 2025-10-25 10:54_

Fixes #20823 

## Summary
Implemented code from original issue

## Test Plan

Added test

---

_Comment by @github-actions[bot] on 2025-10-25 11:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @chirizxc on 2025-10-25 11:06_

My implementation example doesn't seem quite right.

I don't think we should prohibit people from setting `line-length` to more than 320, maybe we should just give a warning? (but it will change the current behavior)

---

_Comment by @ShaharNaveh on 2025-10-25 11:22_

> My implementation example doesn't seem quite right.
> 
> I don't think we should prohibit people from setting `line-length` to more than 320, maybe we should just give a warning? (but it will change the current behavior)

IMO, there's no need for having artificial constraints about the max size of the line-length, or a need to set a warning, the way that I see it, if a user explicitly set `line-length` to 10000 he knows what he's doing (I hope so). I am not saying that there's a need for increasing the possible length (by utilizing NonZerou32).

---

One possible solution would be to remove
https://github.com/astral-sh/ruff/blob/64ab79e5721ec6fdd2182fbf9d39a26534ccca43/crates/ruff_linter/src/line_width.rs#L24-L25

And relying on the constrains of `NonZerou16` at:
https://github.com/astral-sh/ruff/blob/64ab79e5721ec6fdd2182fbf9d39a26534ccca43/crates/ruff_linter/src/line_width.rs#L94-L103

---

_Comment by @chirizxc on 2025-10-25 12:00_

It seems that the root of the problem lies in crate [toml](https://github.com/toml-rs/toml/blob/6df349bffa72263ca25bfacd7016420749c94174/crates/toml/src/value.rs#L32).

---

_Comment by @chirizxc on 2025-10-25 12:08_

although it seems that the TOML specification [mentions this](https://toml.io/en/v1.0.0#integer)

> Non-negative integer values may also be expressed in hexadecimal, octal, or binary. In these formats, leading + is not allowed and leading zeros are allowed (after the prefix). Hex values are case-insensitive. Underscores are allowed between digits (but not between the prefix and the value).
```toml
# hexadecimal with prefix `0x`
hex1 = 0xDEADBEEF
hex2 = 0xdeadbeef
hex3 = 0xdead_beef

# octal with prefix `0o`
oct1 = 0o01234567
oct2 = 0o755 # useful for Unix file permissions

# binary with prefix `0b`
bin1 = 0b11010110
```
Arbitrary 64-bit signed integers (from −2^63 to 2^63−1) should be accepted and handled losslessly. If an integer cannot be represented losslessly, an error must be thrown.

---

_Comment by @ShaharNaveh on 2025-10-25 13:51_

> If an integer cannot be represented losslessly, an error must be thrown.

So if I understand you correctly it will be something like:

```rust
let raw = match i64::deserialize(deserializer) {
  Ok(v) => v,
  Err(_) => {
    let s: &str = Deserializer::deserialize(deserializer)?;
    i64::from_str(s).map_err(ParseLineWidthError::ParseError)?
  }
}

let value = u16::try_from(raw).map_err(???)
```

The main issue with this approach is that 
https://github.com/astral-sh/ruff/blob/64ab79e5721ec6fdd2182fbf9d39a26534ccca43/crates/ruff_linter/src/line_width.rs#L90-L93
can only store `u16` and in the best case with this approach we'll have `i64`. It doesn't make much sense to change the type to `i64`. what does deserlizing to i64 first gives us? why can't we just do what you suggested initially?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/line_width.rs`:62 on 2025-10-25 14:59_

Nit: Maybe something like this:

```suggestion
        let value = Self::try_from(value).map_err(|_| {
            serde::de::Error::custom(format!(
                "line-length must be between 1 and {} (got {value})",
                Self::MAX,
            ))
        })
```

---

_@MichaReiser reviewed on 2025-10-25 15:00_

---

_Review requested from @MichaReiser by @ShaharNaveh on 2025-10-26 10:06_

---

_@MichaReiser approved on 2025-10-27 07:37_

Thank you

---

_Merged by @MichaReiser on 2025-10-27 07:42_

---

_Closed by @MichaReiser on 2025-10-27 07:42_

---

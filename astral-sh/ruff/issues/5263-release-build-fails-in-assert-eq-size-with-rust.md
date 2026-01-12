```yaml
number: 5263
title: "Release build fails in assert_eq_size! with Rust nightly ≥ 2023-06-09"
type: issue
state: closed
author: andersk
labels: []
assignees: []
created_at: 2023-06-21T18:48:13Z
updated_at: 2023-06-21T21:26:11Z
url: https://github.com/astral-sh/ruff/issues/5263
synced_at: 2026-01-12T15:54:45Z
```

# Release build fails in assert_eq_size! with Rust nightly ≥ 2023-06-09

---

_@andersk_

```
error[E0512]: cannot transmute between types of different sizes, or dependently-sized types
   --> crates/ruff_formatter/src/format_element.rs:449:5
    |
449 |     assert_eq_size!(crate::format_element::Tag, [u8; 16]);
    |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    |
    = note: source type: `Tag` (192 bits)
    = note: target type: `[u8; 16]` (128 bits)
    = note: this error originates in the macro `assert_eq_size` (in Nightly builds, run with -Z macro-backtrace for more info)

error[E0512]: cannot transmute between types of different sizes, or dependently-sized types
   --> crates/ruff_formatter/src/format_element.rs:452:5
    |
452 |     assert_eq_size!(crate::FormatElement, [u8; 24]);
    |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    |
    = note: source type: `format_element::FormatElement` (256 bits)
    = note: target type: `[u8; 24]` (192 bits)
    = note: this error originates in the macro `assert_eq_size` (in Nightly builds, run with -Z macro-backtrace for more info)

For more information about this error, try `rustc --explain E0512`.
error: could not compile `ruff_formatter` (lib) due to 2 previous errors
```

This is because Rust inflated `TypeId` from 64 bits to 128 bits (rust-lang/rust#109953).

The affected `format_element::tag::LabelId` struct is currently never constructed and its `id: TypeId` field is never used. I don’t know what you plan to use this for, but does it need the extensibility of `TypeId`, or would a simple `enum` suffice?

---

_Comment by @charliermarsh on 2023-06-21 18:49_

\cc @MichaReiser 

---

_Comment by @MichaReiser on 2023-06-21 20:32_

Thanks for reporting this new issue. According to https://github.com/rust-lang/compiler-team/issues/608, the main reason of the change is to avoid collisions between two type ids. 

> With a 64 bit hash, the probability of collision is 1 in 2^32 (due to the birthday bound) -- 1 in roughly 4 billion.

I recommend that we simply hash the `TypeId` and store a `u64` in `Labelid` instead. The risk of a collision is extremely low because:

* All `Label` types are local to a single crate
* Each language only uses a few Labels (Rome uses 2-3?)
* Label's are mainly used to *mark* some content and immediately test in the parent formatter if it has been set. It's rare (or even non-existing?) that a formatter matches on multiple labels (is it label A or B). 

We can further mitigate this issue by manually implement `PartialEq` and also assert that the type names are equal to get a compile error if the assumption does not uphold.

> or would a simple enum suffice?

We aren't using an `enum` (we could) because `ruff_formatter` is supposed to be language agnostic (you can use it to implement formatting for many languages. The formatter even uses itself to format the IR). Switching to an enum would introduce language-specific code and limits the extensibility because adding a new label becomes restricted to those that can change `ruff_formatter`. I don't think this is a huge concern since we're only supporting a single language, but it is regressing the design.

Edit: Another alternative is to use string labels. String labels have the downside that collisions are even more likely (because someone accidentally re-uses an existing name) and comparing strings is more expensive than comparing numbers.

The safest (and least clever?) may be to introduce a new trait that defines two methods:

* `value() -> u64`: Returns the `Label` value
* `debug_text() -> &'static str`

`LabelId` then only accepts a type implementing the new trait and calls into `value` and `debug_text` internally. This  avoids collisions as long as each formatter only uses a single enum that defines all labels. Mixing labels between formatters is discouraged anyway

---

_Closed by @MichaReiser on 2023-06-21 21:26_

---

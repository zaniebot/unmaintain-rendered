```yaml
number: 3807
title: "Port `default_value_t` to the Builder API"
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - A-builder
  - S-waiting-on-design
assignees: []
created_at: 2022-06-09T13:54:56Z
updated_at: 2022-06-09T13:56:46Z
url: https://github.com/clap-rs/clap/issues/3807
synced_at: 2026-01-12T16:14:15Z
```

# Port `default_value_t` to the Builder API

---

_@epage_

With #3732, we now have a typed API.  This means we can move the derive's `default_value_t` down into the builder API

This would supersede #2813

---

_Label `C-enhancement` added by @epage on 2022-06-09 13:54_

---

_Label `A-builder` added by @epage on 2022-06-09 13:54_

---

_Label `S-waiting-on-design` added by @epage on 2022-06-09 13:54_

---

_Comment by @epage on 2022-06-09 13:56_

This requires the ability to render a string for help.

I would suggest `TypedValueParser` gaining
```rust
fn display(&self) -> Option<String> {
    None
}
```

We would then provide it for all of the built-in value parsers.  The question is if we should make `ToString` a requirement for the `FromStr` blanket impl.

---

_Referenced in [clap-rs/clap#3808](../../clap-rs/clap/issues/3808.md) on 2022-06-09 14:01_

---

_Referenced in [clap-rs/clap#3809](../../clap-rs/clap/issues/3809.md) on 2022-06-09 14:04_

---

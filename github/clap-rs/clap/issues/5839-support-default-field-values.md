---
number: 5839
title: Support default field values
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - E-medium
  - A-derive
assignees: []
created_at: 2024-12-10T19:19:49Z
updated_at: 2024-12-10T21:03:37Z
url: https://github.com/clap-rs/clap/issues/5839
synced_at: 2026-01-07T13:12:20-06:00
---

# Support default field values

---

_Issue opened by @epage on 2024-12-10 19:19_

Default field values would provide an alternative to `default_value_t`

From the [RFC](https://github.com/rust-lang/rfcs/blob/master/text/3681-default-field-values.md) (why is an RFC in 2024 using structopt?)
```rust
#[derive(Debug, StructOpt)]
#[structopt(name = "example", about = "An example of StructOpt usage.")]
struct Opt {
    /// Set speed
    #[structopt(short = "s", long = "speed", default_value_t = 42)]
    speed: f64,
    ...
}
```

`default_value_t` should have higher precedence

Open questions
- How do we handle this while its nightly only?
  - Put it behind a `nightly` feature?
- How do users opt-out of using the default field value?

---

_Label `C-enhancement` added by @epage on 2024-12-10 19:19_

---

_Label `E-medium` added by @epage on 2024-12-10 19:19_

---

_Label `A-derive` added by @epage on 2024-12-10 19:19_

---

_Comment by @jhpratt on 2024-12-10 19:57_

> why is an RFC in 2024 using structopt?

The RFC work started in 2018 😅

---

_Comment by @jalil-salame on 2024-12-10 21:03_

I don't see a use for different `default_value_t` and a struct default's value, so I would fire a warning/deny by default lint ideally (I'd love a `compiler_warning!()` macro)

---

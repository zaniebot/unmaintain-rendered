```yaml
number: 891
title: "arg_enum! case insensitivity doesn't work with `possible_values`"
type: issue
state: closed
author: LegNeato
labels: []
assignees: []
created_at: 2017-03-07T19:53:43Z
updated_at: 2018-08-02T03:30:03Z
url: https://github.com/clap-rs/clap/issues/891
synced_at: 2026-01-12T16:14:10Z
```

# arg_enum! case insensitivity doesn't work with `possible_values`

---

_@LegNeato_

Enum case insensitivity was add in https://github.com/kbknapp/clap-rs/issues/104.

Unfortunetly, if you use `possible_values()` with your enum (for example, `possible_values(&MyEnum::variants())`) case insensitivity is lost.

This means the helpful `clap` error message listing supported values cannot be used and one must handle it manually.

### Rust Version

`rustc 1.15.1 (021bd294c 2017-02-08)`

### Affected Version of clap

`clap 2.20.3`

### Expected Behavior Summary

Because the enums are case-insensitive, I expected `possible_values` to be case insensitive as well.

### Actual Behavior Summary

Using `possible_values` makes enums act like they are case-sensitive again.

### Steps to Reproduce the issue

1. Create an enum using `arg_enum!`
2. Make sure a variant has a capital letter
3. Create an arg, set `possible_values` to the enum
4. See that clap treats `foo` and `Foo` input values differently

---

_Label `T: RFC / question` added by @kbknapp on 2017-03-10 01:29_

---

_Comment by @kbknapp on 2017-03-10 01:29_

Unfortunately, using `possible_values` has always been case sensitive. This is partly by design though, as there may exist cases where people wish to support different values which differ only in capitalization. Assuming you only want to support lowercase variants (which is the most common case by far), you could use something like 

```rust
&MyEnum::variants().iter()
                   .map(std::ascii::AsciiExt::to_ascii_lowercase)
                   .collect::<Vec<_>>()
```

---

_Comment by @LegNeato on 2017-03-10 21:49_

Ah, ok. It was a bit counter-intuitive to read the issues about adding case-insentivity to `arg_enum` but still seeing case-sensitive behavior.

---

_Closed by @LegNeato on 2017-03-10 21:49_

---

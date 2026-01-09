---
number: 5245
title: "Feature Request : Add range validation for float types"
type: issue
state: closed
author: RaulTrombin
labels:
  - C-enhancement
  - A-parsing
assignees: []
created_at: 2023-12-04T12:58:48Z
updated_at: 2023-12-04T16:07:55Z
url: https://github.com/clap-rs/clap/issues/5245
synced_at: 2026-01-07T13:12:20-06:00
---

# Feature Request : Add range validation for float types

---

_Issue opened by @RaulTrombin on 2023-12-04 12:58_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.4.6

### Describe your use case

Actually value_parser! only have .range() setup for unsigned int and int types.
[value_parser](https://docs.rs/clap/latest/clap/struct.Arg.html#method.value_parser)


### Describe the solution you'd like

Would like that clap validade some inputs like:

--set-frequency 300
Should not accept.

--set-frequency 0.1
Ok.

...
.value_parser(clap::value_parser!(f64).range(...200.0))
...

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @RaulTrombin on 2023-12-04 12:58_

---

_Comment by @epage on 2023-12-04 15:31_

The built-in `range` is mostly to help give good error messages when dealing with smaller types like `u8` (we parse it as a `u64` and then range cap it to `u8`)

I'd very much be interested in seeing crates created explore additional value parsers. like https://docs.rs/clio/latest/clio/, and we can consider what might be worth moving into clap.  One reason to be cautious is that the more code we have (even if unused), the worse the impact on compile times.

---

_Closed by @epage on 2023-12-04 15:31_

---

_Label `A-parsing` added by @epage on 2023-12-04 15:31_

---

_Comment by @RaulTrombin on 2023-12-04 15:59_

@epage So the easiest way would be to create some special crate for fields validation with type, range and sets?
Then it could be enabled as a feature for clap or something like?

---

_Comment by @epage on 2023-12-04 16:07_

It wouldn't be enabled as a feature of clap but could integrate with clap by just passing a value constructed from this other crate into clap

---

---
number: 4946
title: "`ValueEnum` drops trailing/leading underscores"
type: issue
state: closed
author: dhruvkb
labels:
  - C-bug
  - A-derive
  - S-wont-fix
assignees: []
created_at: 2023-06-02T08:26:39Z
updated_at: 2023-06-02T14:07:26Z
url: https://github.com/clap-rs/clap/issues/4946
synced_at: 2026-01-10T01:28:03Z
---

# `ValueEnum` drops trailing/leading underscores

---

_Issue opened by @dhruvkb on 2023-06-02 08:26_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.69.0 (84c898d65 2023-04-16)

### Clap Version

4.3.0

### Minimal reproducible code

I have an enum that has 4 variants, two regular, and two with a trailing underscore (to indicate reverse, in my app).

```rust
use clap::ValueEnum;

#[derive(Debug, Copy, Clone, ValueEnum)]
#[allow(non_camel_case_types)]
pub enum SortBasis {
  Name,
  Age,
  Name_,
  Age_,
}
```

I use this enum in my parser.

```
#[derive(Debug, clap::Args)]
#[group()]
#[clap(next_help_heading = "Sorting")]
pub struct Sorting {
	/// the set of fields to sort by
	#[clap(short, long, value_enum, default_values = ["name", "age"])]
	pub sort: Vec<SortBasis>,
}
```



### Steps to reproduce the bug with the above code

The output of help, which can be seen by running `cargo run -- -h` is as follows.

```
Sorting:
  -s, --sort <SORT>  the set of fields to sort by [default: name age] [possible values: name, age, name, age]
```

### Actual Behaviour

I expect there to be a way to preserve leading/trailing underscores, while converting the variants to snake/kebab case.

### Expected Behaviour

Verbatim casing only kind-of works but then I need to break convention, define every variant in lowercase and precisely how I want the user to type it.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @dhruvkb on 2023-06-02 08:26_

---

_Comment by @epage on 2023-06-02 14:07_

Workaround: `#[value(name = "name_")]`

Differentiating values based on trailing `_` seems like a UX we shouldn't be generally encouraging.  So long as we have a path for people to workaround it when they have a good reason within their application, I'm inclined to say we shouldn't change this.  If there is a reason we should reconsider this, let us know!

---

_Closed by @epage on 2023-06-02 14:07_

---

_Label `A-derive` added by @epage on 2023-06-02 14:07_

---

_Label `S-wont-fix` added by @epage on 2023-06-02 14:07_

---

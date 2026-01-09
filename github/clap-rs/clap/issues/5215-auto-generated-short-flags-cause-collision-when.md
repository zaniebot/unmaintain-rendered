---
number: 5215
title: Auto-generated Short Flags Cause Collision When Args Start With the Same Letter
type: issue
state: closed
author: Ghvstcode
labels:
  - C-enhancement
  - A-derive
assignees: []
created_at: 2023-11-15T11:39:26Z
updated_at: 2023-11-15T14:21:17Z
url: https://github.com/clap-rs/clap/issues/5215
synced_at: 2026-01-07T13:12:20-06:00
---

# Auto-generated Short Flags Cause Collision When Args Start With the Same Letter

---

_Issue opened by @Ghvstcode on 2023-11-15 11:39_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.4.8

### Describe your use case

When deriving command-line arguments, there appears to be an issue with the automatic generation of short flag identifiers. Specifically, if two arguments start with the same letter and the short flag (short) is not explicitly specified for them, clap defaults to using the first letter of each argument as the short flag. This results in a collision, as both arguments end up with the same short flag identifier, leading to the error: "Short option names must be unique for each argument".
```Rust
use clap::Parser;

// My CLI Program
#[derive(Parser, Debug)]
#[clap(author, version, about, long_about = None)]
struct Args {
    /// An example option
    #[clap(short, long)]
    option: String,
    /// An example flag
    #[clap(short, long)]
    online: bool,
}
```
both **option** and **online** arguments start with the letter 'o'. Since short is not specified, clap defaults to using 'o' as the short flag for both, causing a conflict.

### Describe the solution you'd like

Ideally, clap should handle such scenarios more gracefully. If two arguments begin with the same letter and no explicit short flags are provided, it could automatically generate unique short flags. One possible approach is to use the first two letters of the argument names for subsequent arguments that start with the same letter. For example, in the case above, **option** could default to **o**, and **online** could default to **on**.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @Ghvstcode on 2023-11-15 11:39_

---

_Comment by @epage on 2023-11-15 14:21_

You should get asserts in debug builds about duplicates.  #3133 is about making those asserts compiler errors.

Note that we do not support multi-character shorts (like `-on`).

I think this is case where it is better to fail error and have the developer fix it by explicitly specifying a short, rather than to pick something.  The CLI should be specifically designed, rather than getting incidental behavior.  For example, with this proposal, just changing the order that the fields are specified would change the CLI.

I'm going to go ahead and close this as I think its best we not go a route like this.  If there is a reason for us to re-evaluate, let us know!

---

_Closed by @epage on 2023-11-15 14:21_

---

_Label `A-derive` added by @epage on 2023-11-15 14:21_

---

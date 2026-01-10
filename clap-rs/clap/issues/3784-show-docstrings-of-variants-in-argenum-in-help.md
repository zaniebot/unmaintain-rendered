---
number: 3784
title: Show Docstrings of variants in ArgEnum in --help
type: issue
state: closed
author: nipunn1313
labels:
  - C-enhancement
assignees: []
created_at: 2022-06-02T22:52:38Z
updated_at: 2022-06-04T12:03:31Z
url: https://github.com/clap-rs/clap/issues/3784
synced_at: 2026-01-10T01:27:46Z
---

# Show Docstrings of variants in ArgEnum in --help

---

_Issue opened by @nipunn1313 on 2022-06-02 22:52_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.1.14

### Describe your use case

Would like something like this:
```
#[derive(Parser, Debug)]
struct Args {
    /// Choice of variant
    #[clap(long, arg_enum)]
    v: Variants
}

#[derive(ArgEnum, Clone, Copy, Debug)]
pub enum Variants {
    /// Variant 1
    V1,
    /// Variant 2
    V2,
    /// Variant 3
    V3,
}

fn main() {
  let args = Args::parse();
}
```

### Describe the solution you'd like

I would ideally like `--help` to show the description of the variants (V1, V2, V3).
Currently, it just shows the docstring for `v: Variants`

### Alternatives, if applicable

Could also work to have this be gated behind a derive arg attribute on the struct

### Additional Context

_No response_

---

_Label `C-enhancement` added by @nipunn1313 on 2022-06-02 22:52_

---

_Comment by @epage on 2022-06-03 00:07_

I believe this is a duplicate of https://github.com/clap-rs/clap/issues/3312 which was implemented in https://github.com/clap-rs/clap/pull/3503 which was released in 3.1.4. 

It is gated behind `unstable-v4` as we were uncomfortable with taking a doc string we previously ignored and starting to put it in the help. 

---

_Closed by @epage on 2022-06-03 00:07_

---

_Comment by @nipunn1313 on 2022-06-03 00:55_

thanks! Appreciate the team behind clap

---

_Comment by @hulxv on 2022-06-04 03:33_

> I believe this is a duplicate of #3312 which was implemented in #3503 which was released in 3.1.4.
> 
> It is gated behind `unstable-v4` as we were uncomfortable with taking a doc string we previously ignored and starting to put it in the help.

Is it possible to do it by **derive** way like that's example
```rust
#[derive(Parser, Debug)]
struct Args {
    /// Choice of variant
    #[clap(long, arg_enum)]
    v: Variants
}

#[derive(ArgEnum, Clone, Copy, Debug)]
pub enum Variants {
    /// Variant 1
    V1,
    /// Variant 2
    V2,
    /// Variant 3
    V3,
}

fn main() {
  let args = Args::parse();
}
```

 or that's available for **builder** only now?

---

_Comment by @epage on 2022-06-04 12:03_

Yes, the derive will automatically pick up doc comments.  While the PR didn't explicitly add any code for that, it picked it up through shared code.  I'm adding an explicit test for it.

---

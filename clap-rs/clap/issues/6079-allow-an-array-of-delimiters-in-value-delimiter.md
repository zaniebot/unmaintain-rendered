```yaml
number: 6079
title: "Allow an array of delimiters in `value_delimiter`"
type: issue
state: open
author: Andrew15-5
labels:
  - C-enhancement
  - S-waiting-on-decision
  - A-parsing
assignees: []
created_at: 2025-07-29T08:33:52Z
updated_at: 2025-07-29T19:02:29Z
url: https://github.com/clap-rs/clap/issues/6079
synced_at: 2026-01-12T16:14:17Z
```

# Allow an array of delimiters in `value_delimiter`

---

_@Andrew15-5_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

master

### Describe your use case

As I found out, `cargo` use a custom parsing for, e.g., `--features` by splitting each argument with comma and whitespace and then flattening an array of the split stuff (if flag was used several times).

https://github.com/clap-rs/clap/discussions/6076#discussioncomment-13919535

That is despite using clap, which does have `value_delimiter`. However, you can only split using a single `char` (or even string?) right now. In Dioxus, [there are several places](https://github.com/clap-rs/clap/discussions/6076#discussion-8632368) where splitting works and doesn't work automatically. Some of this is probably due to downstream further parsing, like with `--features` that is sent to `cargo` through `Command`.

### Describe the solution you'd like

I want to be able to write `value_delimiter = [' ', ',']` and get all the split arguments to be concatenated into a single `Vec<String>`, automatically being flattened. This provides great flexibility in how you can pass arguments to the same flag.

### Alternatives, if applicable

The only alternative would be [this](https://github.com/rust-lang/cargo/blob/37eeab7c165aaccc7c70dfdf2bbd239cbd2b7269/src/bin/cargo/commands/add.rs#L282-L287).

### Additional Context

_No response_

---

_Label `C-enhancement` added by @Andrew15-5 on 2025-07-29 08:33_

---

_Referenced in [clap-rs/clap#6080](../../clap-rs/clap/issues/6080.md) on 2025-07-29 08:42_

---

_Label `S-waiting-on-decision` added by @epage on 2025-07-29 19:02_

---

_Label `A-parsing` added by @epage on 2025-07-29 19:02_

---

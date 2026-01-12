```yaml
number: 3588
title: "Make TryFrom::try_from the default for try_from_os_str annotation"
type: issue
state: closed
author: aj-bagwell
labels:
  - C-enhancement
  - S-waiting-on-decision
  - A-derive
assignees: []
created_at: 2022-03-29T11:59:01Z
updated_at: 2022-05-25T19:58:45Z
url: https://github.com/clap-rs/clap/issues/3588
synced_at: 2026-01-12T16:14:15Z
```

# Make TryFrom::try_from the default for try_from_os_str annotation

---

_@aj-bagwell_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.1.6

### Describe your use case

All the [kinds](https://github.com/clap-rs/clap/blob/v3.1.6/examples/derive_ref/README.md#arg-types) that can be used with the `parse` annotation have a default except for `try_from_os_str`. I think the most logical default would be [std::convert::TryFrom::try_from](https://doc.rust-lang.org/std/convert/trait.TryFrom.html#tymethod.try_from). There don't seem to be and types that implement `TryFrom<&OsStr>` in the standard library, but it is the most obviouse trait to implement for things like file names that take a path but may not be valid.

### Describe the solution you'd like

    use clio::{Input, Output}
    struct Opt {
        /// Would like to be able to do this
        #[clap(parse(try_from_os_str))]
        in_file: Input,
    
        /// Instead of this
        #[clap(parse(try_from_os_str = Output::try_from))]
        out_file: Output,
    }

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @aj-bagwell on 2022-03-29 11:59_

---

_Label `S-waiting-on-decision` added by @epage on 2022-03-29 14:31_

---

_Label `A-derive` added by @epage on 2022-03-29 14:31_

---

_Comment by @epage on 2022-03-29 14:32_

While there might be a default for us to use, overall researching and deciding on this seems like a fairly low priority because `try_from_os_str` is a fairly low use case.  The few people who use it can just specify it directly.

---

_Comment by @epage on 2022-05-25 19:58_

With #3732, the `parse` attribute is being phased out in favor of `value_parser`,, so I'm closing this issue.

---

_Closed by @epage on 2022-05-25 19:58_

---

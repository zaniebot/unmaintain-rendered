```yaml
number: 3620
title: "derive: make boolean `clap()` args (`require`, `global`, ...) default to true if specified"
type: issue
state: open
author: martinvonz
labels:
  - C-enhancement
  - A-derive
  - S-waiting-on-design
assignees: []
created_at: 2022-04-09T16:36:35Z
updated_at: 2022-04-09T19:29:15Z
url: https://github.com/clap-rs/clap/issues/3620
synced_at: 2026-01-12T16:14:15Z
```

# derive: make boolean `clap()` args (`require`, `global`, ...) default to true if specified

---

_@martinvonz_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.1.8

### Describe your use case

I'm using the Derive API to define a struct similar to this:

```rust
#[derive(clap::Parser)]
struct Args {
    #[clap(long, short, global = true)]
    verbose: bool,
    #[clap(required = true)]
    file: String,
}
```


### Describe the solution you'd like

I would expect that I could just say `global` and `required` there without needing the `= true`. That seems consistent with `long` and `short`. I can kind of see how `long` and `short` are different because they translate to non-boolean methods, but that's not how I think of it when I look at the struct.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @martinvonz on 2022-04-09 16:36_

---

_Label `A-derive` added by @epage on 2022-04-09 19:27_

---

_Label `S-waiting-on-design` added by @epage on 2022-04-09 19:27_

---

_Comment by @epage on 2022-04-09 19:29_

Leaving off the arg means there is a default.  I think `true` for bools is a sane default.

The safest option would be to hard code the list of options that are bools for being defaulted.  The downside is this will be a pain to maintain.

We could just say that if there is no `=`, we just assume `= true` and rely on the compiler error for non-bool types.  The downsides are that errors won't be as nice for non-bools and if an arg uses an `Into` that handles bools, we might apply the wrong default.

---

_Referenced in [clap-rs/clap#3726](../../clap-rs/clap/issues/3726.md) on 2022-05-13 12:01_

---

_Referenced in [clap-rs/clap#4064](../../clap-rs/clap/issues/4064.md) on 2022-08-11 16:59_

---

_Referenced in [clap-rs/clap#4688](../../clap-rs/clap/pulls/4688.md) on 2023-02-01 17:19_

---

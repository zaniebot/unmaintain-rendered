---
number: 3867
title: Documentation for value_enum how to print possible values in help
type: issue
state: closed
author: stepancheg
labels:
  - C-enhancement
assignees: []
created_at: 2022-06-25T12:21:57Z
updated_at: 2022-06-25T12:25:28Z
url: https://github.com/clap-rs/clap/issues/3867
synced_at: 2026-01-10T01:27:47Z
---

# Documentation for value_enum how to print possible values in help

---

_Issue opened by @stepancheg on 2022-06-25 12:21_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.2.5

### Describe your use case

```
#[derive(ValueEnum, Clone)]
enum XxxMode {
}

    #[clap(
        long,
        value_enum,
    )]
    mode: XxxMode,
```

`--help` does not print possible values of enum.

### Describe the solution you'd like

The solution I've found:

```
    #[clap(
        long,
        value_enum,
        possible_values = XxxMode::value_variants()
            .iter()
            .map(|v| v.to_possible_value().unwrap()),
    )]
    mode: XxxMode,
```

But it is not mentioned anywhere in documentation (e.g. in derive reference, or next to `ValueEnum` documentation).

(Also it should probably be default behavior to print possible values for `value_enum`).

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @stepancheg on 2022-06-25 12:21_

---

_Comment by @stepancheg on 2022-06-25 12:25_

Sorry, it works without this trick. Something didn't work before, but I don't know what I did when it didn't work.

---

_Closed by @stepancheg on 2022-06-25 12:25_

---

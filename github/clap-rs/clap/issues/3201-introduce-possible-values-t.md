---
number: 3201
title: Introduce possible_values_t
type: issue
state: closed
author: Gordon01
labels:
  - C-enhancement
  - E-medium
  - A-derive
assignees: []
created_at: 2021-12-21T12:35:02Z
updated_at: 2022-05-25T20:03:39Z
url: https://github.com/clap-rs/clap/issues/3201
synced_at: 2026-01-07T13:12:19-06:00
---

# Introduce possible_values_t

---

_Issue opened by @Gordon01 on 2021-12-21 12:35_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.0-rc.7

### Describe your use case

`possible_values` are not chacked for ranges at compile time, like a `default_value_t` and this slightly reduce the robustness of the code.

For example, this code compiles and fails at runtime:
```
#[clap(short, long, default_value_t = 16, possible_values = ["16", "512"])]
fat: u8,
```

### Describe the solution you'd like

The following code should result in error like this:
```
#[clap(short, long, default_value_t = 16, possible_values_t = [16, 512])]
fat: u8,

error: literal out of range for `u8`
note: the literal `512` does not fit into the type `u8` whose range is `0..=255`
```

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @Gordon01 on 2021-12-21 12:35_

---

_Referenced in [clap-rs/clap#3202](../../clap-rs/clap/issues/3202.md) on 2021-12-21 12:36_

---

_Label `A-derive` added by @epage on 2021-12-21 14:21_

---

_Comment by @epage on 2021-12-21 14:22_

A not-great workaround until this is implemented is to use `ArgEnum` where you `#[clap(name = "N")]` each variant.

I've normally thought of possible values for enums.  It makes sense to also support numbers.  I am curious though, what is your use case for only accepting specific numbers?

---

_Label `E-medium` added by @epage on 2021-12-21 14:22_

---

_Comment by @Gordon01 on 2021-12-21 18:11_

> I am curious though, what is your use case for only accepting specific numbers?

FAT filesystem type (12, 16 or 32)

---

_Comment by @epage on 2022-05-17 21:48_

#3732 will have an impact on how we design / expose this

---

_Comment by @epage on 2022-05-25 20:03_

With #3732 and friends, `possible_values` has been deprecated and replaced by `value_parser`, so I'm closing this out

---

_Closed by @epage on 2022-05-25 20:03_

---

---
number: 2631
title: Typo in validator example
type: issue
state: closed
author: Ebedthan
labels:
  - A-docs
  - E-easy
assignees: []
created_at: 2021-07-27T22:59:39Z
updated_at: 2021-07-28T20:40:19Z
url: https://github.com/clap-rs/clap/issues/2631
synced_at: 2026-01-10T01:27:22Z
---

# Typo in validator example

---

_Issue opened by @Ebedthan on 2021-07-27 22:59_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

2.33

### Where?

https://github.com/clap-rs/clap/blob/master/examples/15_custom_validator.rs#L24

### What's wrong?

The example says that the argument for the custom validator should be a String while in the example the argument is &str (val: &str while expected val: String), which can lead to confusion for beginners.

### How to fix?

Change the argument from &str to String.

---

_Label `C: docs` added by @Ebedthan on 2021-07-27 22:59_

---

_Label `D: easy` added by @pksunkara on 2021-07-28 09:24_

---

_Label `Z: good first issue` added by @pksunkara on 2021-07-28 09:24_

---

_Referenced in [clap-rs/clap#2636](../../clap-rs/clap/pulls/2636.md) on 2021-07-28 15:35_

---

_Closed by @pksunkara on 2021-07-28 20:40_

---

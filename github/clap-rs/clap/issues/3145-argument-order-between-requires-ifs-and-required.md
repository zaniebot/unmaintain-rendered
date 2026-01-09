---
number: 3145
title: Argument order between requires_ifs and required_ifs is different
type: issue
state: closed
author: epage
labels:
  - C-bug
  - M-breaking-change
  - A-builder
  - S-triage
assignees: []
created_at: 2021-12-10T20:25:26Z
updated_at: 2021-12-10T20:27:40Z
url: https://github.com/clap-rs/clap/issues/3145
synced_at: 2026-01-07T13:12:19-06:00
---

# Argument order between requires_ifs and required_ifs is different

---

_Issue opened by @epage on 2021-12-10 20:25_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.55.0 (c8dfcfe04 2021-09-06)

### Clap Version

3.0.0-rc.0

### Minimal reproducible code

See code sample in [this comment](https://github.com/clap-rs/clap/issues/3059#issuecomment-989249999).  Looking at the docs for both clap2 and clap3, it appears the val and the arg are in different orders and, in this case, the user assumed they were in the same order.

### Steps to reproduce the bug with the above code

Run the tests

### Actual Behaviour

It works

### Expected Behaviour

Clap3 asserts about the arg "excel" not existing

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @epage on 2021-12-10 20:25_

---

_Label `M-breaking-change` added by @epage on 2021-12-10 20:25_

---

_Label `A-builder` added by @epage on 2021-12-10 20:25_

---

_Label `S-triage` added by @epage on 2021-12-10 20:25_

---

_Comment by @epage on 2021-12-10 20:27_

nm, the `value` is associated with "the defining argument" which is why it comes first.

---

_Closed by @epage on 2021-12-10 20:27_

---

---
number: 3095
title: Unclear what feature flags are needed for API items
type: issue
state: closed
author: epage
labels:
  - C-bug
  - A-docs
assignees: []
created_at: 2021-12-08T21:37:02Z
updated_at: 2021-12-09T03:14:15Z
url: https://github.com/clap-rs/clap/issues/3095
synced_at: 2026-01-10T01:27:32Z
---

# Unclear what feature flags are needed for API items

---

_Issue opened by @epage on 2021-12-08 21:37_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.55.0 (c8dfcfe04 2021-09-06)

### Clap Version

v3.0.0-rc.0

### Minimal reproducible code

N/A

### Steps to reproduce the bug with the above code

See https://docs.rs/clap/3.0.0-rc.0/clap/macro.crate_authors.html

### Actual Behaviour

Unclear you can't use it

### Expected Behaviour

Tells about the feature flag

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @epage on 2021-12-08 21:37_

---

_Label `A-docs` added by @epage on 2021-12-08 21:37_

---

_Comment by @epage on 2021-12-08 21:37_

Looks like there is a `doc_auto_cfg` attribute we can use, see https://github.com/rust-lang/rust/pull/90502

---

_Comment by @pksunkara on 2021-12-09 01:10_

AFAIK, it was supposed to be automatically enabled. Not sure why they wanted a separate attribute to enable it.

---

_Referenced in [clap-rs/clap#3101](../../clap-rs/clap/pulls/3101.md) on 2021-12-09 02:35_

---

_Closed by @epage on 2021-12-09 03:14_

---

---
number: 5457
title: "clap_complete: `generate()` shouldn't panic if writing fails"
type: issue
state: open
author: yedayak
labels:
  - C-bug
  - M-breaking-change
assignees: []
created_at: 2024-04-15T19:00:07Z
updated_at: 2024-04-15T19:14:14Z
url: https://github.com/clap-rs/clap/issues/5457
synced_at: 2026-01-07T13:12:20-06:00
---

# clap_complete: `generate()` shouldn't panic if writing fails

---

_Issue opened by @yedayak on 2024-04-15 19:00_

### Please complete the following tasks

* [x]  I have searched the [discussions](https://github.com/clap-rs/clap/discussions)

* [x]  I have searched the existing issues


### Rust Version

rustc 1.77.2 (25ef9e3d8 2024-04-09)
### Clap Version
master


### Actual Behaviour

Running generate() can panic, for example if writing to a pipe ans the next program stops, or anything really stops the generation from completing. 
For example 
```
cmd --generate | head
```
with a long enough completion will panic.

### Expected Behaviour
generate() should return a Result indicating if it fails. This will be a breaking change.

### Additional Context
Maybe we ignore some errors? I'm not sure what consumers will want to do with the error, and some of them might be not important enough to propagate, for example a broken pipe.
The panic happens here: https://github.com/clap-rs/clap/blob/0cd10d19f0ac39341f89130d924e87879b58867f/clap_complete/src/macros.rs#L5

Opened after suggestion in #5453.


---

_Referenced in [clap-rs/clap#5453](../../clap-rs/clap/pulls/5453.md) on 2024-04-15 19:01_

---

_Label `A-completion` added by @epage on 2024-04-15 19:14_

---

_Label `M-breaking-change` added by @epage on 2024-04-15 19:14_

---

_Label `A-completion` removed by @epage on 2024-04-15 19:14_

---

_Label `C-bug` added by @epage on 2024-04-15 19:14_

---

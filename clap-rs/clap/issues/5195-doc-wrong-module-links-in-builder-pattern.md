```yaml
number: 5195
title: "doc: wrong module links in builder pattern documentation"
type: issue
state: closed
author: pustekuchen91
labels:
  - C-bug
assignees: []
created_at: 2023-11-06T14:17:27Z
updated_at: 2023-11-28T14:14:46Z
url: https://github.com/clap-rs/clap/issues/5195
synced_at: 2026-01-12T16:14:16Z
```

# doc: wrong module links in builder pattern documentation

---

_@pustekuchen91_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

-

### Clap Version

latest

### Minimal reproducible code

https://docs.rs/clap/latest/clap/_tutorial/chapter_0/index.html

### Steps to reproduce the bug with the above code

* https://docs.rs/clap/latest/clap/_tutorial/chapter_0/index.html
* Follow Module links for Chapter 1
* https://docs.rs/clap/latest/clap/_tutorial/chapter_0/chapter_1/index.html will be openend instead of 
  https://docs.rs/clap/latest/clap/_tutorial/chapter_1/index.html

### Actual Behaviour

wrong url is openend - 
The requested resource does not exist


### Expected Behaviour

open correct url

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @pustekuchen91 on 2023-11-06 14:17_

---

_Comment by @epage on 2023-11-06 14:41_

Upstream issue: rust-lang/rust#117290

---

_Closed by @pustekuchen91 on 2023-11-28 14:14_

---

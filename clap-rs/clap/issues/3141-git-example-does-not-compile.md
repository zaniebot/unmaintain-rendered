```yaml
number: 3141
title: Git example does not compile.
type: issue
state: closed
author: ghost
labels:
  - C-bug
assignees: []
created_at: 2021-12-10T06:21:30Z
updated_at: 2021-12-10T16:18:34Z
url: https://github.com/clap-rs/clap/issues/3141
synced_at: 2026-01-12T16:14:14Z
```

# Git example does not compile.

---

_@ghost_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.59.0-nightly (0b42deacc 2021-12-09)

### Clap Version

2.34.0

### Minimal reproducible code

https://github.com/clap-rs/clap/blob/master/examples/git.rs

### Steps to reproduce the bug with the above code

cargo run

### Actual Behaviour

![image](https://user-images.githubusercontent.com/93856041/145527021-a139acda-5362-4c33-a55d-44f4494e4c97.png)


### Expected Behaviour

It should've compiled.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @ghost on 2021-12-10 06:21_

---

_Comment by @epage on 2021-12-10 16:18_

That example is from `master` and is written for clap 3.0.0-rc.3.  What you want is to browse to the examples for v2, like [this link](https://github.com/clap-rs/clap/blob/33bebeda52b52c6f643b4ed6fa880671ba0ab80a/examples/20_subcommands.rs).

We are working to improve this by
- [Embedding examples directly in docs.rs](https://docs.rs/clap/3.0.0-rc.3/clap/struct.App.html#method.new)
- Having [doc.rs link to the relevant example version](https://docs.rs/clap/3.0.0-rc.3/clap/index.html#clap)

---

_Closed by @epage on 2021-12-10 16:18_

---

```yaml
number: 2982
title: "Examples in master branch don't build with 2.33.3"
type: issue
state: closed
author: jcos1
labels:
  - A-docs
assignees: []
created_at: 2021-11-02T00:51:33Z
updated_at: 2021-11-02T01:04:55Z
url: https://github.com/clap-rs/clap/issues/2982
synced_at: 2026-01-12T16:14:14Z
```

# Examples in master branch don't build with 2.33.3

---

_@jcos1_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

2.33.3

### Where?

https://github.com/clap-rs/clap/blob/master/examples/01b_quick_example.rs

### What's wrong?

The examples don't bulid.




### How to fix?

- `Arg::new("config")`  -> `Arg::with_name("config")`
- `.short('c')` -> `.short("c")`
- `.about("configuration file")` -> `.help("configuration file")`

---

_Label `C: docs` added by @jcos1 on 2021-11-02 00:51_

---

_Comment by @epage on 2021-11-02 01:04_

We are preparing a breaking release, 3.0, so it is expected the the `master` examples will be expecting that, rather than be for 2.33.3.  You'll be wanting to look at the examples in the v2-master branch, see https://github.com/clap-rs/clap/tree/v2-master/examples

Or even better, the examples tagged for you release: https://github.com/clap-rs/clap/tree/v2.33.3/examples

---

_Closed by @epage on 2021-11-02 01:04_

---

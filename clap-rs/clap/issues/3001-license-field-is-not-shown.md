---
number: 3001
title: license field is not shown
type: issue
state: closed
author: TheAlgorythm
labels:
  - C-bug
assignees: []
created_at: 2021-11-08T11:02:29Z
updated_at: 2021-11-12T17:24:49Z
url: https://github.com/clap-rs/clap/issues/3001
synced_at: 2026-01-10T01:27:30Z
---

# license field is not shown

---

_Issue opened by @TheAlgorythm on 2021-11-08 11:02_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

1.56

### Clap Version

master

### Minimal reproducible code

```rust
.license("MIT")
```


### Steps to reproduce the bug with the above code

```sh
$ cargo run --example 01a_quick_example -- --help
$ cargo run --example 01a_quick_example -- --version
```
I used [01a_quick_example.rs](https://github.com/clap-rs/clap/blob/master/examples/01a_quick_example.rs) as it contains the license statement.

### Actual Behaviour

The license information is not shown in the help message. Further, I tried `--version`, in case it would be there, but it wasn't.

### Expected Behaviour

The license information should be shown in the help message.

### Additional Context

Imo the functionality to render the license information was forgotten to be implemented.
Further, I will create a PR to be able to add the license with the derive macro if nobody has something against it.

### Debug Output

_No response_

---

_Label `T: bug` added by @TheAlgorythm on 2021-11-08 11:02_

---

_Comment by @epage on 2021-11-08 13:42_

Looks like `App::license` was added in https://github.com/clap-rs/clap/pull/2144 but nothing used it in that PR but it was only implemented to help prepare for https://github.com/clap-rs/clap/issues/1768.

I'm wondering if we should remove this or put it behind an `unstable-license` feature flag until we have #1768

---

_Added to milestone `3.0` by @epage on 2021-11-08 13:42_

---

_Comment by @TheAlgorythm on 2021-11-08 14:32_

If somebody guides me to the file, where the rendering happens, and says how #1768 should look like, I should be able to implement it.

---

_Comment by @epage on 2021-11-08 14:46_

> says how #1768 should look like

I think thats the important part we need to first discuss on #1768

---

_Referenced in [clap-rs/clap#3016](../../clap-rs/clap/pulls/3016.md) on 2021-11-12 16:06_

---

_Closed by @bors[bot] on 2021-11-12 17:24_

---

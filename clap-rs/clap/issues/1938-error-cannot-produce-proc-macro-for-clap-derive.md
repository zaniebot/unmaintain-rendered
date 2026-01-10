---
number: 1938
title: "error: cannot produce proc-macro for `clap_derive v3.0.0-beta.1` as the target `x86_64-unknown-linux-musl` does not support these crate types"
type: issue
state: closed
author: mrusme
labels:
  - C-bug
assignees: []
created_at: 2020-05-21T15:51:34Z
updated_at: 2023-06-26T06:43:49Z
url: https://github.com/clap-rs/clap/issues/1938
synced_at: 2026-01-10T01:27:09Z
---

# error: cannot produce proc-macro for `clap_derive v3.0.0-beta.1` as the target `x86_64-unknown-linux-musl` does not support these crate types

---

_Issue opened by @mrusme on 2020-05-21 15:51_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closes issues

### Code

[Here](https://github.com/mrusme/cmdr).

### Steps to reproduce the issue

[Here](https://github.com/mrusme/cmdr/blob/master/Dockerfile).

### Version

Environment: `1.43.1-alpine3.11`
Clap: `3.0.0-beta.1`

### Actual Behavior Summary

`error: cannot produce proc-macro for 'clap_derive v3.0.0-beta.1' as the target 'x86_64-unknown-linux-musl' does not support these crate types`

### Expected Behavior Summary

Build should be successful.

### Additional context

Alpine Linux, official Rust image.

### Debug output

[Here](https://github.com/mrusme/cmdr/runs/696809291?check_suite_focus=true#step:3:55).

---

_Label `T: bug` added by @mrusme on 2020-05-21 15:51_

---

_Assigned to @CreepySkeleton by @pksunkara on 2020-05-21 16:16_

---

_Comment by @pksunkara on 2020-05-21 16:17_

https://github.com/rust-lang/rust/issues/40174#issuecomment-538791091

---

_Comment by @CreepySkeleton on 2020-05-21 16:18_

@mrusme As a general rule, please avoid linking to someone's `github.com/<user>/<repo>/master` because master (and other tags/branches) are not permalinks. You push a new commit, and the link now points to another piece of code.

Ideally, you should copy-paste the relevant code into the issue. If you think that the code sample is too large, you can use permalinks:

![image](https://user-images.githubusercontent.com/50968528/82579527-9d056e80-9b96-11ea-8eeb-4dc270731782.png)

---

As regards to the issue, this is [a bug in rustc](https://github.com/rust-lang/cargo/issues/7563). The fix is already merged and will be released in 1.44.0.

The temporary workaround is to use this
```
RUSTFLAGS="-C target-feature=-crt-static" cargo build
```

instead of `cargo build`.

Or if you don't need the derive (and it looks like you don't), you can just switch it off with:
```
# Cargo.toml
[dependencies]
clap = { git = "...", default-features = false, features = ["std"] }
```




---

_Closed by @CreepySkeleton on 2020-05-21 16:18_

---

_Comment by @mrusme on 2020-05-21 16:21_

@CreepySkeleton thank you, Sir!

---

_Comment by @CreepySkeleton on 2020-05-21 16:24_

I'm a Sir, yay!

---

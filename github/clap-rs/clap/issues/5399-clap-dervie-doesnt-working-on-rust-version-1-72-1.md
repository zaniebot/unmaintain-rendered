---
number: 5399
title: clap_dervie doesnt working on rust version 1.72.1
type: issue
state: closed
author: satyakommula96
labels:
  - C-bug
assignees: []
created_at: 2024-03-18T17:09:11Z
updated_at: 2024-03-22T11:39:59Z
url: https://github.com/clap-rs/clap/issues/5399
synced_at: 2026-01-07T13:12:20-06:00
---

# clap_dervie doesnt working on rust version 1.72.1

---

_Issue opened by @satyakommula96 on 2024-03-18 17:09_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.71.1

### Clap Version

4.5.9

### Minimal reproducible code

```rust
fn main() {}
```


### Steps to reproduce the bug with the above code

```
cargo msrv --linear
Fetching index
Determining the Minimum Supported Rust Version (MSRV) for toolchain x86_64-unknown-linux-gnu
Using check command cargo check
Check for toolchain '1.76.0-x86_64-unknown-linux-gnu' succeeded
Check for toolchain '1.75.0-x86_64-unknown-linux-gnu' succeeded
Check for toolchain '1.74.1-x86_64-unknown-linux-gnu' succeeded

Check for toolchain '1.73.0-x86_64-unknown-linux-gnu' failed with:
┌────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ error: package `clap_derive v4.5.3` cannot be built because it requires rustc 1.74 or newer, while the currently active rustc version  │
│ is 1.73.0                                                                                                                              │
│ Either upgrade to rustc 1.74 or newer, or use                                                                                          │
│ cargo update -p clap_derive@4.5.3 --precise ver                                                                                        │
│ where `ver` is the latest version of `clap_derive` supporting rustc 1.73.0                                                             │
└────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
   Finished The MSRV is: 1.74.1   ███████████████████████████████████████████████████████████████████████████████████████████████ 00:03:23
   ```

### Actual Behaviour

should work on rust version 1.72.1 also without breaking anythings

### Expected Behaviour

should work on rust version1.72.1 also

### Additional Context

_No response_

### Debug Output

clap = { version = "4", features = ["derive", "cargo"] }


---

_Label `C-bug` added by @satyakommula96 on 2024-03-18 17:09_

---

_Comment by @epage on 2024-03-18 17:15_

This states it should work on 1.72 but doesn't say why.

---

_Comment by @satyakommula96 on 2024-03-18 17:25_

@epage is there way to get full verbose output?


---

_Comment by @epage on 2024-03-18 17:31_

Verbose output of what?

---

_Comment by @epage on 2024-03-22 11:39_

Without further information, I'm closing this.  Our MSRV policy is N-2 so it is within that policy to support 1.74

---

_Closed by @epage on 2024-03-22 11:39_

---

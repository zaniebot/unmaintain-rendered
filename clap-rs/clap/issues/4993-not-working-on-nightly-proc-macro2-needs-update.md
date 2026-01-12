```yaml
number: 4993
title: "Not working on nightly - proc-macro2 needs update | renovate not working?"
type: issue
state: closed
author: udondan
labels:
  - C-bug
assignees: []
created_at: 2023-07-05T10:08:20Z
updated_at: 2023-07-07T19:08:35Z
url: https://github.com/clap-rs/clap/issues/4993
synced_at: 2026-01-12T16:14:16Z
```

# Not working on nightly - proc-macro2 needs update | renovate not working?

---

_@udondan_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.72.0-nightly

### Clap Version

4.3.10

### Minimal reproducible code

None. Installation fails

### Steps to reproduce the bug with the above code

rustup default nightly
cargo add clap
cargo build

### Actual Behaviour

Failing due to incomaptible outdated dependency.

```
error[E0635]: unknown feature `proc_macro_span_shrink`
  --> /Users/user/.cargo/registry/src/index.crates.io-6f17d22bba15001f/proc-macro2-1.0.53/src/lib.rs:92:30
   |
92 |     feature(proc_macro_span, proc_macro_span_shrink)
   |                              ^^^^^^^^^^^^^^^^^^^^^^
```

### Expected Behaviour

Working

### Additional Context

proc-macro2 is a dependency of clap_derive:

```
cargo tree
...
│   ├── clap_derive v4.3.2 (proc-macro)
│   │   ├── heck v0.4.1
│   │   ├── proc-macro2 v1.0.53 (*)
...
```

The problem has been fixed in 1.0.60+ https://github.com/dtolnay/proc-macro2/issues/398

The last [dependabot PR for proc-macro2](https://github.com/clap-rs/clap/pull/4321) had been closed without comment. I see you switched to renovate now but I wonder if it's working correctly, since I don't see any MR for that package.

### Debug Output

_No response_

---

_Label `C-bug` added by @udondan on 2023-07-05 10:08_

---

_Comment by @epage on 2023-07-05 15:15_

As that version of `proc-macro2` works fine on stable, we have no reason to require a higher minimum version of it.

If you are needing a newer proc-macro2 within your crate, you can update it via `cargo update -p proc-macro2`

---

_Comment by @epage on 2023-07-07 19:08_

As this isn't a bug with clap and without further input, I'm going to go ahead and close this.  Let us know if we need to re-evaluate this!

---

_Closed by @epage on 2023-07-07 19:08_

---

```yaml
number: 6007
title: "`env` method not found in `Arg`"
type: issue
state: closed
author: pronebird
labels:
  - C-bug
assignees: []
created_at: 2025-05-19T07:52:38Z
updated_at: 2025-05-20T18:49:01Z
url: https://github.com/clap-rs/clap/issues/6007
synced_at: 2026-01-12T16:14:17Z
```

# `env` method not found in `Arg`

---

_@pronebird_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.88.0-nightly (a15cce269 2025-04-17)

### Clap Version

4

### Minimal reproducible code

```rust
use clap::Args;

#[derive(Debug, Clone, Args)]
pub struct Command {
    #[arg(long, default_value_t, env = "PLAY_BALL", action = clap::ArgAction::SetTrue)]
    pub play_ball: bool,
}
```


### Steps to reproduce the bug with the above code

Compile it

### Actual Behaviour

Fails to compile with ``env` method not found in `Arg``

However `env()` is documented to exist on `Arg` as per https://docs.rs/clap/latest/clap/struct.Arg.html#method.env

### Expected Behaviour

That the code compiles

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @pronebird on 2025-05-19 07:52_

---

_Comment by @pi55man on 2025-05-19 20:55_

the env method is behind a feature flag, you have to include the "env" feature in your cargo.toml

`clap = {version = "4", features = ["derive", "env" ]}`

---

_Comment by @pronebird on 2025-05-19 20:57_

@pi55man thanks. 🙏 Perhaps we need to document it because it's not very obvious from documentation?

---

_Comment by @epage on 2025-05-19 21:03_

The compiler should have given a message about `features = "env"`.  Do you have the full compiler output?

Also, the docs should also mention it but they aren't and I'm not seeing why.  I'll have to dig into that.

---

_Comment by @pi55man on 2025-05-19 21:13_

here's the compiler output:

> error[E0599]: no method named `env` found for struct `Arg` in the current scope
>  --> src/main.rs:5:33
>    |
> 5 |     #[arg(long, default_value_t,env= "PLAY_BALL", action = clap::ArgAction::SetTrue)]
>    |                                 ^^^ method not found in `Arg`
> 
> For more information about this error, try `rustc --explain E0599`.
> error: could not compile `foo` (bin "foo") due to 2 previous errors

---

_Comment by @epage on 2025-05-20 17:31_

For the docs:
- [4.5.34](https://docs.rs/clap/4.5.34/clap/struct.Arg.html#method.env) (2025-03-27): worked
- [4.5.35](https://docs.rs/clap/4.5.35/clap/struct.Arg.html#method.env) (2025-04-01): didn't worked

Not seeing anything relevant from the [diff](https://diff.rs/clap/4.5.34/4.5.35/Cargo.toml)

Looking at [clap_builder](https://docs.rs/clap_builder/4.5.38/clap_builder/builder/struct.Arg.html#method.env), it still works.  This seems to be a bug with re-exports.

---

_Comment by @epage on 2025-05-20 17:39_

For the docs, I opened rust-lang/rust#141301

---

_Comment by @epage on 2025-05-20 18:46_

As for the compiler error, apparently its only for top-level items and not functions in `impl` blocks? 

```console
❯ cargo check
    Checking dep v0.1.0 (/home/epage/src/personal/dump/clap-6007/dep)
    Checking clap-6007 v0.1.0 (/home/epage/src/personal/dump/clap-6007)
error[E0425]: cannot find function `foo_feature` in crate `dep`
 --> src/main.rs:6:10
  |
6 |     dep::foo_feature();
  |          ^^^^^^^^^^^ help: a function with a similar name exists: `no_feature`
  |
 ::: /home/epage/src/personal/dump/clap-6007/dep/src/lib.rs:4:1
  |
4 | pub fn no_feature() {}
  | ------------------- similarly named function `no_feature` defined here
  |
note: found an item that was configured out
 --> /home/epage/src/personal/dump/clap-6007/dep/src/lib.rs:2:8
  |
2 | pub fn foo_feature() {}
  |        ^^^^^^^^^^^
note: the item is gated behind the `foo` feature
 --> /home/epage/src/personal/dump/clap-6007/dep/src/lib.rs:1:7
  |
1 | #[cfg(feature = "foo")]
  |       ^^^^^^^^^^^^^^^

warning: unused import: `clap_builder as clap`
 --> src/main.rs:1:5
  |
1 | use clap_builder as clap;
  |     ^^^^^^^^^^^^^^^^^^^^
  |
  = note: `#[warn(unused_imports)]` on by default

error[E0599]: no function or associated item named `foo_feature` found for struct `dep::Arg` in the current scope
 --> src/main.rs:8:15
  |
8 |     dep::Arg::foo_feature();
  |               ^^^^^^^^^^^ function or associated item not found in `dep::Arg`
  |
note: if you're trying to build a new `dep::Arg`, consider using `dep::Arg::new` which returns `dep::Arg`
 --> /home/epage/src/personal/dump/clap-6007/dep/src/lib.rs:9:5
  |
9 |     pub fn new() -> Self {
  |     ^^^^^^^^^^^^^^^^^^^^
help: there is an associated function `no_feature` with a similar name
  |
8 -     dep::Arg::foo_feature();
8 +     dep::Arg::no_feature();
  |

Some errors have detailed explanations: E0425, E0599.
For more information about an error, try `rustc --explain E0425`.
warning: `clap-6007` (bin "clap-6007") generated 1 warning
error: could not compile `clap-6007` (bin "clap-6007") due to 2 previous errors; 1 warning emitted
```

---

_Comment by @epage on 2025-05-20 18:49_

Looks like the compiler diagnostics are being tracked in rust-lang/rust#130319.

Looks like there is nothing more we can do so I'm going to close this.

---

_Closed by @epage on 2025-05-20 18:49_

---

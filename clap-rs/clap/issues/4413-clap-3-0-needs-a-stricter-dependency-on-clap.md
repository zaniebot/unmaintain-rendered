---
number: 4413
title: clap 3.0 needs a stricter dependency on clap_derive
type: issue
state: closed
author: dimo414
labels:
  - C-bug
assignees: []
created_at: 2022-10-21T07:45:03Z
updated_at: 2022-10-23T04:29:33Z
url: https://github.com/clap-rs/clap/issues/4413
synced_at: 2026-01-10T01:27:54Z
---

# clap 3.0 needs a stricter dependency on clap_derive

---

_Issue opened by @dimo414 on 2022-10-21 07:45_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.63.0 (4b91a6ea7 2022-08-08)

### Clap Version

3.0.14

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Parser)]
struct Cli {
    #[clap(long)]
    foo: bool,
}

fn main() {
    let cli = Cli::parse();
    println!("foo:{:?}", cli.foo);
}
```


### Steps to reproduce the bug with the above code

The above compiles and runs with the following dependencies:

```
[dependencies]
clap = { version = "~3.0", features = ["derive"] }
clap_derive = "~3.0"
```

But removing the `clap_derive` dependency and clearing `Cargo.lock` causes the build to fail:

### Actual Behaviour

```
$ cargo build
    Updating crates.io index
   Compiling clap_derive v3.2.18
   Compiling clap v3.0.14
   Compiling clap-demo v0.1.0
error[E0407]: method `from_arg_matches_mut` is not a member of trait `clap::FromArgMatches`
 --> src/main.rs:3:10
  |
3 | #[derive(Parser)]
  |          ^^^^^^
  |          |
  |          not a member of trait `clap::FromArgMatches`
  |          help: there is an associated function with a similar name: `from_arg_matches`
  |
  = note: this error originates in the derive macro `Parser` (in Nightly builds, run with -Z macro-backtrace for more info)

error[E0407]: method `update_from_arg_matches_mut` is not a member of trait `clap::FromArgMatches`
 --> src/main.rs:3:10
  |
3 | #[derive(Parser)]
  |          ^^^^^^
  |          |
  |          not a member of trait `clap::FromArgMatches`
  |          help: there is an associated function with a similar name: `update_from_arg_matches`
  |
  = note: this error originates in the derive macro `Parser` (in Nightly builds, run with -Z macro-backtrace for more info)

error[E0405]: cannot find trait `CommandFactory` in crate `clap`
 --> src/main.rs:3:10
  |
3 | #[derive(Parser)]
  |          ^^^^^^ not found in `clap`
  |
  = note: this error originates in the derive macro `Parser` (in Nightly builds, run with -Z macro-backtrace for more info)

error[E0412]: cannot find type `Command` in crate `clap`
 --> src/main.rs:3:10
  |
3 | #[derive(Parser)]
  |          ^^^^^^ not found in `clap`
  |
  = note: this error originates in the derive macro `Parser` (in Nightly builds, run with -Z macro-backtrace for more info)
help: consider importing this struct
  |
1 | use std::process::Command;
  |
help: if you import `Command`, refer to it directly
  |
3 - #[derive(Parser)]
3 + #[derive(Parser)]
  |

error[E0433]: failed to resolve: could not find `Command` in `clap`
 --> src/main.rs:3:10
  |
3 | #[derive(Parser)]
  |          ^^^^^^ not found in `clap`
  |
  = note: this error originates in the derive macro `Parser` (in Nightly builds, run with -Z macro-backtrace for more info)
help: consider importing this struct
  |
1 | use std::process::Command;
  |
help: if you import `Command`, refer to it directly
  |
3 - #[derive(Parser)]
3 + #[derive(Parser)]
  |

Some errors have detailed explanations: E0405, E0407, E0412, E0433.
For more information about an error, try `rustc --explain E0405`.
error: could not compile `clap-demo` due to 5 previous errors
```

### Expected Behaviour

I expected to be able to specify a [tilde requirement](https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html#tilde-requirements) on 3.0 and be able to use clap_derive, but because [clap's `Cargo.toml`](https://github.com/clap-rs/clap/blob/dc035de4094e2f336fe90bd6d3332c9711137766/Cargo.toml#L121) doesn't restrict the upgrade path for its `clap_derive` dep it pulls down newer, incompatible versions of `clap_derive`.

### Additional Context

I'm planning to [upgrade to 3.2 eventually](https://github.com/EmbarkStudios/cargo-deny/pull/431), but for the time being I'm trying to keep a few different projects using the same syntax+version for ease of maintenance. An older project happened to migrate to 3.0 when clap_derive was on 3.1.4 and that worked without issue, but setting up a new project with clap:3.0 failed unexpectedly and confusingly, until I realized a different version of clap_derive was being linked in.

It looks like as of [3.1.9](https://github.com/clap-rs/clap/blob/7598c000f9da5698fc00985dea1b8ba17e9572c5/Cargo.toml#L121) the dependency on clap_derive was constrained to a nearby version (ed57342b), but the 3.0 branch is still broken. It'd be great if a 3.0.15 patch could be cut that fixes this dep.

### Debug Output

LMK if you need this.

---

_Label `C-bug` added by @dimo414 on 2022-10-21 07:45_

---

_Comment by @epage on 2022-10-21 11:48_

Especially as this is an old release and there is a workaround, I don't see sufficient justification for branching and updating 3.0.x when we've already moved on to 3.1.x, 3.2.x, and 4.0.x.  As-is, 3.2.x is in maintenance mode.

---

_Closed by @epage on 2022-10-21 11:48_

---

_Comment by @dimo414 on 2022-10-23 04:29_

I feel it's still worth cutting a new release so that it continues to compile, as this is a regression from the behavior when the release was published. Although the workaround is straightforward it's totally opaque at the error-site - you have to recognize the compilation errors indicate a bug in a longstanding release of Clap, which isn't intuitive or expected.

---

---
number: 3592
title: Emit compile-time error if the derive interface produces duplicate short/long arguments
type: issue
state: closed
author: nsunderland1
labels:
  - C-enhancement
assignees: []
created_at: 2022-03-30T18:12:09Z
updated_at: 2022-03-30T18:17:37Z
url: https://github.com/clap-rs/clap/issues/3592
synced_at: 2026-01-07T13:12:19-06:00
---

# Emit compile-time error if the derive interface produces duplicate short/long arguments

---

_Issue opened by @nsunderland1 on 2022-03-30 18:12_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.1.3

### Describe your use case

Not much special context for the use case, reasoning behind the feature is given below.

### Describe the solution you'd like

Given the following code ([playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=19e608001b198271c876aaeb1ea04aaa)):

```rust
use clap::Parser;

#[derive(Parser)]
struct Args {
    #[clap(short)]
    arg1: String,

    #[clap(short)]
    arg2: String,
}

fn main() {
    let _ = Args::parse();
}
```

clap will successfully detect at runtime that `arg1` and `arg2` have the duplicate short form `-a`:

```
thread 'main' panicked at 'Command playground: Short option names must be unique for each argument, but '-a' is in use by both 'arg1' and 'arg2'', /playground/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.1.3/src/build/debug_asserts.rs:100:17
note: [run with `RUST_BACKTRACE=1` environment variable to display a backtrace](https://play.rust-lang.org/#)
```

With clap's derive interface, it seems like this could be caught at compile time when the proc macro executes on `Args`, by producing a compiler error if it picks up on duplicate short (or long) names.

This would give much quicker feedback for programmer error. Also, since clap only checks for argument uniqueness in debug mode, if you're only ever running a release build of your binary you won't actually hit this panic.



### Alternatives, if applicable

I personally wouldn't mind it if the assertions were enabled in release builds, but the compile-time approach seems better as it avoids the runtime overhead.

### Additional Context

We were mostly only running our binary in release mode, and we went a few days with duplicate short arguments. When we hit the panic, it wasn't immediately obvious what was going on.

---

_Label `C-enhancement` added by @nsunderland1 on 2022-03-30 18:12_

---

_Comment by @epage on 2022-03-30 18:16_

FYI this has previously been reported as https://github.com/clap-rs/clap/issues/3133

---

_Closed by @epage on 2022-03-30 18:16_

---

_Comment by @nsunderland1 on 2022-03-30 18:17_

Ah thanks, went searching with "unique" as my query, shoulda tried flipping it :)

---

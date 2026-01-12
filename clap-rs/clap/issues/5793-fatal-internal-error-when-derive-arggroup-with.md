```yaml
number: 5793
title: "Fatal Internal Error when derive ArgGroup with flag that doesn't exist in separate crate"
type: issue
state: closed
author: JulianKnodt
labels:
  - C-bug
assignees: []
created_at: 2024-10-29T06:45:28Z
updated_at: 2024-10-29T18:11:53Z
url: https://github.com/clap-rs/clap/issues/5793
synced_at: 2026-01-12T16:14:17Z
```

# Fatal Internal Error when derive ArgGroup with flag that doesn't exist in separate crate

---

_@JulianKnodt_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.83.0-nightly (52fd99839 2024-10-10)

### Clap Version

4.5.7 w/ Derive

### Minimal reproducible code

This example requires two separate files. I believe the `bin` file is counted as a separate crate.

src/lib.rs:
```
use clap::Parser;
#[derive(Parser)]
#[clap(group(
    clap::ArgGroup::new("example")
        .required(true)
        .args(&["noexists"])
))]
pub struct Args {
}
```

src/bin/main.rs:
```
use clap::Parser;

pub fn main() {
  let _args = clap_repro::Args::parse();
}

```

Maybe this is because there's some difference between structs defined in the same crate vs

### Steps to reproduce the bug with the above code

`cargo run --release` with no commands.

### Actual Behaviour

<img width="722" alt="image" src="https://github.com/user-attachments/assets/ae4d2ee6-e57b-44f8-8a87-1e4b753c5b99">

If I pass an arg that exists in the ArgGroup it works silently.

### Expected Behaviour

It should error that the arg doesn't exist and exit, regardless of other flags.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @JulianKnodt on 2024-10-29 06:45_

---

_Comment by @epage on 2024-10-29 14:24_

For me, I see this whether you have everything in the bin or split between bin and lib.

When you run without `--release`, the root cause is exposed:
```
thread 'main' panicked at /home/epage/.cargo/registry/src/index.crates.io-6f17d22bba15001f/clap_builder-4.5.20/src/bui
lder/debug_asserts.rs:292:13:
Command clap_repro: Argument group 'example' contains non-existent argument 'noexists'
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

As this assert is caused by a development time bug that we report in dev builds, I'm inclined to close this.  If there is a reason for us to keep this open, let us know!

---

_Closed by @epage on 2024-10-29 14:24_

---

_Comment by @JulianKnodt on 2024-10-29 17:27_

Hm, is it possible to indicate that the problem can be seen during a dev build? I only build my actual use case in release, so I was quite confused when I saw the fatal error.

If that's not something y'all would like to change that's fine 

---

_Comment by @epage on 2024-10-29 18:11_

We only build the more specialized assertions in dev builds that hit before these internal ones.  This is for build times, binary size, and runtime performance.

---

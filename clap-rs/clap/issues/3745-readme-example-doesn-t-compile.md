---
number: 3745
title: "README example doesn't compile"
type: issue
state: closed
author: mlynch
labels:
  - C-bug
assignees: []
created_at: 2022-05-24T14:23:37Z
updated_at: 2022-06-05T18:08:28Z
url: https://github.com/clap-rs/clap/issues/3745
synced_at: 2026-01-10T01:27:45Z
---

# README example doesn't compile

---

_Issue opened by @mlynch on 2022-05-24 14:23_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.61.0 (fe5b13d68 2022-05-18)

### Clap Version

3.1.18

### Minimal reproducible code

```rust
use clap::Parser;

/// Simple program to greet a person
#[derive(Parser, Debug)]
#[clap(author, version, about, long_about = None)]
struct Args {
    /// Name of the person to greet
    #[clap(short, long, value_parser)]
    name: String,

    /// Number of times to greet
    #[clap(short, long, value_parser, default_value_t = 1)]
    count: u8,
}

fn main() {
    let args = Args::parse();

    for _ in 0..args.count {
        println!("Hello {}!", args.name)
    }
}
```


### Steps to reproduce the bug with the above code

cargo run

### Actual Behaviour

```rust
error: unexpected attribute: value_parser
 --> src/main.rs:8:25
  |
8 |     #[clap(short, long, value_parser)]
  |                         ^^^^^^^^^^^^
```

### Expected Behaviour

Example should compile

### Additional Context

Looking closer, it seems to be a mismatch with the example that seems to expect whatever is in `master`, and the version specified in Cargo. If I see the `README` corresponding to the 3.1.18 tag, the example works and does not include that `value_parser` arg.

### Debug Output

_No response_

---

_Label `C-bug` added by @mlynch on 2022-05-24 14:23_

---

_Comment by @epage on 2022-05-24 14:28_

The README reflects whats in `master`.

If you want to browse the README for a specific version, I recommend looking at the tag for it, like https://github.com/clap-rs/clap/tree/v3.1.18

---

_Closed by @epage on 2022-05-24 14:28_

---

_Comment by @mlynch on 2022-05-24 16:10_

Yea just note that the `README` currently says to add this to Cargo, which will result in the example not compiling: 

```toml
clap = { version = "3.1.18", features = ["derive"] }
```

---

_Comment by @epage on 2022-05-24 16:12_

That is the version in `master` since we don't bump until the next release and I'd rather not have this be a git dependency for all commits except for the release commit and then revert back again.

---

_Comment by @jackh-ncl on 2022-06-05 18:08_

Thanks @mlynch, I ran into this just now as a new user and coming across this issue helped me resolve the problem.

---

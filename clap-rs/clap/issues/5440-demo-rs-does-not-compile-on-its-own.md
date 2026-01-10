---
number: 5440
title: "`demo.rs` does not compile on its own"
type: issue
state: closed
author: DeflateAwning
labels:
  - C-bug
assignees: []
created_at: 2024-04-03T03:34:19Z
updated_at: 2024-04-03T21:52:51Z
url: https://github.com/clap-rs/clap/issues/5440
synced_at: 2026-01-10T01:28:11Z
---

# `demo.rs` does not compile on its own

---

_Issue opened by @DeflateAwning on 2024-04-03 03:34_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.77.0 (aedd173a2 2024-03-17)

### Clap Version

clap = "4.5.4"

### Minimal reproducible code

Copied directly from: https://github.com/clap-rs/clap/blob/9d14f394ba22f65f8957310a03ae5fd613f89d76/examples/demo.rs

```rust
use clap::Parser;

/// Simple program to greet a person
#[derive(Parser, Debug)]
#[command(version, about, long_about = None)]
struct Args {
    /// Name of the person to greet
    #[arg(short, long)]
    name: String,

    /// Number of times to greet
    #[arg(short, long, default_value_t = 1)]
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

Run the code.

### Actual Behaviour

It has a bunch of compile errors.

### Expected Behaviour

I'd expect a file named `demo.rs` to work in isolation. 

### Additional Context

Disclaimer: I don't really know what I'm doing. I'm sure someone who knew what they were doing would have more success than I am.

### Debug Output

_No response_

---

_Label `C-bug` added by @DeflateAwning on 2024-04-03 03:34_

---

_Comment by @epage on 2024-04-03 16:08_

Generally, the examples are written to be viewed from rustdoc.  For example, see https://docs.rs/clap/latest/clap/#example for the discussion on `demo.rs`

If you want to bypass that, to understand examples, you need to look at the context of the `Cargo.toml`.  See
https://github.com/clap-rs/clap/blob/9d14f394ba22f65f8957310a03ae5fd613f89d76/Cargo.toml#L117-L119

As this is working as expected and people do have a path forward, I'm going to go ahead and close this.  If there is a reason for us to reconsider, let us know!

---

_Closed by @epage on 2024-04-03 16:08_

---

_Comment by @DeflateAwning on 2024-04-03 17:03_

Could you please consider adding a README to that folder which directs people to the docs appropriately?

---

_Comment by @epage on 2024-04-03 17:24_

One exists: https://github.com/clap-rs/clap/blob/master/examples/README.md

---

_Comment by @DeflateAwning on 2024-04-03 21:52_

Ah I do remember reading that, and didn't get the impression that the examples weren't meant to function in isolation. 

Perhaps a message along the lines of the following would be beneficial:

> These examples are not meant to function in isolation, and are only suplemental material to the following tutorial(s):



---

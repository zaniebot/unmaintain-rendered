---
number: 3199
title: Error text for derived numeric ranges is unclear
type: issue
state: closed
author: epage
labels:
  - C-bug
  - A-derive
  - S-waiting-on-design
assignees: []
created_at: 2021-12-20T16:32:41Z
updated_at: 2022-05-25T20:01:08Z
url: https://github.com/clap-rs/clap/issues/3199
synced_at: 2026-01-07T13:12:19-06:00
---

# Error text for derived numeric ranges is unclear

---

_Issue opened by @epage on 2021-12-20 16:32_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.55.0 (c8dfcfe04 2021-09-06)

### Clap Version

master

### Minimal reproducible code

```rust
use clap::Parser;

/// Simple program to greet a person
#[derive(Parser, Debug)]
#[clap(about, version, author)]
struct Args {
    /// Name of the person to greet
    #[clap(short, long)]
    name: String,

    /// Number of times to greet
    #[clap(short, long, default_value_t = 1)]
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

In the clap repo
```bash
$ cargo run --example demo --features derive -- --name foo --count 20000000
```

### Actual Behaviour

```
error: Invalid value for '--count <COUNT>': number too large to fit in target type

For more information try --help
```

### Expected Behaviour

```
error: Invalid value for '--count <COUNT>': 20000000 too large, must be <255

For more information try --help
```

### Additional Context

From reddit post https://www.reddit.com/r/rust/comments/rhxkau/clap_300rc7/hp9drzd/

### Debug Output

_No response_

---

_Label `C-bug` added by @epage on 2021-12-20 16:32_

---

_Label `A-derive` added by @epage on 2021-12-20 16:32_

---

_Label `S-triage` added by @epage on 2021-12-20 16:32_

---

_Comment by @epage on 2021-12-20 16:35_

The challenge with fixing this is that the error is coming from `FromStr`.  We don't inspect the actual type information to be able to provide a more user-meaningful error.

A workaround is for the user to provide their own parse function.

---

_Comment by @epage on 2022-02-02 20:07_

From the core of clap's perspective, this is a wont-fix.  The user is supplying the type and relying on the default parser.

Two thoughts on where we can go with this
- A crate of types designed to handle cases like this (ideally using const-generics to allow restricting the range further)
- https://github.com/clap-rs/clap/issues/2683 might lead to us doing custom trait implementations and we could instead build this into clap.

---

_Label `S-triage` removed by @epage on 2022-02-02 20:08_

---

_Label `S-waiting-on-design` added by @epage on 2022-02-02 20:08_

---

_Referenced in [clap-rs/clap#3732](../../clap-rs/clap/pulls/3732.md) on 2022-05-17 21:47_

---

_Referenced in [clap-rs/clap#3734](../../clap-rs/clap/issues/3734.md) on 2022-05-18 00:23_

---

_Comment by @epage on 2022-05-25 20:01_

Huh, this was supposed to be closed by #3743 3734

---

_Closed by @epage on 2022-05-25 20:01_

---

_Referenced in [pacak/bpaf#61](../../pacak/bpaf/issues/61.md) on 2022-09-16 16:51_

---

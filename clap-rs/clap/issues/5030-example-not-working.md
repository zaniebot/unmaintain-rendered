---
number: 5030
title: Example Not Working 
type: issue
state: closed
author: JiveyGuy
labels:
  - C-bug
assignees: []
created_at: 2023-07-20T22:23:11Z
updated_at: 2023-07-21T00:20:57Z
url: https://github.com/clap-rs/clap/issues/5030
synced_at: 2026-01-10T01:28:05Z
---

# Example Not Working 

---

_Issue opened by @JiveyGuy on 2023-07-20 22:23_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.71.0 (8ede3aae2 2023-07-12)

### Clap Version

clap = "4.3.17"

### Minimal reproducible code

```rust
use clap::{Parser, command, arg};

/// Simple program to greet a person
#[derive(Parser, Debug)]
#[command(author, version, about, long_about = None)]
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

```bash
cargo add clap
cargo run
```

### Actual Behaviour

```bash
❯ cargo run
   Compiling bs4_generator_rs v0.1.0 (C:\Users\N39999\OneDrive - NGC\Documents\main_work\work\Parse_Bundle_Set_4\BS4_generator_rs)
error: cannot find derive macro `Parser` in this scope
 --> src\main.rs:4:10
  |
4 | #[derive(Parser, Debug)]
  |          ^^^^^^
  |
note: `Parser` is imported here, but it is only a trait, without a derive macro
 --> src\main.rs:1:12
  |
1 | use clap::{Parser, command, arg};
  |            ^^^^^^

error: cannot find attribute `command` in this scope
 --> src\main.rs:5:3
  |
5 | #[command(author, version, about, long_about = None)]
  |   ^^^^^^^
  |
note: `command` is imported here, but it is a function-like macro
 --> src\main.rs:1:20
  |
1 | use clap::{Parser, command, arg};
  |                    ^^^^^^^

error: cannot find attribute `arg` in this scope
 --> src\main.rs:8:7
  |
8 |     #[arg(short, long)]
  |       ^^^
  |
note: `arg` is imported here, but it is a function-like macro
 --> src\main.rs:1:29
  |
1 | use clap::{Parser, command, arg};
  |                             ^^^

error: cannot find attribute `arg` in this scope
  --> src\main.rs:12:7
   |
12 |     #[arg(short, long, default_value_t = 1)]
   |       ^^^
   |
note: `arg` is imported here, but it is a function-like macro
  --> src\main.rs:1:29
   |
1  | use clap::{Parser, command, arg};
   |                             ^^^

error[E0599]: no function or associated item named `parse` found for struct `Args` in the current scope
  --> src\main.rs:17:22
   |
6  | struct Args {
   | ----------- function or associated item `parse` not found for this struct
...
17 |     let args = Args::parse();
   |                      ^^^^^ function or associated item not found in `Args`
   |
   = help: items from traits can only be used if the trait is implemented and in scope
   = note: the following traits define an item `parse`, perhaps you need to implement one of them:  
           candidate #1: `Parser`
           candidate #2: `TypedValueParser`

For more information about this error, try `rustc --explain E0599`.
error: could not compile `bs4_generator_rs` (bin "bs4_generator_rs") due to 5 previous errors 
```

### Expected Behaviour

```bash

Hello [Name]!

```

### Additional Context

The example from docs does not work for 4.3.17.

### Debug Output

_No response_

---

_Label `C-bug` added by @JiveyGuy on 2023-07-20 22:23_

---

_Comment by @epage on 2023-07-20 22:43_

The line before the example [on docs.rs](https://docs.rs/clap/latest/clap/#example) says to run
```console
$ cargo add clap --features derive
```
The missing `--features derive` is what would be causing those failures

---

_Comment by @JiveyGuy on 2023-07-21 00:15_

Aha! 

I should have read the instructions, I just went to the examples folder on GitHub and copied the raw code. My bad. 

---

_Closed by @JiveyGuy on 2023-07-21 00:15_

---

_Referenced in [clap-rs/clap#5031](../../clap-rs/clap/pulls/5031.md) on 2023-07-21 00:20_

---

_Comment by @epage on 2023-07-21 00:20_

We post all of our examples on docs.rs, like [the cookbook](https://docs.rs/clap/latest/clap/_derive/_cookbook/index.html).  This allows them to be versioned, include example output, and provide a more discoverable way of finding the right example 

---

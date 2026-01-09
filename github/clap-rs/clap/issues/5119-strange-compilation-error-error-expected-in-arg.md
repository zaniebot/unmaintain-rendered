---
number: 5119
title: "Strange compilation error \"error: expected `,`\" in arg derive"
type: issue
state: open
author: apbr
labels:
  - C-bug
  - A-derive
assignees: []
created_at: 2023-09-11T13:09:19Z
updated_at: 2024-04-04T17:37:24Z
url: https://github.com/clap-rs/clap/issues/5119
synced_at: 2026-01-07T13:12:20-06:00
---

# Strange compilation error "error: expected `,`" in arg derive

---

_Issue opened by @apbr on 2023-09-11 13:09_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.71.0 (8ede3aae2 2023-07-12)

### Clap Version

4.4.2

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Parser, Debug)]
#[command(author, version, about)]
pub struct Args {
    #[arg(long, short, default_value_t = "foo".to_string())]
    pub some_value: String,
}

fn main() {
    let args = Args::parse();
    println!("Hello, world!");
}
```


### Steps to reproduce the bug with the above code

cargo run

### Actual Behaviour

I do get this error:
```
error: expected `,`
 --> src/main.rs:6:47
  |
6 |     #[arg(long, short, default_value_t = "foo".to_string())]
  |                                               ^
```
This message is not really helpful.

### Expected Behaviour

I don't see why this should not just compile. At least there should be a more conclusive error message.


### Additional Context

I know I can work around this by using `#[arg(long, short, default_value_t = String::from("foo"))]` or `#[arg(long, short, default_value_t = ("foo".to_string()))]`.
Though the latter option gives the following warning when compiling:
```
warning: unnecessary parentheses around assigned value
 --> src/main.rs:6:42
  |
6 |     #[arg(long, short, default_value_t = ("foo".to_string()))]
  |                                          ^                 ^
  |
  = note: `#[warn(unused_parens)]` on by default
help: remove these parentheses
  |
6 -     #[arg(long, short, default_value_t = ("foo".to_string()))]
6 +     #[arg(long, short, default_value_t = "foo".to_string())]
  |
```

### Debug Output

_No response_

---

_Label `C-bug` added by @apbr on 2023-09-11 13:09_

---

_Label `A-derive` added by @epage on 2023-09-11 14:23_

---

_Comment by @epage on 2023-09-11 14:24_

Not quite sure what is going on.  If I comment out the code that generates code for `default_value_t`, it still happens.

---

_Comment by @synthfi on 2024-03-23 23:24_

also seeing this

---

_Comment by @epage on 2024-03-25 15:40_

For anyone blocked on this, there are two workarounds for this, depending on your situation
- Use `default_value`
- Move the call out to a function

See
```rust
#!/usr/bin/env nargo
---
[dependencies]
clap = { version = "4", features = ["derive"] }
---

use clap::Parser;

#[derive(Parser, Debug)]
#[command(author, version, about)]
pub struct Args {
    //#[arg(long, short, default_value = "foo")]
    #[arg(long, short, default_value_t = "foo".to_string())]
    //#[arg(long, short, default_value_t = default())]
    pub some_value: String,
}

fn default() -> String {
    "foo".to_string()
}

fn main() {
    let args = Args::parse();
    println!("Hello, world!");
}
```

---

_Comment by @lolbinarycat on 2024-04-04 17:37_

there's another workaround i've found: surround it in parenthesies: `default_value_t = ("foo".to_string())`.

unfortunately rustc deems these "unnecessary" and will output a warning about it.

---

_Referenced in [rust-lang/rust-clippy#13000](../../rust-lang/rust-clippy/issues/13000.md) on 2024-06-27 18:28_

---

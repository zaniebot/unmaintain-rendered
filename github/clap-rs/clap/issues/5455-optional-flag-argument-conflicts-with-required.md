---
number: 5455
title: Optional flag argument conflicts with required subcommand
type: issue
state: closed
author: domenkozar
labels:
  - C-bug
assignees: []
created_at: 2024-04-15T11:32:32Z
updated_at: 2024-04-17T11:38:37Z
url: https://github.com/clap-rs/clap/issues/5455
synced_at: 2026-01-07T13:12:20-06:00
---

# Optional flag argument conflicts with required subcommand

---

_Issue opened by @domenkozar on 2024-04-15 11:32_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.76

### Clap Version

4.5.4

### Minimal reproducible code

```rust

use clap::{Parser, Subcommand};


#[derive(Parser, Debug)]
#[command(subcommand_required = true)]
struct Cli {
    #[arg(
        short,
        long,
        value_delimiter = ',',
    )]
    arg: Option<Vec<String>>,

    #[command(subcommand)]
    command: Subcommands,
}


#[derive(Subcommand, Clone, Debug)]
enum Subcommands {
    #[command()]
    Subcommand { },
}

fn main() {
    let cli = Cli::parse();
    println!("{:?}", cli);
}
```



### Steps to reproduce the bug with the above code


```
❯ cargo run -- --arg subcommand
    Finished dev [unoptimized + debuginfo] target(s) in 0.03s
     Running `target/debug/clap-arg-subcommand --arg subcommand`
error: 'clap-arg-subcommand' requires a subcommand but one was not provided
  [subcommands: subcommand, help]

Usage: clap-arg-subcommand [OPTIONS] <COMMAND>

For more information, try '--help'.
```

### Actual Behaviour

--arg consumed `subcommand` as an arg and then complains about missing subcommand

### Expected Behaviour

```
❯ cargo run -- --arg subcommand
Cli { arg: Some([]), command: Subcommand }

❯ cargo run -- subcommand
Cli { arg: None, command: Subcommand }

❯ cargo run -- --arg arg1 subcommand
Cli { arg: Some(["arg1"]), command: Subcommand }

❯ cargo run -- --arg arg1,arg2 subcommand
Cli { arg: Some(["arg1", "arg2"]), command: Subcommand }
```

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @domenkozar on 2024-04-15 11:32_

---

_Comment by @epage on 2024-04-15 14:21_

Try enabling `#[command(subcommand_precedence_over_arg = true)]`

---

_Comment by @domenkozar on 2024-04-17 11:38_

Thanks, I assume there's a good reason it's not on by default :)

---

_Closed by @domenkozar on 2024-04-17 11:38_

---

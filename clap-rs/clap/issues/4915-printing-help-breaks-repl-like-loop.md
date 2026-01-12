```yaml
number: 4915
title: Printing help breaks REPL-like loop
type: issue
state: closed
author: dominicmeyer
labels:
  - C-bug
assignees: []
created_at: 2023-05-18T14:30:21Z
updated_at: 2023-05-18T14:33:44Z
url: https://github.com/clap-rs/clap/issues/4915
synced_at: 2026-01-12T16:14:16Z
```

# Printing help breaks REPL-like loop

---

_@dominicmeyer_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.66.1

### Clap Version

4.2.4

### Minimal reproducible code

```rust
#[derive(Parser)]
#[command(multicall = true)]
pub struct ConfigCommand {
    #[command(subcomman)]
    pub cmd: Option<ConfigSubCommand>
}

#[derive(Subcommand)]
pub enum ConfigSubCommand {
    Quit, ...
}

fn main() {
    loop {
        let cmd = ConfigCommand::parse_from(vec!["help"]);

        match cmd {
            Quit => break, ...
        }
    }
}
```


### Steps to reproduce the bug with the above code

cargo run

### Actual Behaviour

When running this, help gets printed as expected, but afterwards the loop exits.

### Expected Behaviour

I expected help to print and afterwards start over again, until quit gets triggered.

### Additional Context

Of course the code above will never hit quit, it's just the minimal way to replicate the problem. In my real application the user is giving the input and with any other command than help everything functions as expected. This loop is ment to be something like a REPL.

### Debug Output

_No response_

---

_Label `C-bug` added by @dominicmeyer on 2023-05-18 14:30_

---

_Comment by @epage on 2023-05-18 14:33_

`parse_from` will call `std::process:exit` on error.  What you want is `try_parse_from`

See also https://docs.rs/clap/latest/clap/_derive/_cookbook/repl/index.html (though we don't yet have a derive version for this yet, all of the repl principles are the same)

---

_Closed by @epage on 2023-05-18 14:33_

---

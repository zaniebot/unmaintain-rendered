---
number: 6152
title: Nested Subcommands with arguments do not work
type: issue
state: open
author: matthiasbeyer
labels:
  - C-bug
  - A-derive
  - S-waiting-on-design
assignees: []
created_at: 2025-10-15T16:23:29Z
updated_at: 2025-10-15T18:08:59Z
url: https://github.com/clap-rs/clap/issues/6152
synced_at: 2026-01-07T13:12:20-06:00
---

# Nested Subcommands with arguments do not work

---

_Issue opened by @matthiasbeyer on 2025-10-15 16:23_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.90

### Clap Version

4.5.48

### Minimal reproducible code

```rust
use clap::Parser;
use clap::Subcommand;

#[derive(Parser)]
struct App {
    #[clap(subcommand)]
    command: Command,
}

#[derive(Clone, Debug, Subcommand)]
enum Command {
    Inner {
        #[clap(long, short)]
        foo: i32,
        
        #[clap(subcommand)]
        inner: Config,
    }
}

#[derive(Clone, Debug, Subcommand)]
enum Config {
    #[clap(subcommand)]
    Show,
    #[clap(subcommand)]
    Edit,
}

fn main() {
    App::parse_from(&["app", "inner", "--foo", "12", "show"]);
}
```

[playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=9e7c1208107eb14e5d7cfe5e7401e5f2)


### Steps to reproduce the bug with the above code

Run playground

### Actual Behaviour

Prints help text, which indicates the exact same that was passed

### Expected Behaviour

Parses CLI correctly

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @matthiasbeyer on 2025-10-15 16:23_

---

_Label `S-triage` added by @matthiasbeyer on 2025-10-15 16:23_

---

_Comment by @epage on 2025-10-15 16:48_

Note that the presence of `--foo 12` doesn't have anything to do with this.

Is there a reason you are setting `#[clap(subcommand)]` on `Show` and `Edit`.  To make them subcommands, its not needed, as shown by your `Command::Inner` variant.

`#[clap(subcommand)]` on an enum variant is meant for
```rust
enum Command {
    #[clap(subcommand)]
    Inner(Config)
}
```

Unsure why we allow `#[clap(subcommand)]` on unit variants.

---

_Comment by @matthiasbeyer on 2025-10-15 17:54_

> Unsure why we allow #[clap(subcommand)] on unit variants.

Probably this is the underlying issue then... removing that in my code also solved that problem for me. 🎉 

---

_Label `A-derive` added by @epage on 2025-10-15 18:08_

---

_Label `S-triage` removed by @epage on 2025-10-15 18:08_

---

_Label `S-waiting-on-design` added by @epage on 2025-10-15 18:08_

---

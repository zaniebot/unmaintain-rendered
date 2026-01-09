---
number: 5894
title: "Setting `value_parser` hides `value_enum` variants from help output"
type: issue
state: closed
author: dcsommer
labels:
  - C-bug
assignees: []
created_at: 2025-01-27T17:47:22Z
updated_at: 2025-01-28T20:18:53Z
url: https://github.com/clap-rs/clap/issues/5894
synced_at: 2026-01-07T13:12:20-06:00
---

# Setting `value_parser` hides `value_enum` variants from help output

---

_Issue opened by @dcsommer on 2025-01-27 17:47_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.84.0

### Clap Version

4.5.26

### Minimal reproducible code

Run this (e.g. in rustexplorer.com)

```rust
/*
$ play --help
*/

/*
[dependencies]
anyhow = { version = "*"}
clap = { version = "4.5.26", features = [ "derive" ] }
*/
use std::io::IsTerminal;
use clap::ValueEnum;
use clap::Parser;
use anyhow::Result;

#[derive(Clone, Parser, Debug)]
#[command(about, arg_required_else_help = true)]
struct Command {
    // XXX: this shows the default and options
    // #[arg(default_value = "off", value_enum)]
    // XXX: this shows the default but NOT the options
    #[arg(default_value = "auto", value_parser = custom_parser, value_enum)]
    color: Color,
}

#[derive(Clone, ValueEnum, Debug)]
enum Color {
    /// Color is off
    Off,
    /// Color is on
    On,
}

#[allow(dead_code)]
fn custom_parser(arg: &str) -> Result<Color> {
    if arg == "auto" {
        if std::io::stdout().is_terminal() {
            Ok(Color::On)
        } else {
            Ok(Color::Off)
        }
    } else {
        Color::from_str(arg, false)
            .map_err(|e| anyhow::anyhow!("Invalid color option: {}", e))
    }
}

fn main() {
    let c = Command::parse_from(&["--help"]);
    println!("{:?}", c);
}
```


### Steps to reproduce the bug with the above code

`cargo run`

### Actual Behaviour

The default value is shown on the help for `--color`

### Expected Behaviour

The possible values should also be shown (via the `value_enum` derive)

### Additional Context

In my experimentation, it appears the presence of `value_parser` suppresses the help string behavior of `value_enum` to show list the possible options. The presence of or value within `default_value` does not seem to matter.

### Debug Output

_No response_

---

_Label `C-bug` added by @dcsommer on 2025-01-27 17:47_

---

_Comment by @epage on 2025-01-27 18:00_

The help is driven by `ValueParser`.  If you replace it, then you lose that.

Instead of providing a custom `value_parser`, you could provide [`EnumValueParser`](https://docs.rs/clap/latest/clap/builder/struct.EnumValueParser.html) and call `map` or `try_map` on it.

However, I personally would recommend handling `auto` later by having it in the `Color` enum.

---

_Comment by @dcsommer on 2025-01-28 17:25_

1. Is it possible to provide an `EnumValueParser` from the derive macro?
2. The argument to `map` and `try_map` is the already-parsed enum value, not the input string. So it seems too late to handle `auto` there?

---

_Comment by @epage on 2025-01-28 17:58_

1. yes, you can do `value_parser = EnumValueParser...` like any other `value_parser
2. yes, you're right. I overlooked that you are doing pre-processing rather than post-processing

---

_Comment by @dcsommer on 2025-01-28 18:25_

I don't suppose there are any options for pre-processing in clap? I suppose a custom `TypedValueParser` that wraps `EnumValueParser` is one way, but is there a lighter-weight option?

---

_Comment by @epage on 2025-01-28 18:47_

Not at the moment and I feel like this might be too specialized too support natively.

---

_Closed by @dcsommer on 2025-01-28 20:18_

---

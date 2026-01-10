---
number: 6019
title: ValueEnum fails to parse variant with an unmatched ToString implementation
type: issue
state: open
author: zeozeozeo
labels:
  - C-bug
  - A-derive
assignees: []
created_at: 2025-05-28T13:10:27Z
updated_at: 2025-05-28T14:11:52Z
url: https://github.com/clap-rs/clap/issues/6019
synced_at: 2026-01-10T01:28:20Z
---

# ValueEnum fails to parse variant with an unmatched ToString implementation

---

_Issue opened by @zeozeozeo on 2025-05-28 13:10_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.89.0-nightly (45f256d9d 2025-05-27)

### Clap Version

4.5.39

### Minimal reproducible code

```rust
use clap::{Parser, ValueEnum};
use std::fmt;

#[derive(Debug, Clone, ValueEnum)]
enum Color {
    Red,
    Blue,
}

impl fmt::Display for Color {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        let color_str = match self {
            Color::Red => "Red",
            Color::Blue => "Blue",
        };
        write!(f, "{}", color_str)
    }
}

#[derive(Parser, Debug)]
#[command(version, about, long_about = None)]
struct Args {
    #[arg(short, long, default_value_t = Color::Red)]
    color: Color,
}

fn main() {
    let args = Args::parse();
    println!("You selected the color {:?}", args.color);
}
```


### Steps to reproduce the bug with the above code

`cargo run`

### Actual Behaviour

```
$ cargo run
error: invalid value 'Red' for '--color <COLOR>'
  [possible values: red, blue]

  tip: a similar value exists: 'red'

For more information, try '--help'.
error: process didn't exit successfully: `target\debug\clap-test.exe` (exit code: 2)
```

### Expected Behaviour

In stdout: `You selected the color Red`

### Additional Context

Works if you replace "Red" and "Blue" in the Display impl with "red" and "blue"
(note that `default_value_t` for `color` is correctly set to `Color::Red` - program complains either way) 

### Debug Output

```
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="clap-test"
[clap_builder::builder::command]Command::_propagate:clap-test
[clap_builder::builder::command]Command::_check_help_and_version:clap-test expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building default --version
[clap_builder::builder::command]Command::_propagate_global_args:clap-test
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:color
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:version
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::parse
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:color:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:color: has default vals
[clap_builder::parser::parser]Parser::add_default_value:iter:color: wasn't used
[clap_builder::parser::parser]Parser::react action=Set, identifier=None, source=DefaultValue
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="color", source=DefaultValue
[clap_builder::parser::parser]Parser::push_arg_values: ["Red"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
error: invalid value 'Red' for '--color <COLOR>'
  [possible values: red, blue]

  tip: a similar value exists: 'red'

For more information, try '--help'.
error: process didn't exit successfully: `target\debug\clap-test.exe` (exit code: 2)
```

---

_Label `C-bug` added by @zeozeozeo on 2025-05-28 13:10_

---

_Label `A-derive` added by @epage on 2025-05-28 13:50_

---

_Comment by @epage on 2025-05-28 13:51_

Is there a reason to have an unmatched `ToString`?

To help people catch this, we could add a `debug_assert!` in the proc macro that they match.

---

_Comment by @zeozeozeo on 2025-05-28 13:58_

> Is there a reason to have an unmatched `ToString`?

I was under impression that clap handled the PascalCase => kebab-case conversion everywhere automatically, and since ValueEnum required a ToString impl, I just did this:

```rs
impl ToString for Color {
    fn to_string(&self) -> String { format!("{:?}", self) }
}
```

but apparently this does not seem to be the case. I think clap should handle this automatically instead of just throwing an assertion

---

_Comment by @epage on 2025-05-28 14:11_

clap does not derive a `Display` automatically (which is generally what should be implemented instead of `ToString`).  We could offer one but with the caveats of (1) it would assert or be underivable when fields are skipped and (2) unsure what the common practice is for naming specialized derives like that.  I would be against `derive(ValueEnum)` automatically deriving `Display` as people may want other options and the skipped issue.

Our [git-derive](https://docs.rs/clap/latest/clap/_cookbook/git_derive/index.html) cookbook entry has an example of a `Display` on `ColorWhen`.  I could see extending our [tutorial on `ValueEnum`](https://docs.rs/clap/latest/clap/_derive/_tutorial/index.html#enumerated-values) to also cover this.

Note: If you apply the `#[arg(value_enum)]` attribute to `color`, then `Display` won't be used.

---

---
number: 4339
title: "Strange empty default value for Vec<String> in clap_derive"
type: issue
state: closed
author: JakkuSakura
labels:
  - C-bug
assignees: []
created_at: 2022-10-03T20:23:51Z
updated_at: 2022-10-04T16:29:33Z
url: https://github.com/clap-rs/clap/issues/4339
synced_at: 2026-01-10T01:27:53Z
---

# Strange empty default value for Vec<String> in clap_derive

---

_Issue opened by @JakkuSakura on 2022-10-03 20:23_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.66.0-nightly (57f097ea2 2022-10-01)

### Clap Version

4.0.8

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Parser)]
#[clap(author, version, about, long_about = None)]
struct CliArgument {
    #[clap(long, default_value = "", env = "PUB_CERTS", value_delimiter = ',')]
    pub_certs: Vec<String>,
}
fn main() {
    let args: CliArgument = CliArgument::parse();
    assert_eq!(args.pub_certs, vec![]);
}

```


### Steps to reproduce the bug with the above code

`cargo run`

### Actual Behaviour

panic,
pub_certs == vec![""]

### Expected Behaviour

does not panic,
pub_certs == vec![]

### Additional Context
If I use this, it works fine
```rust
 #[clap(long, env = "PUB_CERTS", value_delimiter = ',')]
    pub_certs: Option<Vec<String>>,
```

### Debug Output

[      clap::builder::command] 	Command::_do_parse
...
[        clap::parser::parser] 	Parser::add_defaults:iter:pub_certs:
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:pub_certs: has default vals
[        clap::parser::parser] 	Parser::add_default_value:iter:pub_certs: wasn't used
[        clap::parser::parser] 	Parser::react action=Append, identifier=None, source=DefaultValue
[   clap::parser::arg_matcher] 	ArgMatcher::start_custom_arg: id="pub_certs", source=DefaultValue
[      clap::builder::command] 	Command::groups_for_arg: id="pub_certs"
[   clap::parser::arg_matcher] 	ArgMatcher::start_custom_arg: id="CliArgument", source=DefaultValue
[        clap::parser::parser] 	Parser::push_arg_values: [""]
[        clap::parser::parser] 	Parser::add_single_val_to_arg: cur_idx:=5
[      clap::builder::command] 	Command::groups_for_arg: id="pub_certs"
[   clap::parser::arg_matcher] 	ArgMatcher::needs_more_vals: o=pub_certs, pending=0
[   clap::parser::arg_matcher] 	ArgMatcher::needs_more_vals: expected=1, actual=0
[        clap::parser::parser] 	Parser::react not enough values passed in, leaving it to the validator to complain
[        clap::parser::parser] 	Parser::add_defaults:iter:priv_cert:
[        clap::parser::parser] 	Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser] 	Parser::add_default_value:iter:priv_cert: has default vals
[        clap::parser::parser] 	Parser::add_default_value:iter:priv_cert: wasn't used
[        clap::parser::parser] 	Parser::react action=Set, identifier=None, source=DefaultValue
[   clap::parser::arg_matcher] 	ArgMatcher::start_custom_arg: id="priv_cert", source=DefaultValue
[      clap::builder::command] 	Command::groups_for_arg: id="priv_cert"
[   clap::parser::arg_matcher] 	ArgMatcher::start_custom_arg: id="CliArgument", source=DefaultValue
[        clap::parser::parser] 	Parser::push_arg_values: [""]
AppConfig {
    pub_certs: [
        "",
    ]
}

---

_Label `C-bug` added by @JakkuSakura on 2022-10-03 20:23_

---

_Referenced in [clap-rs/clap#4346](../../clap-rs/clap/issues/4346.md) on 2022-10-04 15:15_

---

_Comment by @epage on 2022-10-04 16:27_

To make this easier to reproduce:
```rust
#!/usr/bin/env -S rust-script

//! ```cargo
//! [dependencies]
//! clap = { version = "4.0.4", features = ["derive", "wrap_help", "env"] }
//! ```

use clap::Parser;

#[derive(Parser)]
#[clap(author, version, about, long_about = None)]
struct CliArgument {
    #[clap(long, default_value = "", env = "PUB_CERTS", value_delimiter = ',')]
    pub_certs: Vec<String>,
}
fn main() {
    let args: CliArgument = CliArgument::parse();
    let expected: Vec<String> = Vec::new();
    assert_eq!(args.pub_certs, expected);
}
```

---

_Comment by @epage on 2022-10-04 16:29_

clap is doing what it is told.  It was given an empty string as a default value.  That is different than defaulting to an empty `Vec`.

clap will do "the right thing" if you just remove the `default_value`
```rust
#!/usr/bin/env -S rust-script

//! ```cargo
//! [dependencies]
//! clap = { version = "4.0.4", features = ["derive", "wrap_help", "env"] }
//! ```

use clap::Parser;

#[derive(Parser)]
#[clap(author, version, about, long_about = None)]
struct CliArgument {
    #[clap(long, env = "PUB_CERTS", value_delimiter = ',')]
    pub_certs: Vec<String>,
}
fn main() {
    let args: CliArgument = CliArgument::parse();
    let expected: Vec<String> = Vec::new();
    assert_eq!(args.pub_certs, expected);
}
```

---

_Closed by @epage on 2022-10-04 16:29_

---

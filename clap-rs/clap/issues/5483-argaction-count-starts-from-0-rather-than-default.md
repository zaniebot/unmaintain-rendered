```yaml
number: 5483
title: "`ArgAction::Count` starts from `0` rather than `default_value`"
type: issue
state: open
author: eirnym
labels:
  - C-bug
  - M-breaking-change
  - S-waiting-on-decision
  - A-parsing
assignees: []
created_at: 2024-05-03T12:48:25Z
updated_at: 2024-05-03T16:01:04Z
url: https://github.com/clap-rs/clap/issues/5483
synced_at: 2026-01-12T16:14:17Z
```

# `ArgAction::Count` starts from `0` rather than `default_value`

---

_@eirnym_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.78.0 (9b00956e5 2024-04-29)

### Clap Version

4.5.0

### Minimal reproducible code

```rust
use clap::{ArgAction, Parser};

#[derive(Debug, Clone, Parser)]
pub struct CliArgs {
    #[clap(short = 'v', action = ArgAction::Count, default_value_t=5)]
    pub verbose: u8,
}
fn main() {
    let args = CliArgs::parse();
    println!("verbose count: {}", args.verbose);
}
```


### Steps to reproduce the bug with the above code

```shell
$ cargo run -- -vvv
```

### Actual Behaviour

`default_value_t` is not taken into consideration for the starting point of `Count`

output:

```shell 
$ cargo run 
verbose count: 5
$ cargo run -- -vvv
verbose count: 3
```
### Expected Behaviour

`default_value_t` is taken into consideration for the starting point of `Count`

output:

```shell 
$ cargo run 
verbose count: 5
$ cargo run -- -vvv
verbose count: 8
```

### Additional Context


### Debug Output

_No response_

---

_Label `C-bug` added by @eirnym on 2024-05-03 12:48_

---

_Renamed from "ArgAction::Count doesn't " to "ArgAction::Count doesn't recognize default_value_t" by @eirnym on 2024-05-03 13:44_

---

_Renamed from "ArgAction::Count doesn't recognize default_value_t" to "`ArgAction::Count` doesn't recognize `default_value_t`" by @eirnym on 2024-05-03 13:45_

---

_Comment by @epage on 2024-05-03 15:13_

I am seeing the same behavior for both `default_value` and `default_value_t` and the behavior is correct.

Could you give more details, like copies of the output that is expected and unexpected?

```rust
#!/usr/bin/env nargo
---
[dependencies]
clap = { path = "../clap", features = ["derive"] }
---

use clap::{ArgAction, Parser};

#[derive(Debug, Clone, Parser)]
pub struct CliArgs {
    #[arg(short = 'v', action = ArgAction::Count, default_value_t=5)]
    pub verbose: u8,
}
fn main() {
    let args = CliArgs::parse();
    println!("verbose count: {}", args.verbose);
}
```
```console
$ ./clap-5483.rs
verbose count: 5
$ ./clap-5483.rs -vvv
verbose count: 3
```

Running
```rust
#!/usr/bin/env nargo
---
[dependencies]
clap = { path = "../clap", features = ["derive"] }
---

use clap::{ArgAction, Parser};

#[derive(Debug, Clone, Parser)]
pub struct CliArgs {
    #[arg(short = 'v', action = ArgAction::Count, default_value="5")]
    pub verbose: u8,
}
fn main() {
    let args = CliArgs::parse();
    println!("verbose count: {}", args.verbose);
}
```
```console
$ ./clap-5483.rs
verbose count: 5
$ ./clap-5483.rs -vvv
verbose count: 3
```

---

_Comment by @eirnym on 2024-05-03 15:26_

@epage Thank you for your reply. I updated my comment above. TL;DR I expect, that `ArgAction::Count` will start count starting at given value. From documentation this action sets `default_value` to 0 if not provided, so I assumed, that it will start counting at `default_value`

---

_Comment by @epage on 2024-05-03 15:33_

In all ways within clap, `default_value` is assigned if nothing else is used.  iirc Python's argparse has the same behavior and I've run into the same frustration :).

This means that your application needs to take this into account.  https://github.com/clap-rs/clap-verbosity-flag is a crate that encodes all of this for that one specific use case.

I'm hesitant to change it and inclined to close this but I have just enough doubt that I think I'll leave this open to see what additional insights it might generate.

---

_Renamed from "`ArgAction::Count` doesn't recognize `default_value_t`" to "`ArgAction::Count` starts from `0` rather than `default_value`" by @epage on 2024-05-03 15:34_

---

_Comment by @epage on 2024-05-03 15:36_

I've updated the Issue to reflect the latest clarification.  To be clear, there is no difference in behavior between `default_value` and `default_value_t`.  If you are seeing some, we'd need a reproduction case to better understand it.

---

_Label `S-waiting-on-decision` added by @epage on 2024-05-03 15:36_

---

_Label `A-parsing` added by @epage on 2024-05-03 15:36_

---

_Label `M-breaking-change` added by @epage on 2024-05-03 15:36_

---

_Comment by @eirnym on 2024-05-03 15:41_

It could be a different action if there's no way to change this one. At least clarification for this action is required.

Example could be found in my recently published [tool](https://github.com/eirtools/sqlgrep) I'd like to start my verbose level with 2 (Info). `-q` is an opposite of `-v` and then I calculate actual verbosity level. (beginning of main.rs and args.rs)

---

_Comment by @epage on 2024-05-03 16:01_

Added a clarification in 11ff6ccb0d6d06280f1c6e60b056bfb6e5487ef2

There is a cost for everything we add to the API.   I'd be hesitant to add a variant that changes the default behavior especially when this can be worked around.  I'd be more inclined towards the custom Actions that I've been considering.

---

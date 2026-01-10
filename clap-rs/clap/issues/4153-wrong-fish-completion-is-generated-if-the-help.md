---
number: 4153
title: Wrong fish completion is generated if the help description of the value has the comma
type: issue
state: closed
author: sorairolake
labels:
  - C-bug
  - A-completion
  - E-easy
assignees: []
created_at: 2022-08-31T04:50:38Z
updated_at: 2022-09-02T13:48:25Z
url: https://github.com/clap-rs/clap/issues/4153
synced_at: 2026-01-10T01:27:51Z
---

# Wrong fish completion is generated if the help description of the value has the comma

---

_Issue opened by @sorairolake on 2022-08-31 04:50_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.63.0

### Clap Version

3.2.18

### Minimal reproducible code

```rust
use std::io;

use clap::{CommandFactory, Parser, ValueEnum};
use clap_complete::Shell;

#[derive(Parser)]
struct Opt {
    /// Help message.
    #[clap(long, value_enum)]
    arg: Value,
}

#[derive(Clone, ValueEnum)]
pub enum Value {
    /// Alice and Bob.
    Foo,

    /// Carol, a third participant.
    Bar,
}

fn main() {
    clap_complete::generate(Shell::Fish, &mut Opt::command(), "test", &mut io::stdout());
}
```


### Steps to reproduce the bug with the above code

`cargo run`

### Actual Behaviour

The following completions are generated:

```fish
complete -c test -l arg -d 'Help message' -r -f -a "{foo        Alice and Bob,bar       Carol, a third participant}"
complete -c test -s h -l help -d 'Print help information'
```

### Expected Behaviour

The comma in the help description should be escaped:

```fish
complete -c test -l arg -d 'Help message' -r -f -a "{foo        Alice and Bob,bar       Carol\, a third participant}"
complete -c test -s h -l help -d 'Print help information'
```

### Additional Context

- `clap_complete`: 3.2.4

### Debug Output

_No response_

---

_Label `C-bug` added by @sorairolake on 2022-08-31 04:50_

---

_Label `A-completion` added by @epage on 2022-08-31 11:52_

---

_Label `E-easy` added by @epage on 2022-08-31 11:52_

---

_Referenced in [clap-rs/clap#4174](../../clap-rs/clap/pulls/4174.md) on 2022-09-02 12:38_

---

_Closed by @epage on 2022-09-02 13:48_

---

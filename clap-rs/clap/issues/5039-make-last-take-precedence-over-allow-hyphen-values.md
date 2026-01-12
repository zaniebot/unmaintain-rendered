```yaml
number: 5039
title: "Make `last` take precedence over `allow_hyphen_values`"
type: issue
state: open
author: mamekoro
labels:
  - C-enhancement
  - S-waiting-on-decision
  - A-parsing
assignees: []
created_at: 2023-07-22T15:06:58Z
updated_at: 2024-06-26T21:09:29Z
url: https://github.com/clap-rs/clap/issues/5039
synced_at: 2026-01-12T16:14:16Z
```

# Make `last` take precedence over `allow_hyphen_values`

---

_@mamekoro_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.3.19

### Describe your use case

former discussion: https://github.com/clap-rs/clap/discussions/4960

My program needs 2 kinds of command line arguments.
One is the options for the program itself (called `opts`), and the other is the command and its arguments passed to an external program (called `cmdline`).

I'd like to distinguish between `opts` and `cmdline` by separating them with a double hyphen `--`. However, it does not work because `allow_hyphen_values` has higher precedence than `last`.

Here is an example:

```rust
use clap::Parser;

#[derive(Clone, Debug, Parser)]
pub struct Args {
    #[arg(allow_hyphen_values = true)]
    pub opts: Vec<String>,

    #[arg(allow_hyphen_values = true, last = true)]
    pub cmdline: Vec<String>,
}

fn main() {
    let args = Args::parse();
    println!("opts = {:?}, cmdline = {:?}", args.opts, args.cmdline);
}
```

Run it in Bash.

```bash
alias run='cargo run --quiet --'

# The following two are bugged.

run --name xyz -- ls -l
# Expected: opts = ["--name", "xyz"],                   cmdline = ["ls", "-l"]
#   Actual: opts = ["--name", "xyz", "--", "ls", "-l"], cmdline = []

run --name xyz --
# Expected: opts = ["--name", "xyz"],       cmdline = []
#   Actual: opts = ["--name", "xyz", "--"], cmdline = []

# The following work correctly.

run --name xyz
# Expected: opts = ["--name", "xyz"], cmdline = []
#   Actual: opts = ["--name", "xyz"], cmdline = []

run -- ls -l
# Expected: opts = [], cmdline = ["ls", "-l"]
#   Actual: opts = [], cmdline = ["ls", "-l"]

run --
# Expected: opts = [], cmdline = []
#   Actual: opts = [], cmdline = []

run
# Expected: opts = [], cmdline = []
#   Actual: opts = [], cmdline = []
```


### Describe the solution you'd like

`last` should take precedence over `allow_hyphen_values`.


### Alternatives, if applicable

`value_terminator = "--"` would be an alternative, but it is ambiguous when the arguments does not contain the terminator `--`. (see comparison below)

On the other hand, `last` **requires** the terminator in the arguments.
This is why I prefer `last = true` to `value_terminator = "--"`; `last` is less confusing.

<details>
<summary>comparison of <code>value_terminator = "--"</code> and <code>last = true</code></summary>

```rust
use clap::Parser;

// This uses `value_terminator = "--"`.
#[derive(Clone, Debug, Parser)]
pub struct ValueTerminator {
    #[arg(allow_hyphen_values = true, value_terminator = "--")]
    pub opts: Vec<String>,

    #[arg(allow_hyphen_values = true)]
    pub cmdline: Vec<String>,
}

// This uses `last = true`.
#[derive(Clone, Debug, Parser)]
pub struct Last {
    #[arg(allow_hyphen_values = true)]
    pub opts: Vec<String>,

    #[arg(allow_hyphen_values = true, last = true)]
    pub cmdline: Vec<String>,
}

fn main() {
    let args = ValueTerminator::parse_from(["program", "--name", "xyz"]);
    println!("opts = {:?}, cmdline = {:?}", args.opts, args.cmdline);
    // Expected: opts = ["--name", "xyz"], cmdline = []
    //   Actual: opts = ["--name", "xyz"], cmdline = []

    let args = ValueTerminator::parse_from(["program", "--", "ls", "-l"]);
    println!("opts = {:?}, cmdline = {:?}", args.opts, args.cmdline);
    // Expected: opts = [], cmdline = ["ls", "-l"]
    //   Actual: opts = ["ls", "-l"], cmdline = []
    //
    // This is a bug of `value_terminator` and is outside the scope of this issue.
    // See #5040 (value_terminator has no effect when it is the first argument).

    let args = ValueTerminator::parse_from(["program", "ls", "-l"]);
    println!("opts = {:?}, cmdline = {:?}", args.opts, args.cmdline);
    // Expected: ?
    //   I don't know what to expect.
    //   The terminator "--" is not passed to the arguments,
    //   so it is ambiguous whether ["ls", "-l"] is opt or cmdline.
    //   This is why I prefer `last = true` to `value_terminator = "--"`.
    //   `value_terminator = "--"` is ambiguous when no terminator is passed.
    // Actual: opts = ["ls", "-l"], cmdline = []

    let args = ValueTerminator::parse_from(["program", "--name", "xyz", "--", "ls", "-l"]);
    println!("opts = {:?}, cmdline = {:?}", args.opts, args.cmdline);
    // Expected: opts = ["--name", "-xyz"], cmdline = ["ls", "-l"]
    //   Actual: opts = ["--name", "-xyz"], cmdline = ["ls", "-l"]


    let args = Last::parse_from(["program", "--name", "xyz"]);
    println!("opts = {:?}, cmdline = {:?}", args.opts, args.cmdline);
    // Expected: opts = ["--name", "xyz"], cmdline = []
    //   Actual: opts = ["--name", "xyz"], cmdline = []

    let args = Last::parse_from(["program", "--", "ls", "-l"]);
    println!("opts = {:?}, cmdline = {:?}", args.opts, args.cmdline);
    // Expected: opts = [], cmdline = ["ls", "-l"]
    //   Actual: opts = [], cmdline = ["ls", "-l"]

    let args = Last::parse_from(["program", "ls", "-l"]);
    println!("opts = {:?}, cmdline = {:?}", args.opts, args.cmdline);
    // Expected: opts = ["ls", "-l"], cmdline = []
    //   Actual: opts = ["ls", "-l"], cmdline = []
    //
    // `["ls", "-l"]` is parsed as `opts` in this case,
    // but this is correct because the terminator `--` is not passed.

    let args = Last::parse_from(["program", "--name", "xyz", "--", "ls", "-l"]);
    println!("opts = {:?}, cmdline = {:?}", args.opts, args.cmdline);
    // Expected: opts = ["--name", "xyz"], cmdline = ["ls", "-l"]
    //   Actual: opts = ["--name", "xyz", "--", "ls", "-l"], cmdline = []
    // This is the bug that this issue points to.
}
```

---

_Label `C-enhancement` added by @mamekoro on 2023-07-22 15:06_

---

_Label `A-parsing` added by @epage on 2023-07-24 19:28_

---

_Label `S-waiting-on-decision` added by @epage on 2023-07-24 19:29_

---

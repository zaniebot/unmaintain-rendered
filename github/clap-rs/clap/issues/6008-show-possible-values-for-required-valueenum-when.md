---
number: 6008
title: show possible values for required ValueEnum when omitting the argument
type: issue
state: closed
author: m4rch3n1ng
labels:
  - C-enhancement
  - A-help
assignees: []
created_at: 2025-05-22T13:36:05Z
updated_at: 2025-05-22T13:49:54Z
url: https://github.com/clap-rs/clap/issues/6008
synced_at: 2026-01-07T13:12:20-06:00
---

# show possible values for required ValueEnum when omitting the argument

---

_Issue opened by @m4rch3n1ng on 2025-05-22 13:36_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

v4.5.38 / master at commit [e2188d9a]

### Describe your use case

when not providing a value for a required `ValueEnum`, clap doesn't show the possible values. i have to explicitly call `--help` or guess a possible value and then clap shows me the possible values.

example code:

```rust
use clap::{Parser, ValueEnum};

#[derive(Parser)]
enum Args {
	Cycle {
		#[arg(value_enum)]
		direction: Direction,
	},
}

#[derive(Clone, Copy, ValueEnum)]
enum Direction {
	Next,
	Prev,
}

fn main() {
	let _ = Args::parse();
}
```

and the output:

```
$ cargo r -q cycle
error: the following required arguments were not provided:
  <DIRECTION>

Usage: stuff cycle <DIRECTION>

For more information, try '--help'.
```

```
$ cargo r -q cycle what
error: invalid value 'what' for '<DIRECTION>'
  [possible values: next, prev]

For more information, try '--help'.
```

```
$ cargo r -q cycle --help
Usage: stuff cycle <DIRECTION>

Arguments:
  <DIRECTION>  [possible values: next, prev]

Options:
  -h, --help  Print help
```

as you can see, the only version of that where it doesn't show the possible values for `<DIRECTION>` is when you omit it entirely

### Describe the solution you'd like

i would like the output for the error when omitting the required value enum entirely to also show all possible values.

```
$ cargo r -q cycle
error: the following required arguments were not provided:
  <DIRECTION>  [possible values: next, prev]

Usage: stuff cycle <DIRECTION>

For more information, try '--help'.
```

i think it would be a lot nicer if it showed directly what values i would be able to pass in, instead of me having to do `--help` or guess to see what i can do.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @m4rch3n1ng on 2025-05-22 13:36_

---

_Label `A-help` added by @epage on 2025-05-22 13:48_

---

_Referenced in [clap-rs/clap#5156](../../clap-rs/clap/issues/5156.md) on 2025-05-22 13:49_

---

_Comment by @epage on 2025-05-22 13:49_

While #5156 is more general, I feel like there is enough overlap that I'm going to close in favor of that.  If there is a reason to keep this open as a separate issue, let us know!

---

_Closed by @epage on 2025-05-22 13:49_

---

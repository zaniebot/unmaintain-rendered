```yaml
number: 3835
title: Argument descriptions not aligned when mixing positional arguments with others under a help heading
type: issue
state: closed
author: pemistahl
labels:
  - C-bug
  - A-help
  - E-medium
assignees: []
created_at: 2022-06-15T07:52:02Z
updated_at: 2025-04-01T16:50:38Z
url: https://github.com/clap-rs/clap/issues/3835
synced_at: 2026-01-12T16:14:15Z
```

# Argument descriptions not aligned when mixing positional arguments with others under a help heading

---

_@pemistahl_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.61.0 (fe5b13d68 2022-05-18)

### Clap Version

3.2.2

### Minimal reproducible code

```rust
use clap::Parser;
use std::path::PathBuf;

#[derive(Parser)]
struct Cli {
    /// One or more test cases separated by blank space.
    #[clap(value_name = "INPUT", value_parser, help_heading = "INPUT")]
    input: Vec<String>,

    /// Reads test cases on separate lines from a file.
    #[clap(
        name = "file",
        value_name = "FILE",
        short,
        long,
        value_parser,
        help_heading = "INPUT"
    )]
    file_path: Option<PathBuf>
}

fn main() {
    let cli = Cli::parse();
}
```


### Steps to reproduce the bug with the above code

1. Compile the binary.
2. Print the help using `binary -h`.

### Actual Behaviour

The help messages are not properly aligned.

```
INPUT:
    -f, --file <FILE>    Reads test cases on separate lines from a file
    <INPUT>...       One or more test cases separated by blank space
```

### Expected Behaviour

The help messages should be properly aligned.

```
INPUT:
    -f, --file <FILE>    Reads test cases on separate lines from a file
    <INPUT>...           One or more test cases separated by blank space
```

### Additional Context

I understand that proper alignment is not always possible or appropriate. Grouped arguments, however, should always be aligned correctly, in my opinion. The problem here is probably that help messages of flags are aligned differently than help messages of arguments receiving arbitrary input.

### Debug Output

_No response_

---

_Label `C-bug` added by @pemistahl on 2022-06-15 07:52_

---

_Referenced in [clap-rs/clap#3836](../../clap-rs/clap/pulls/3836.md) on 2022-06-15 15:04_

---

_Label `A-help` added by @epage on 2022-06-15 15:04_

---

_Label `E-medium` added by @epage on 2022-06-15 15:04_

---

_Comment by @epage on 2022-06-15 15:06_

Thanks for reporting this!

Currently, our help output is making the assumption that positionals and named arguments are in separate sections.  Supporting them in the same section will take a little bit of finagling to ensure all of the padding situations work out correctly especially to not over-pad in positional-only cases.

If we can reduce leading padding in long-flag only cases, that'd be a bonus to fixing this.

---

_Renamed from "Help messages of grouped arguments are not properly aligned" to "Argument descriptions not aligned when mixing positional arguments with others under a help heading" by @epage on 2022-06-17 17:16_

---

_Referenced in [clap-rs/clap#5963](../../clap-rs/clap/pulls/5963.md) on 2025-03-30 07:33_

---

_Closed by @epage on 2025-04-01 16:50_

---

_Closed by @epage on 2025-04-01 16:50_

---

_Referenced in [rust-lang/cargo#11072](../../rust-lang/cargo/issues/11072.md) on 2025-11-10 16:55_

---

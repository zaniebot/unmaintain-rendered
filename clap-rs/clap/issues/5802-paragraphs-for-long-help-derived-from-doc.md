---
number: 5802
title: Paragraphs for long help derived from doc comments may be splitted on more cases
type: issue
state: closed
author: JohanVonElectrum
labels:
  - C-bug
  - A-derive
assignees: []
created_at: 2024-11-01T16:35:05Z
updated_at: 2024-11-04T19:06:53Z
url: https://github.com/clap-rs/clap/issues/5802
synced_at: 2026-01-10T01:28:17Z
---

# Paragraphs for long help derived from doc comments may be splitted on more cases

---

_Issue opened by @JohanVonElectrum on 2024-11-01 16:35_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.80.0 (051478957 2024-07-21)

### Clap Version

4.5.20

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Debug, Parser)]
struct GlobalArgs {
    /// Verbose level
    ///
    /// Levels:  
    /// - 0: info + warn + error  
    /// - 1: debug + info + warn + error  
    /// - 2: trace + debug + info + warn + error
    #[arg(short, long, action = clap::ArgAction::Count)]
    verbose: u8,
}

#[derive(Debug, Parser)]
#[command(version, about, long_about = None)]
struct Args {
    #[clap(flatten)]
    global: GlobalArgs,
}

fn main() {
    let args = Args::parse();
    println!("{:?}", args);
}
```


### Steps to reproduce the bug with the above code

cargo run -- --help

### Actual Behaviour

Options:
  -v, --verbose...
          Verbose mode, print more information

          Levels: - 0: info + warn + error - 1: debug + info + warn + error - 2: trace + debug + info + warn + error


### Expected Behaviour

Options:
  -v, --verbose...
          Verbose mode, print more information

          Levels:
          - 0: info + warn + error
          - 1: debug + info + warn + error
          - 2: trace + debug + info + warn + error


### Additional Context

I expect a paragraph to split on lines ending on double space or backslash or when starting by `- ` or `<num>. `. Note that in the provided minimal example the lines from `Level:  ` to `- 1: ...` end with double space.

### Debug Output

_No response_

---

_Label `C-bug` added by @JohanVonElectrum on 2024-11-01 16:35_

---

_Referenced in [clap-rs/clap#5803](../../clap-rs/clap/pulls/5803.md) on 2024-11-01 16:36_

---

_Assigned to @epage by @epage on 2024-11-04 18:17_

---

_Label `A-derive` added by @epage on 2024-11-04 18:17_

---

_Unassigned @epage by @epage on 2024-11-04 18:17_

---

_Comment by @epage on 2024-11-04 18:18_

This appears to be a duplicate of #2389, closing in favor of that

---

_Closed by @epage on 2024-11-04 18:18_

---

_Comment by @JohanVonElectrum on 2024-11-04 18:46_

Thank you for pointing that out. I had searched for issues related to newlines but didn’t find #2389, as it addresses general markdown parsing. I now see the overlap and will contribute to the discussion there. Thanks for the guidance!

---

_Comment by @epage on 2024-11-04 19:06_

Fully understand the difficulty of finding issues and identifying whats relevant!

---

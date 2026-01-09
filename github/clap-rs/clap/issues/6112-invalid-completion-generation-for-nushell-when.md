---
number: 6112
title: "Invalid completion generation for nushell when option is of style `--option.suboption <value>`"
type: issue
state: open
author: 0xferrous
labels:
  - C-bug
  - E-medium
  - A-completion
assignees: []
created_at: 2025-08-27T08:10:20Z
updated_at: 2025-08-27T14:35:15Z
url: https://github.com/clap-rs/clap/issues/6112
synced_at: 2026-01-07T13:12:20-06:00
---

# Invalid completion generation for nushell when option is of style `--option.suboption <value>`

---

_Issue opened by @0xferrous on 2025-08-27 08:10_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.91.0-nightly (9eb4a2652 2025-08-18)

### Clap Version

clap_complete_nushell = "4.5.8", clap = "4.5.46", clap_complete = "4.5.57"

### Minimal reproducible code

```rust
use clap::CommandFactory;
use clap_complete::aot::generate;

#[derive(clap::Parser)]
struct Args {
    #[arg(long = "nested.option")]
    pub nested_option: Option<u64>,
}

fn main() {
    generate(
        clap_complete_nushell::Nushell,
        &mut Args::command(),
        "repro",
        &mut std::io::stdout(),
    );
}
```


### Steps to reproduce the bug with the above code

`cargo r`

### Actual Behaviour

```nushell
module completions {

  export extern repro [
    --nested.option: string
    --help(-h)                # Print help
  ]

}

export use completions *
```

generated completion is not valid nushell:

```
Error: nu::parser::parse_mismatch

  × Parse mismatch during operation.
   ╭─[entry #1:2:5]
 1 │   export extern repro [
 2 │     --nested.option: string
   ·     ───────┬───────
   ·            ╰── expected valid variable name for this long flag
 3 │     --help(-h)                # Print help
   ╰────
```

### Expected Behaviour

Not sure what the valid generation for this option's completion will be(i will try to look into this soon), but whatever is generated should be valid nushell.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @0xferrous on 2025-08-27 08:10_

---

_Label `E-medium` added by @epage on 2025-08-27 14:35_

---

_Label `A-completion` added by @epage on 2025-08-27 14:35_

---

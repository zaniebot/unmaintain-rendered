```yaml
number: 5171
title: "bug(clap_complete):  shell.generate  function always panic"
type: issue
state: closed
author: ahaoboy
labels:
  - C-bug
assignees: []
created_at: 2023-10-12T07:56:45Z
updated_at: 2023-10-12T12:53:25Z
url: https://github.com/clap-rs/clap/issues/5171
synced_at: 2026-01-12T16:14:16Z
```

# bug(clap_complete):  shell.generate  function always panic

---

_@ahaoboy_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.74.0-nightly (65ea825f4 2023-09-18)

### Clap Version

[[package]] name = "clap" version = "4.4.6" -- dependencies = [  "clap_builder",  "clap_derive", ] -- [[package]] name = "clap_builder" version = "4.4.6" --  "anstyle",  "clap_lex",  "strsim", -- [[package]] name = "clap_complete" version = "4.4.3" -- dependencies = [  "clap", ] -- [[package]] name = "clap_derive" version = "4.4.2" -- [[package]] name = "clap_lex" version = "0.5.1" -- dependencies = [  "clap",  "clap_complete",  "futures",

### Minimal reproducible code

```rust
use clap::{CommandFactory, Parser};
use clap_complete::{generate, Shell, Generator};
#[derive(Parser, Debug, PartialEq)]
#[clap(name = "bug", version = env!("CARGO_PKG_VERSION"), bin_name = "bug")]
struct Opt {
    #[arg(long = "generate", value_enum)]
    generator: Option<Shell>,
}
fn main() {
    let opt = Opt::parse();

    if let Some(shell) = opt.generator {
        let mut cmd = Opt::command();
        let name = cmd.get_name().to_string();

        // error
        // shell.generate(&cmd, &mut std::io::stdout());

        // ok
        generate(shell, &mut cmd, name, &mut std::io::stdout())
    } else {
        println!("{opt:#?}");
    }
}

```


### Steps to reproduce the bug with the above code

```
 cargo run  -- --generate fish
```

### Actual Behaviour

```
thread 'main' panicked at ---rsproxy.cn-8f6827c7555bfaf8\clap_complete-4.4.3\src\shells\fish.rs:158:31:
built
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
error: process didn't exit successfully: `target\debug\x.exe --generate fish` (exit code: 101)

```

### Expected Behaviour

Both ways should succeed
```

let mut cmd = Opt::command();
let name = cmd.get_name().to_string();

// error
// shell.generate(&cmd, &mut std::io::stdout());

// ok
generate(shell, &mut cmd, name, &mut std::io::stdout())

```

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @ahaoboy on 2023-10-12 07:56_

---

_Comment by @ahaoboy on 2023-10-12 12:35_

Run build first
```
let mut cmd = Opt::command();
cmd.build();
shell.generate(&cmd, &mut std::io::stdout());
```

---

_Closed by @ahaoboy on 2023-10-12 12:35_

---

_Comment by @epage on 2023-10-12 12:53_

Its generally expected that you use [generate](https://docs.rs/clap_complete/latest/clap_complete/generator/fn.generate.html)

---

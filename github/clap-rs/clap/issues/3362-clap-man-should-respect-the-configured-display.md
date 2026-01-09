---
number: 3362
title: "`clap_man` should respect the configured display order for args and subcommands"
type: issue
state: closed
author: epage
labels:
  - C-bug
  - E-help-wanted
  - E-easy
  - A-man
assignees: []
created_at: 2022-01-28T21:28:12Z
updated_at: 2025-10-22T15:14:15Z
url: https://github.com/clap-rs/clap/issues/3362
synced_at: 2026-01-07T13:12:19-06:00
---

# `clap_man` should respect the configured display order for args and subcommands

---

_Issue opened by @epage on 2022-01-28 21:28_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.57.0 (f1edd0429 2021-11-29)

### Clap Version

v3.0.6

### Minimal reproducible code

TBD

### Steps to reproduce the bug with the above code

TBD

### Actual Behaviour

Arguments come back in whatever order the `App` provides rather than what is configured

### Expected Behaviour

Arguments show up in the configured order

### Additional Context

Split out of #3174

### Debug Output

_No response_

---

_Label `C-bug` added by @epage on 2022-01-28 21:28_

---

_Label `E-easy` added by @epage on 2022-01-28 21:28_

---

_Label `A-man` added by @epage on 2022-01-28 21:28_

---

_Referenced in [clap-rs/clap#3373](../../clap-rs/clap/pulls/3373.md) on 2022-01-31 19:26_

---

_Label `E-help-wanted` added by @epage on 2022-06-16 21:12_

---

_Referenced in [nextest-rs/nextest#312](../../nextest-rs/nextest/pulls/312.md) on 2022-06-25 20:39_

---

_Comment by @ericgumba on 2025-07-16 07:20_

Hi @epage is this issue still relevant? I tried to reproduce this via

`
// build.rs
use clap::{arg, Command};
use clap_mangen::Man;
use std::fs;
use std::io;
use std::path::PathBuf;

fn main() -> io::Result<()> {
    let out_dir = PathBuf::from(std::env::var_os("OUT_DIR").ok_or(io::ErrorKind::NotFound)?);

    let cmd = Command::new("myapp")
        .version("0.1.0")
        .about("Does cool things")
        .arg(arg!(-b --beta <VALUE> "Beta option"))
        .arg(arg!(-a --alpha <VALUE> "Alpha option"))
        .subcommand(
            Command::new("zulu").about("Zulu command")
        )
        .subcommand(
            Command::new("alpha").about("Alpha command")
        );

    let man = Man::new(cmd);

    let mut buffer = Vec::new();
    man.render(&mut buffer)?;

    fs::write(out_dir.join("myapp.1"), buffer)?;

    Ok(())
}`

After that I tried examining the man page that was generated via 

`groff -man -Tutf8 target/debug/build/fuck-*/out/myapp.1 | less -R`

and the resulting output is the same order that was configured in the code block (beta, alpha for the args and zulu alpha in the subcommands.

<img width="649" height="519" alt="Image" src="https://github.com/user-attachments/assets/f873f5f9-41c5-4d6f-b76d-96411a691262" />

Unless im misunderstanding this issue? Thanks. 

---

_Comment by @ericgumba on 2025-07-16 08:52_

@epage Actually nevermind, I took a look at 

https://github.com/clap-rs/clap/pull/3373

and I think I get it now, clap_man should take into consideration the function `display_order` when organizing the man page. 

---

_Referenced in [clap-rs/clap#6072](../../clap-rs/clap/pulls/6072.md) on 2025-07-19 07:09_

---

_Referenced in [clap-rs/clap#6141](../../clap-rs/clap/pulls/6141.md) on 2025-09-26 14:05_

---

_Referenced in [clap-rs/clap#6142](../../clap-rs/clap/pulls/6142.md) on 2025-09-26 22:19_

---

_Closed by @epage on 2025-10-22 15:14_

---

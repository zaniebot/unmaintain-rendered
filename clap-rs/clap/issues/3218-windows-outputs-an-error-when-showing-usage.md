---
number: 3218
title: Windows outputs an error when showing usage
type: issue
state: closed
author: sigmaSd
labels:
  - C-bug
  - A-validators
  - S-triage
assignees: []
created_at: 2021-12-25T16:00:55Z
updated_at: 2021-12-27T20:37:24Z
url: https://github.com/clap-rs/clap/issues/3218
synced_at: 2026-01-10T01:27:36Z
---

# Windows outputs an error when showing usage

---

_Issue opened by @sigmaSd on 2021-12-25 16:00_

Currently when clap shows the usage it exit with exit_code=2

The problem is windows outputs an error  after it detects that the application exited with an error

```error: process didn't exit successfully: `` (exit code:2)```

This makes applications using clap on windows weird,  just by simply running the application to see its arguments, you'll always encounter an error.

On linux the shells don't complain, so this is windows specific (windows 10, same behavior on cmd/powershell/windows terminal)

According to the code, the usage-code is taken from python argparse, but I tested some (contrived) python examples and it doesn't seem to do that

meta:
clap: 3.0.0-beta.5

---

_Comment by @epage on 2021-12-25 16:47_

What shell and OS version?

---

_Comment by @epage on 2021-12-25 16:47_

Also, could you provide example code and command line to make sure we reproduce it the same way?

---

_Label `A-validators` added by @epage on 2021-12-25 16:47_

---

_Label `S-triage` added by @epage on 2021-12-25 16:47_

---

_Label `C-bug` added by @epage on 2021-12-25 16:47_

---

_Comment by @sigmaSd on 2021-12-25 17:09_

```rust
use clap::Parser;

#[derive(Parser)]
pub struct Opts {
 #[clap(subcommand)]
 pub cmd: Subcommand
}

#[derive(Parser)] 
pub enum Subcommand {}

fn main() {
 let _ = Opts::parse();
}
```
`cargo run #no arguments`

windows 10 20h2
powershell  5.1.19041.1320

---

_Comment by @epage on 2021-12-26 01:38_

Alright, won't be until at least Monday before I can take a look.

Bleh, having to touch powershell :/
(I know a lot of people like it, just never really clicked for me as something useful outside of automation)

---

_Comment by @sigmaSd on 2021-12-26 06:57_

I dont use windows but just happened to test a program on it and I noticed this

But I must say the dev experience is  better, I replecated my vim setup and the windows terminal looks good

But the only thing I missed is fish shell xD

---

_Comment by @epage on 2021-12-27 14:39_

Do you only see this when running `cargo run`?

I reproduced it with `cmd`.  It looks like a cargo message, so I ran the binary directly and I do not see it.  I then ran `cargo run -q` and it goes away.

Digging into cargo's code:
1. `cargo run` calls [`commands::runs::exec`](https://github.com/rust-lang/cargo/blob/9a3c162dc3b912cde1a0f436958adb2cb8f21e5e/src/bin/cargo/commands/run.rs#L85)  which calls ...
2. [`ops::runs`](https://github.com/rust-lang/cargo/blob/9a3c162dc3b912cde1a0f436958adb2cb8f21e5e/src/cargo/ops/cargo_run.rs#L100) which calls ...
3. [ProcessBuilder::exec_replace](https://github.com/rust-lang/cargo/blob/9a3c162dc3b912cde1a0f436958adb2cb8f21e5e/crates/cargo-util/src/process_builder.rs#L198) which calls a platform-specific implementation.  Only the Windows version calls
4. [ProcessBuilder::exec](https://github.com/rust-lang/cargo/blob/9a3c162dc3b912cde1a0f436958adb2cb8f21e5e/crates/cargo-util/src/process_builder.rs#L165) which looks to print the relevant message

So this seems to be Windows-specific cargo behavior.

At least in my quick searches of cargo's issues, I didn't find any about this being inconsistent between platforms.

---

_Comment by @sigmaSd on 2021-12-27 20:23_

You're right I didn't notice it, should I close this issue since its an upstream problem?

---

_Comment by @epage on 2021-12-27 20:37_

I'll go ahead.  I was waiting in case there was something I missed.

---

_Closed by @epage on 2021-12-27 20:37_

---

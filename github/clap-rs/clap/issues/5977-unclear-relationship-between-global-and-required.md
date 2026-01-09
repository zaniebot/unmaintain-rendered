---
number: 5977
title: "Unclear relationship between `global` and `required`"
type: issue
state: open
author: mlegner
labels:
  - C-bug
  - A-parsing
  - A-validators
assignees: []
created_at: 2025-04-24T14:09:26Z
updated_at: 2025-10-24T15:10:09Z
url: https://github.com/clap-rs/clap/issues/5977
synced_at: 2026-01-07T13:12:20-06:00
---

# Unclear relationship between `global` and `required`

---

_Issue opened by @mlegner on 2025-04-24 14:09_

It is unclear whether an argument can be marked as `global` and `required` simultaneously. There used to be a note "Global arguments *cannot* be required." in the docs, which was removed in #1075. There still remains a [debug assertion](https://github.com/clap-rs/clap/blob/183cfe3b011f2f1385a996ee9606289c69189550/clap_builder/src/builder/debug_asserts.rs#L255-L260) forbidding the combination.

If an argument is in fact both `global` and `required`, it seems that it is expected to be provided at *all* levels. Consider the following minimal example:

```rust
use clap::Parser;

#[derive(Parser)]
struct Cli {
    #[arg(long, global = true)]
    name: String,
    #[command(subcommand)]
    subcommand: Subcommand,
}

#[derive(Parser, Debug)]
enum Subcommand {
    Print,
}

fn main() {
    let cli = Cli::parse();
    println!("Name: {}", cli.name);
}
```

When running the debug build, this triggers the debug assertion:

```console
$ cargo run -- print                        
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/test-clap print`

thread 'main' panicked at ~/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/clap_builder-4.5.37/src/builder/debug_asserts.rs:255:9:
Command test-clap: Global arguments cannot be required.

        'name' is marked as both global and required
```

When running the release build, it requires the `name` at every level:

```console
$ cargo run --release -- print --name test
    Finished `release` profile [optimized] target(s) in 0.06s
     Running `target/release/test-clap print --name test`
error: the following required arguments were not provided:
  --name <NAME>

Usage: test-clap --name <NAME> <COMMAND>

For more information, try '--help'.

$ cargo run --release -- --name test print
    Finished `release` profile [optimized] target(s) in 0.02s
     Running `target/release/test-clap --name test print`
error: the following required arguments were not provided:
  --name <NAME>

Usage: test-clap --name <NAME> print --name <NAME>

For more information, try '--help'.

$ cargo run --release -- --name test print --name test
    Finished `release` profile [optimized] target(s) in 0.01s
     Running `target/release/test-clap --name test print --name test`
Name: test
```

---

_Referenced in [MystenLabs/walrus#2003](../../MystenLabs/walrus/pulls/2003.md) on 2025-04-24 14:13_

---

_Label `A-parsing` added by @epage on 2025-04-24 14:13_

---

_Label `A-validators` added by @epage on 2025-04-24 14:16_

---

_Label `C-bug` added by @epage on 2025-04-24 14:16_

---

_Comment by @epage on 2025-04-24 14:16_

Related issues:
- #5020
- #1546
- #1204
- #6049
- #6160

---

_Referenced in [clap-rs/clap#6026](../../clap-rs/clap/issues/6026.md) on 2025-06-09 14:22_

---

_Referenced in [clap-rs/clap#6049](../../clap-rs/clap/issues/6049.md) on 2025-06-27 15:16_

---

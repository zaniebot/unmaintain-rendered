```yaml
number: 3282
title: "`rename_all` on a `Subcommand` applies to variant-struct fields"
type: issue
state: closed
author: epage
labels:
  - C-bug
  - M-breaking-change
  - E-medium
  - A-derive
assignees: []
created_at: 2022-01-11T20:49:41Z
updated_at: 2022-05-09T15:48:36Z
url: https://github.com/clap-rs/clap/issues/3282
synced_at: 2026-01-12T16:14:14Z
```

# `rename_all` on a `Subcommand` applies to variant-struct fields

---

_@epage_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.57.0 (f1edd0429 2021-11-29)

### Clap Version

v3.0.6

### Minimal reproducible code

```rust
use clap::{Parser, Subcommand};

#[derive(Parser, Debug)]
#[clap(author, version, about, long_about = None)]
struct Cli {
    #[clap(long)]
    out_dir: Option<String>,

    #[clap(subcommand)]
    command: Commands,
}

#[derive(Subcommand, Debug)]
#[clap(rename_all = "snake_case")]
enum Commands {
    /// Adds files to myapp
    Add {
        #[clap(long)]
        out_dir: Option<String>,
    },
}

fn main() {
    let cli = Cli::parse();
    dbg!(cli);
}
```


### Steps to reproduce the bug with the above code

```console
$ cargo run -- add --help
```

### Actual Behaviour

```
test-clap-add 
Adds files to myapp

USAGE:
    test-clap add [OPTIONS]

OPTIONS:
    -h, --help                 Print help information
        --out_dir <OUT_DIR>    
```

### Expected Behaviour

```
test-clap-add 
Adds files to myapp

USAGE:
    test-clap add [OPTIONS]

OPTIONS:
    -h, --help                 Print help information
        --out-dir <OUT_DIR>    
```

### Additional Context

Found via https://github.com/rust-lang/rustc-perf/pull/1142

### Debug Output

_No response_

---

_Label `C-bug` added by @epage on 2022-01-11 20:49_

---

_Label `E-medium` added by @epage on 2022-01-11 20:49_

---

_Label `A-derive` added by @epage on 2022-01-11 20:49_

---

_Referenced in [rust-lang/rustc-perf#1142](../../rust-lang/rustc-perf/pulls/1142.md) on 2022-01-11 20:49_

---

_Label `M-breaking-change` added by @epage on 2022-02-09 00:07_

---

_Added to milestone `4.0` by @epage on 2022-02-09 00:08_

---

_Comment by @epage on 2022-05-06 21:54_

Since clap specifically tests for this behavior, I got worried about what precedent is set for this so I tested serde
```rust
#!/usr/bin/env rust-script

//! ```cargo
//! [dependencies]
//! serde = { version = "1", features = ["derive"] }
//! serde_json = "1"
//! ```

#[derive(serde::Serialize, serde::Deserialize, Debug)]
#[serde(rename_all = "SCREAMING_SNAKE_CASE")]
enum Args {
    One,
    Two { field: String },
}

fn main() {
    let args = Args::One;
    let json = serde_json::to_string(&args).unwrap();
    println!("{}", json);
    let args = Args::Two {
        field: "value".into(),
    };
    let json = serde_json::to_string(&args).unwrap();
    println!("{}", json);
}
```
```console
$ ./clap-3282.rs
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `/home/epage/src/personal/cargo-script-mvs/target/debug/rust-script ./clap-3282.rs`
"ONE"
{"TWO":{"field":"value"}}
```
Looks like serde does not apply `rename_all` recursively which makes sense because variants and fields are very different and can have different naming needs.

---

_Comment by @epage on 2022-05-08 02:05_

Thinking about it more, I think  with it being an `id`, it is fine being skipped by `rename_all`, especially since we set `value_name`.  However, I could see us providing more specialized renames, `rename_flags`, `rename_commands`, `rename_values`

---

_Referenced in [clap-rs/clap#3708](../../clap-rs/clap/pulls/3708.md) on 2022-05-09 15:37_

---

_Referenced in [clap-rs/clap#3709](../../clap-rs/clap/issues/3709.md) on 2022-05-09 15:43_

---

_Closed by @epage on 2022-05-09 15:48_

---

_Referenced in [clap-rs/clap#4759](../../clap-rs/clap/pulls/4759.md) on 2023-03-13 17:25_

---

_Referenced in [clap-rs/clap#4869](../../clap-rs/clap/issues/4869.md) on 2023-04-28 20:26_

---

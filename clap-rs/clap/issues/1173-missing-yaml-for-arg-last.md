```yaml
number: 1173
title: "Missing YAML for Arg::last"
type: issue
state: closed
author: AndrewGaspar
labels: []
assignees: []
created_at: 2018-02-11T19:10:17Z
updated_at: 2018-08-02T03:30:18Z
url: https://github.com/clap-rs/clap/issues/1173
synced_at: 2026-01-12T16:14:10Z
```

# Missing YAML for Arg::last

---

_@AndrewGaspar_

### Rust Version
`rustc 1.23.0 (766bd11c8 2018-01-01)`
### Affected Version of clap
```
andre@ANDREW-DESKTOP:/mnt/c/Users/andre/Projects/cargo-mpirun$ grep clap Cargo.lock
 "clap 2.29.4 (registry+https://github.com/rust-lang/crates.io-index)",
name = "clap"
"checksum clap 2.29.4 (registry+https://github.com/rust-lang/crates.io-index)" = "7b8f59bcebcfe4269b09f71dab0da15b355c75916a8f975d3876ce81561893ee"
```

### Expected Behavior Summary
The YAML should allow you to specify an arg with `last: true`, marking it as the `last` argument
### Actual Behavior Summary
YAML fails to parse with this error:
```
thread 'main' panicked at 'Unknown Arg setting 'last' in YAML file for arg 'args'', /home/andre/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.29.4/src/args/arg.rs:147:21note: Run with `RUST_BACKTRACE=1` for a backtrace.
```
### Steps to Reproduce the issue
Create a new clap App from a YAML file.

Example:

cli.yml
```yaml
name: example
version: "0.1"
author: Foo B. <foo@bar.com>
about: Fails to compile YAML
args:
    - args:
        last: true
        multiple: true
        allow_hyphen_values: true
```

main.rs
```rust
#[macro_use]
extern crate clap;
use clap::App;

fn main() {
    let cli = load_yaml!("cli.yml");
    let _matches = App::from_yaml(cli).get_matches();
}
```
### Debug output
```
andre@ANDREW-DESKTOP:/mnt/c/Users/andre/Projects/cargo-mpirun$ cargo run -- --help
   Compiling clap v2.29.4
   Compiling cargo-mpirun v0.1.0 (file:///mnt/c/Users/andre/Projects/cargo-mpirun)
    Finished dev [unoptimized + debuginfo] target(s) in 10.57 secs
     Running `target/debug/cargo-mpirun --help`
thread 'main' panicked at 'Unknown Arg setting 'last' in YAML file for arg 'args'', /home/andre/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.29.4/src/args/arg.rs:147:21
note: Run with `RUST_BACKTRACE=1` for a backtrace.
```

---

_Comment by @kbknapp on 2018-02-11 19:19_

Ah, thanks! I'm not at a computer right now, but this will be a quick fix once I can get to one.

---

_Referenced in [clap-rs/clap#1175](../../clap-rs/clap/pulls/1175.md) on 2018-02-12 05:21_

---

_Closed by @kbknapp on 2018-02-12 19:14_

---

_Comment by @AndrewGaspar on 2018-02-12 19:16_

@kbknapp Will this fix go into v2.*?

---

_Comment by @kbknapp on 2018-02-12 19:54_

Yes, I'll put out v2.29.5 shortly which will include it 😉 

---

_Comment by @AndrewGaspar on 2018-02-12 20:07_

Awesome, thank you!

---

_Comment by @kbknapp on 2018-02-13 18:09_

@AndrewGaspar this fix was published under v2.30.0 which is on crates.io now

---

_Comment by @AndrewGaspar on 2018-02-13 18:25_

Excellent, thank you!

---

_Referenced in [AndrewGaspar/cargo-mpirun#1](../../AndrewGaspar/cargo-mpirun/issues/1.md) on 2018-02-13 18:28_

---

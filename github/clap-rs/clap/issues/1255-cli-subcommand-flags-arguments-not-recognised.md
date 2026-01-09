---
number: 1255
title: CLI subcommand flags arguments not recognised after switching from NON-YAML approach to using the YAML file approach 
type: issue
state: closed
author: ltfschoen
labels: []
assignees: []
created_at: 2018-04-19T10:41:03Z
updated_at: 2018-08-02T03:30:22Z
url: https://github.com/clap-rs/clap/issues/1255
synced_at: 2026-01-07T13:12:19-06:00
---

# CLI subcommand flags arguments not recognised after switching from NON-YAML approach to using the YAML file approach 

---

_Issue opened by @ltfschoen on 2018-04-19 10:41_

### Rust Version

* Use the output of `rustc -V`
```
$ rustc -V
rustc 1.27.0-nightly (ad610bed8 2018-04-11)
```

### Affected Version of clap

* Can be found in Cargo.lock of your project (i.e. `grep clap Cargo.lock`)
```
$ grep clap Cargo.lock
name = "clap"
 "clap 2.31.2 (registry+https://github.com/rust-lang/crates.io-index)",
"checksum clap 2.31.2 (registry+https://github.com/rust-lang/crates.io-index)" = "f0f16b89cbb9ee36d87483dc939fe9f1e13c05898d56d7b230a0d4dff033a536"
```

### Expected Behavior Summary

I had originally implemented the **first method from the documentation link that offered the advanced configuration options** by following the documentation here https://docs.rs/clap/2.31.2/clap/ and my code ran successfully:

**lib.rs**
```rust
extern crate clap;
#[macro_use] extern crate log;
extern crate env_logger;

use clap::{Arg, App, SubCommand};

pub fn main() {
	let matches = App::new("Polkadot-clone")
		.version("0.1")
		.author("Luke S. <ltfschoen@gmail.com>")
		.about("Implementation Polkadot node in Rust")
		.arg(Arg::with_name("log")
			.short("l")
			.value_name("LOG")
			.help("Sets logging.")
			.required(false)
			.takes_value(true))
		.subcommand(SubCommand::with_name("collator"))
			.arg(Arg::with_name("c")
				.short("c")
				.help("collator in debug mode"))
		.subcommand(SubCommand::with_name("validator"))
			.arg(Arg::with_name("v")
				.short("v")
				.help("validator in debug mode"))
		.get_matches();
```

When I ran my code with any of the following commands it worked successfully:

    ```bash
    cargo run -- --help
    
    RUST_LOG=trace RUST_LOG_STYLE=auto \
    cargo run -- -c -l "default.conf" collator
    
    RUST_LOG=error RUST_LOG_STYLE=auto \
    cargo run -- -c -l "default.conf" validator
    ```

And it output the following when I ran `cargo run -- --help`. **NOTE IMPORTANTLY** how the `-c` and `-v` flags are shown listed

```
$ RUST_BACKTRACE=1 cargo run -- --help
   Compiling polkadot-cli v0.1.0 (file:///Users/Me/code/blockchain/polkadot-clone/polkadot-clone/cli)
   Compiling polkadot-clone v0.1.0 (file:///Users/Me/code/blockchain/polkadot-clone/polkadot-clone)
    Finished dev [unoptimized + debuginfo] target(s) in 9.53 secs
     Running `target/debug/polkadot-clone --help`
Welcome to Polkadot Clone
Polkadot-clone 0.1
Luke S. <ltfschoen@gmail.com>
Implementation Polkadot node in Rust

USAGE:
    polkadot-clone [FLAGS] [OPTIONS] [SUBCOMMAND]

FLAGS:
    -c               collator in debug mode
    -h, --help       Prints help information
    -v               validator in debug mode
    -V, --version    Prints version information

OPTIONS:
    -l <LOG>        Sets logging.

SUBCOMMANDS:
    collator     
    help         Prints this message or the help of the given subcommand(s)
    validator  
```

And additionally it outputs the following when I run `cargo run -- -c -l "default.conf" collator`. **NOTE IMPORTANTLY** how I'm using the `-c` flag and it's working without any errors:

```
$ cargo run -- -c -l "default.conf" collator
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target/debug/polkadot-clone -c -l default.conf collator`
Welcome to Polkadot Clone
ERROR 2018-04-19T10:27:38Z: polkadot_cli: Example Error message
 WARN 2018-04-19T10:27:38Z: polkadot_cli: Example Error message
 INFO 2018-04-19T10:27:38Z: polkadot_cli: Example Info message
Value for config log pattern: default.conf
Running as collator in debug mode...
```

But then I decided to refactor my code to use the **third method from the documentation of using a YAML file to build the CLI**

**lib.rs**
```rust
extern crate env_logger;

#[macro_use]
extern crate clap;
#[macro_use]
extern crate log;

pub fn main() {
	let yaml = load_yaml!("cli.yml");
	let matches = clap::App::from_yaml(yaml).get_matches();
...
```

**cli.yml**
```yaml
name: polkadot-clone
version: "0.1"
author: Luke S. <ltfschoen@gmail.com>
about: Implementation of Polkadot node in Rust
args:
  - log:
      short: l
      value_name: LOG
      help: Sets custom logging
      required: false
      takes_value: true
subcommands:
  - collator:
      about: Run collator node
      args:
        - c:
            short: c
            help: collator in debug mode
  - validator:
      about: Run validator node
      args:
        - v:
            short: v
            help: validator in debug mode
```

When I ran my code with the following commands that I used previously, it doesn't give me the same outputs:

    ```bash
    cargo run -- --help
    
    RUST_LOG=trace RUST_LOG_STYLE=auto \
    cargo run -- -c -l "default.conf" collator
    
    RUST_LOG=error RUST_LOG_STYLE=auto \
    cargo run -- -c -l "default.conf" validator
    ```

And it output the following when I ran `cargo run -- --help`. **NOTE IMPORTANTLY** how the `-c` and `-v` flags **MISSING** whereas before they were shown

```
$ RUST_BACKTRACE=1 cargo run -- --help
   Compiling polkadot-cli v0.1.0 (file:///Users/Me/code/blockchain/polkadot-clone/polkadot-clone/cli)
   Compiling polkadot-clone v0.1.0 (file:///Users/Me/code/blockchain/polkadot-clone/polkadot-clone)
    Finished dev [unoptimized + debuginfo] target(s) in 9.53 secs
     Running `target/debug/polkadot-clone --help`
Welcome to Polkadot Clone
Polkadot-clone 0.1
Luke S. <ltfschoen@gmail.com>
Implementation Polkadot node in Rust

USAGE:
    polkadot-clone [OPTIONS] [SUBCOMMAND]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

OPTIONS:
    -l <LOG>        Sets custom logging

SUBCOMMANDS:
    collator     Run collator node
    help         Prints this message or the help of the given subcommand(s)
    validator    Run validator node
```

And  it outputs the following when I run `cargo run -- -c -l "default.conf" collator`. **NOTE IMPORTANTLY** how because the `-c` option isn't appearing in the help menu it's failing, whereas before without using the .yaml file it worked:

```
$ cargo run -- -c -l "default.conf" collator

error: Found argument '-c' which wasn't expected, or isn't valid in this context

USAGE:
    polkadot-clone [OPTIONS] [SUBCOMMAND]
```

I expected it to work the same way with both the non-yaml file approach as with the yaml file approach.

### Actual Behavior Summary

I've mentioned the actual  behaviour in the expected behaviour section above

### Steps to Reproduce the issue

* Clone my repo https://github.com/ltfschoen/polkadot-clone
* View the NON-YAML version in the "master" branch
* Switch to the "clap-yaml" branch where I've uploaded the version where I tried to replicate the same outputs using the YML file https://github.com/ltfschoen/polkadot-clone/compare/clap-yaml?expand=1

### Sample Code or Link to Sample Code

* Link to code that I was using before switching to using the approach with the cli.yml file https://github.com/ltfschoen/polkadot-clone/blob/master/cli/src/lib.rs

### Debug output

Compile clap with cargo features `"debug"` such as:

```toml
[dependencies]
clap = { version = "2.31.2", features = ["yaml", "debug"] } 
```

```
$ cargo build
   Compiling clap v2.31.2
   Compiling polkadot-cli v0.1.0 (file:///Users/Me/code/blockchain/polkadot-clone/polkadot-clone/cli)
   Compiling polkadot-clone v0.1.0 (file:///Users/Me/code/blockchain/polkadot-clone/polkadot-clone)
    Finished dev [unoptimized + debuginfo] target(s) in 18.17 secs
```


---

_Comment by @kbknapp on 2018-06-05 02:26_

Sorry for taking so long, I've been away with my day job for a few weeks.

In your original code, there is a mistake which probably led to this confusion. But let's check it out to make sure.

This is what you had:

```rust
.subcommand(SubCommand::with_name("collator"))  // <-- Error! Extra ")" mean subcommand is done
	.arg(Arg::with_name("c")
		.short("c")
		.help("collator in debug mode"))
```

How clap interprets the above:

```rust
.subcommand(SubCommand::with_name("collator"))
.arg(Arg::with_name("c")
	.short("c")
	.help("collator in debug mode"))
```

I.e. argument `c` is actually part of the parent command and *not* part of the subcommand.

The correct way to make `c` a part of the subcommand is like this:

```rust
.subcommand(SubCommand::with_name("collator")  // <--- Notice no ")"
			.arg(Arg::with_name("c")
				.short("c")
				.help("collator in debug mode")))  // <--- Notice extra ")" closing off the subcommand
```

I'm going to close this, but please let me know if I'm totally missing the issue!

---

_Closed by @kbknapp on 2018-06-05 02:26_

---

_Comment by @kbknapp on 2018-06-05 02:27_

To be more clear, in the YAML version you did it correct and the `c` arg was defined as part of the subcommand and could have been seen by `cargo run -- collator --help` or used with `cargo run -- collator -c`

---

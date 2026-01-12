```yaml
number: 3129
title: "Subcommand hijacks parent's about"
type: issue
state: closed
author: epage
labels:
  - C-bug
  - A-derive
assignees: []
created_at: 2021-12-09T16:33:59Z
updated_at: 2021-12-10T20:04:48Z
url: https://github.com/clap-rs/clap/issues/3129
synced_at: 2026-01-12T16:14:14Z
```

# Subcommand hijacks parent's about

---

_@epage_

<a href="https://github.com/CodeSandwich"><img src="https://avatars.githubusercontent.com/u/26183680?v=4" align="left" width="96" height="96" hspace="10"></img></a> **Issue by [CodeSandwich](https://github.com/CodeSandwich)**
_Thursday May 14, 2020 at 09:35 GMT_
_Originally opened as https://github.com/TeXitoi/structopt/issues/391_

----

I have a simple program:
```rust
use structopt::StructOpt;

/// The main thing
#[derive(StructOpt)]
struct Main {
    #[structopt(subcommand)]
    subcommand: Subcommand,
}

/// The subcommand
#[derive(StructOpt)]
enum Subcommand {
    A{}
}

fn main() {
    Main::from_args();
}
```
When I run it with `cargo run -- --help`, I get the following output:

```
poligon 0.1.0
The subcommand

USAGE:
    poligon <SUBCOMMAND>

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

SUBCOMMANDS:
    a       
    help    Prints this message or the help of the given subcommand(s)
```
The app description should be `The main thing`, not `The subcommand`!


---

_Comment by @epage on 2021-12-09 16:34_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Friday May 15, 2020 at 06:55 GMT_

----

Thanks for the clear report.


---

_Comment by @epage on 2021-12-09 16:34_

<a href="https://github.com/pizzamig"><img src="https://avatars.githubusercontent.com/u/57148?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [pizzamig](https://github.com/pizzamig)**
_Monday Jul 20, 2020 at 21:41 GMT_

----

This also prevents flattening of struct with their own rustdoc:
```rust
use structopt::StructOpt;

#[derive(Debug, StructOpt)]
#[structopt(name = "structopt-bug", about = "Show a bug in Structopt")]
struct Opt {
    #[structopt(flatten)]
    v: Verbose,
}

/// Re-usable Verbose flag
#[derive(Debug, StructOpt)]
struct Verbose {
    /// Enable the verbosity messages
    #[structopt(short)]
    verbose: bool,
}
fn main() {
    let opt = Opt::from_args();
    println!("{:?}", opt);
}
```

The `cargo run -- -h` shows:
```
structopt-bug 0.1.0
Re-usable Verbose flag

USAGE:
    structopt-bug [FLAGS]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information
    -v               Enable the verbosity messages
```



---

_Label `A-derive` added by @epage on 2021-12-09 16:36_

---

_Label `C-bug` added by @epage on 2021-12-09 16:36_

---

_Comment by @epage on 2021-12-09 16:36_

Is this a dup of #3127 ?  Is it already fixed?

---

_Comment by @pksunkara on 2021-12-10 01:42_

Yeah, that's what I was wondering. Didn't you already fix this and #3127?

---

_Comment by @epage on 2021-12-10 20:04_

Confirmed, this is fixed
```rust
use clap::StructOpt;

/// The main thing
#[derive(StructOpt)]
struct Main {
    #[structopt(subcommand)]
    subcommand: Subcommand,
}

/// The subcommand
#[derive(StructOpt)]
enum Subcommand {
    A {},
}

fn main() {
    Main::from_args();
}
```
```
test-clap 

The main thing

USAGE:
    test-clap <SUBCOMMAND>

OPTIONS:
    -h, --help    Print help information

SUBCOMMANDS:
    a       
    help    Print this message or the help of the given subcommand(s)
```

---

_Closed by @epage on 2021-12-10 20:04_

---

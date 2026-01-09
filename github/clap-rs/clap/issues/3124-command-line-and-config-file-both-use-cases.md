---
number: 3124
title: Command-line and config file, both use-cases become possible with structopt?
type: issue
state: closed
author: epage
labels: []
assignees: []
created_at: 2021-12-09T16:16:18Z
updated_at: 2021-12-09T16:29:06Z
url: https://github.com/clap-rs/clap/issues/3124
synced_at: 2026-01-07T13:12:19-06:00
---

# Command-line and config file, both use-cases become possible with structopt?

---

_Issue opened by @epage on 2021-12-09 16:16_

<a href="https://github.com/SamuelMarks"><img src="https://avatars.githubusercontent.com/u/807580?v=4" align="left" width="96" height="96" hspace="10"></img></a> **Issue by [SamuelMarks](https://github.com/SamuelMarks)**
_Wednesday Jun 05, 2019 at 04:46 GMT_
_Originally opened as https://github.com/TeXitoi/structopt/issues/197_

----

A few common use-cases:
- Accept input from config file
- Accept input from environment variables
- Accept input from command-line

Then, overriding config file attributes with environment variable or command line.

I'm not sure exactly how to represent this in Rust, but something like:

```rust
#[derive(StructOpt, Debug)]
#[structopt(name = "example", about = "An example of StructOpt usage.")]
struct Opt {
    /// The config file
    #[structopt(short = "c", long = "config", help = "Config file")]
    config_file: Option<Config>,

    #[structopt(flatten, required_unless = "config_file")
    options: Option<Config>,
}

#[derive(StructOpt, Debug)]
struct Config {
    /// A flag, true if used in the command line.
    #[structopt(short = "d", long = "debug", help = "Activate debug mode")]
    debug: bool,

    /// An argument of type float, with a default value.
    #[structopt(short = "s", long = "speed", help = "Set speed", default_value = "42")]
    speed: f64,

    /// Needed parameter, the first on the command line.
    #[structopt(help = "Input file")]
    input: String,

    /// An optional parameter, will be `None` if not present on the
    /// command line.
    #[structopt(help = "Output file, stdout if not present")]
    output: Option<String>,

    /// An optional parameter with optional value, will be `None` if
    /// not present on the command line, will be `Some(None)` if no
    /// argument is provided (i.e. `--log`) and will be
    /// `Some(Some(String))` if argument is provided (e.g. `--log
    /// log.txt`).
    #[structopt(
        long = "log",
        help = "Log file, stdout if no file, no logging if not present"
    )]
    log: Option<Option<String>>,

    /// An optional list of values, will be `None` if not present on
    /// the command line, will be `Some(vec![])` if no argument is
    /// provided (i.e. `--optv`) and will be `Some(Some(String))` if
    /// argument list is provided (e.g. `--optv a b c`).
    #[structopt(long = "optv")]
    optv: Option<Vec<String>>,
}
```


---

_Comment by @epage on 2021-12-09 16:16_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Wednesday Jun 05, 2019 at 09:15 GMT_

----

See also #72


---

_Comment by @epage on 2021-12-09 16:16_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Wednesday Jun 05, 2019 at 09:41 GMT_

----

Note that clap support environment variable. You can use that:
```rust
#[derive(StructOpt, Debug)]
struct Opt {
    #[structopt(short, long, env = "FILE", default_value = "foo")]
    file: String,
}
```
if `-f` or `--file` is given on the command line, it is used, else if the environment variable `FILE` is setted, it is used, else the default value "foo" is used.


---

_Comment by @epage on 2021-12-09 16:29_

I'm closing this in favor of #3113 to track config layering

---

_Closed by @epage on 2021-12-09 16:29_

---

_Referenced in [TeXitoi/structopt#197](../../TeXitoi/structopt/issues/197.md) on 2022-01-18 15:25_

---

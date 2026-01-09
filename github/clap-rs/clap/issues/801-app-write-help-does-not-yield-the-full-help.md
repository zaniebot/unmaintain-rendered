---
number: 801
title: "App::write_help does not yield the full help message"
type: issue
state: closed
author: phrohdoh
labels:
  - C-bug
  - M-breaking-change
  - A-help
assignees: []
created_at: 2017-01-02T19:38:44Z
updated_at: 2018-08-02T03:29:59Z
url: https://github.com/clap-rs/clap/issues/801
synced_at: 2026-01-07T13:12:19-06:00
---

# App::write_help does not yield the full help message

---

_Issue opened by @phrohdoh on 2017-01-02 19:38_

### Rust Version

* `rustc 1.16.0-nightly (4ecc85beb 2016-12-28)`

### Affected Version of clap

* `2.19.3`

### Expected Behavior Summary

Outputs should be exactly the same.

### Actual Behavior Summary

FLAGS section is missing.
`help` subcommand is missing.

### Steps to Reproduce the issue

```
tmba:sentinel-cli thill $ ./target/debug/sentinel-cli # uses results of `App::write_help`
sentinel-cli 0.1.0
Taryn Hill <taryn@phrohdoh.com>
CLI for Sentinel -- git hooks that guard you from yourself

USAGE:
    sentinel-cli [SUBCOMMAND]

SUBCOMMANDS:
    init    Initialize a new Sentinel project in the given directory
```
```
tmba:sentinel-cli thill $ ./target/debug/sentinel-cli --help # displays generated help string
sentinel-cli 0.1.0
Taryn Hill <taryn@phrohdoh.com>
CLI for Sentinel -- git hooks that guard you from yourself

USAGE:
    sentinel-cli [SUBCOMMAND]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

SUBCOMMANDS:
    help    Prints this message or the help of the given subcommand(s)
    init    Initialize a new Sentinel project in the given directory
```

### Sample Code

```rust
    let mut help_bytes: Vec<u8> = Vec::new();
    let app = App::new("sentinel-cli")
        .version("0.1.0")
        .author("Taryn Hill <taryn@phrohdoh.com>")
        .about("CLI for Sentinel -- git hooks that guard you from yourself")
        .subcommand(SubCommand::with_name("init")
            .about("Initialize a new Sentinel project in the given directory")
            .version("0.1.0")
            .author("Taryn Hill <taryn@phrohdoh.com>")
            .arg(Arg::with_name("PATH")
                .value_name("PATH")
                .index(1)));
    app.write_help(&mut help_bytes).expect("Failed to capture help message");
    let matches = app.get_matches();

    let res: Result<(), SentinelCliError> = match matches.subcommand() {
        ("init", Some(args)) => init_project(args),
        (cmd, _) if cmd.is_empty() => {
            let help_string = String::from_utf8(help_bytes).expect("Help message was invalid UTF8");
            println!("{}", help_string);
            Ok(())
        },
        (cmd, _) => Err(SentinelCliError::UnrecognizedCommand(cmd.into())),
    };
```

---

_Comment by @kbknapp on 2017-01-02 19:54_

Thanks for taking the time to file this! This is something I've been aware of and was originally somewhat by design as `write_help` was originally private. `clap` generates `--help` and `--version` lazily and not until one of the `get_matches_*` methods is called to prevent something like:

```rust
let app = App::new("foo");
app.write_help(&mut help).unwrap(); // if --help and --version are generated here
let m = app
    .arg(Arg::with_name("help")
        .long("--help")) // error here for two args having the long `--help`
    .get_matches();
```

I.e. modifying the help message after writing it. Although I do think this is a super niche case.

Since you're not the first person to note this, I think it may be time I just change that and cause `--help` and `--version` to be generated earlier if required.

----

If you're trying to print the help message when no subcommand is received use [`AppSettings::SubcomamndRequiredElseHelp`](https://docs.rs/clap/2.19.3/clap/enum.AppSettings.html#variant.SubcommandRequiredElseHelp) :wink:

edit: fixed link

---

_Label `T: RFC / question` added by @kbknapp on 2017-01-02 19:55_

---

_Label `C: help message` added by @kbknapp on 2017-01-02 21:46_

---

_Label `D: intermediate` added by @kbknapp on 2017-01-02 21:46_

---

_Label `P4: nice to have` added by @kbknapp on 2017-01-02 21:46_

---

_Label `T: enhancement` added by @kbknapp on 2017-01-02 21:46_

---

_Label `W: 2.x` added by @kbknapp on 2017-01-02 21:46_

---

_Added to milestone `2.20.0` by @kbknapp on 2017-01-02 21:46_

---

_Label `T: bug` added by @kbknapp on 2017-01-02 21:52_

---

_Label `T: enhancement` removed by @kbknapp on 2017-01-02 21:52_

---

_Label `T: RFC / question` removed by @kbknapp on 2017-01-02 21:52_

---

_Closed by @homu on 2017-01-03 06:56_

---

_Referenced in [clap-rs/clap#808](../../clap-rs/clap/issues/808.md) on 2017-01-05 16:26_

---

_Comment by @kbknapp on 2017-01-05 16:27_

I need to re-open and revert the commit that fixed this until the discussion in #808 is complete. Sadly 2.20.0 won't have this fix, but it's still possible for v2.21.0 :wink:

---

_Reopened by @kbknapp on 2017-01-05 16:27_

---

_Removed from milestone `2.20.0` by @kbknapp on 2017-01-05 16:27_

---

_Label `E: breaking change` added by @kbknapp on 2017-02-12 16:27_

---

_Added to milestone `v3-alpha1` by @kbknapp on 2018-02-02 01:56_

---

_Comment by @kbknapp on 2018-02-02 01:56_

Closed in v3-master

---

_Closed by @kbknapp on 2018-02-02 01:56_

---

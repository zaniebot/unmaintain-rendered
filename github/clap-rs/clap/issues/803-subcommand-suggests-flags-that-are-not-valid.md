---
number: 803
title: SubCommand suggests FLAGS that are not valid
type: issue
state: closed
author: phrohdoh
labels: []
assignees: []
created_at: 2017-01-03T02:26:34Z
updated_at: 2018-08-02T03:29:59Z
url: https://github.com/clap-rs/clap/issues/803
synced_at: 2026-01-07T13:12:19-06:00
---

# SubCommand suggests FLAGS that are not valid

---

_Issue opened by @phrohdoh on 2017-01-03 02:26_

### Rust Version

* `rustc 1.16.0-nightly (4ecc85beb 2016-12-28)`

### Affected Version of clap

* `2.19.3`

### Expected Behavior Summary

FLAGS section should:
1) Not be present if no flags are available
2) Not show flags that are not available for said subcommand

### Actual Behavior Summary

FLAGS section includes `-h/--help` and `-V/--version`.

### Steps to Reproduce the issue

```
tmba:sentinel-cli thill $ ./target/debug/sentinel-cli help init
sentinel-cli-init 0.1.0
Taryn Hill <taryn@phrohdoh.com>
Initialize a new Sentinel project in the given directory

USAGE:
    sentinel-cli init [<path>]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

ARGS:
    <path>    Defaults to the current directory
```

### Sample Code or Link to Sample Code

```rust
    let matches = App::new("sentinel-cli")
        .version("0.1.0")
        .author("Taryn Hill <taryn@phrohdoh.com>")
        .about("CLI for Sentinel -- git hooks that guard you from yourself")
        .setting(AppSettings::SubcommandRequiredElseHelp)
        .subcommand(SubCommand::with_name("init")
            .about("Initialize a new Sentinel project in the given directory")
            .version("0.1.0")
            .author("Taryn Hill <taryn@phrohdoh.com>")
            .arg(Arg::with_name("path")
                .value_name("path")
                .help("Defaults to the current directory")
                .index(1)))
        .get_matches();
```

---

_Comment by @kbknapp on 2017-01-03 03:40_

`--help` and `--version` *are* available for the subcommand though.

```
$ sentinel-cli init --version
sentinel-cli-init 0.1.0
```
This is because some people will develop subcomands as either external applicaitons/plugins (like `cargo`) or wish to have their subcommands have their own versions independant of the parent. If you'd like to develop the entire application (subcommands and all) with a single version number, I'd suggest using [`AppSettings:GlobalVersion`](https://docs.rs/clap/2.19.3/clap/enum.AppSettings.html#variant.GlobalVersion).

Also note the following are identical in terms of functionality:

```
$ sentinel-cli help init
[..snip..]

$ sentinel-cli init -h 
[..snip..]

$ sentinel-cli init --help
[..snip..]
```

One major difference is if `init` had it's own subcommands (for example `foo`), you could also do:

```
$ sentinel-cli help init foo
[ foo's help]

$ sentinel-cli init help
[ init's help]
```

---

_Label `T: RFC / question` added by @kbknapp on 2017-01-03 03:41_

---

_Comment by @kbknapp on 2017-01-03 03:55_

I forgot to mention, if you don't want them available, you simply hide help:

```rust
let app = App::new("sentinel-cli")
    .subcommand(SubCommand::with_name("init")
        .arg(Arg::with_name("help")
            .long("help")
            .hidden(true)));
```

And version can be disabled entirely, either globally (all child subcommands) with [`AppSettings::VersionlessSubcommands`](https://docs.rs/clap/2.19.3/clap/enum.AppSettings.html#variant.VersionlessSubcommands) or just for specific subcommands with [`AppSettings::DisableVersion`](https://docs.rs/clap/2.19.3/clap/enum.AppSettings.html#variant.DisableVersion)

---

_Comment by @phrohdoh on 2017-01-03 17:02_

I swear this did not work last night but it is working just as you described this time around.
Thank you for the detailed response!

---

_Closed by @phrohdoh on 2017-01-03 17:02_

---

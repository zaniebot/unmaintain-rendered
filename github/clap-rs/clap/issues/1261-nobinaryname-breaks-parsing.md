---
number: 1261
title: NoBinaryName breaks parsing
type: issue
state: closed
author: KalitaAlexey
labels: []
assignees: []
created_at: 2018-04-25T09:59:50Z
updated_at: 2018-08-02T03:30:23Z
url: https://github.com/clap-rs/clap/issues/1261
synced_at: 2026-01-07T13:12:19-06:00
---

# NoBinaryName breaks parsing

---

_Issue opened by @KalitaAlexey on 2018-04-25 09:59_

### Rust Version

* rustc 1.27.0-nightly (7360d6dd6 2018-04-15)

### Affected Version of clap

* 2.31.2

### Expected Behavior Summary

It doesn't break parsing and *--help* doesn't display the binary name

### Actual Behavior Summary

It breaks parsing and there is no workaround

### Steps to Reproduce the issue

* *cargo new --bin app*
* Insert the code to *main.rs*

### Sample Code or Link to Sample Code

```rust
extern crate clap;

fn main() {
    let _ = clap::App::new("app")
        .setting(clap::AppSettings::NoBinaryName)
        .setting(clap::AppSettings::SubcommandRequired)
        .subcommand(clap::SubCommand::with_name("subcommand"))
        .get_matches();
}
```

### Debug output

It's doesn't compile if I enable the debug feature.

```
E:\Projects\Rust\clap_test> .\target\debug\clap_test.exe build
error: Found argument 'E:\Projects\Rust\clap_test\target\debug\clap_test.exe' which wasn't expected, or isn't valid in this context

USAGE:
    app <SUBCOMMAND>

For more information try --help
```

---

_Comment by @kbknapp on 2018-06-05 01:30_

Sorry for the long wait, I've been away with my day job for a few weeks.

`NoBinaryName` is meant for things like parsing custom line CLIs or daemon mode processes, it's not meant to be used when you are directly calling the binary program.

i.e. `racer` uses in the the daemon mode, and if one were building a custom interactive prompt it would be useful, but that's about it.

Is this to build a cargo subcommand? If so there is a specific way to do that. I'm going to close this issue, but if it's to build a cargo subcommand either ping me here or in Gitter and I'll tell you how to do that.

---

_Closed by @kbknapp on 2018-06-05 01:30_

---

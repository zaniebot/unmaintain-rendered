```yaml
number: 4285
title: "Argument names must be unique, but 'version' is in use by more than one argument or group"
type: issue
state: closed
author: bonigarcia
labels:
  - C-bug
assignees: []
created_at: 2022-09-29T07:58:04Z
updated_at: 2022-09-29T14:47:42Z
url: https://github.com/clap-rs/clap/issues/4285
synced_at: 2026-01-12T16:14:15Z
```

# Argument names must be unique, but 'version' is in use by more than one argument or group

---

_@bonigarcia_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.63.0 (4b91a6ea7 2022-08-08)

### Clap Version

4.0.2

### Minimal reproducible code

```
use std::error::Error;
use clap::Parser;

/// Selenium-Manager: Automated driver management for Selenium
#[derive(Parser, Debug)]
#[clap(version, about, long_about = None)]
struct Cli {
    /// Browser type (e.g., chrome, firefox, edge)
    #[clap(short, long, value_parser)]
    browser: String,

    /// Major browser version (e.g., 105, 106, etc.)
    #[clap(short, long, value_parser, default_value = "")]
    version: String,

    /// Display DEBUG messages
    #[clap(short, long)]
    debug: bool,

    /// Display TRACE messages
    #[clap(short, long)]
    trace: bool,
}

fn main() -> Result<(), Box<dyn Error>> {
    let cli = Cli::parse();
    Ok(())
}
```


### Steps to reproduce the bug with the above code

```
cargo run -- --help
```

### Actual Behaviour

```
thread 'main' panicked at 'Command selenium-manager: Argument names must be unique, but 'version' is in use by more than one argument or group (call `cmd.disable_version_flag(true)` to remove the auto-generated `--version`)', C:\Users\boni\.cargo\registry\src\github.com-1ecc6299db9ec823\clap-4.0.2\src\builder\debug_asserts.rs:95:13
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
error: process didn't exit successfully: `C:\Users\boni\Documents\dev\rust-examples\target\debug\selenium-manager.exe --help` (exit code: 101)
```

### Expected Behaviour

The CLI help should be shown.

### Additional Context

This behavior started to happen with clap 4.0.2, recently released. It seems the problem is the use of `version` as argument. If I remove `version` from the following line, it works again (but I wanted to show also my app version in the help):

```
#[clap(about, long_about = None)]
```

### Debug Output

```
clap = { version = "4.0.2", features = ["derive"] }
```

---

_Label `C-bug` added by @bonigarcia on 2022-09-29 07:58_

---

_Comment by @epage on 2022-09-29 14:02_

When you specify the `#[clap(version)]` attribute, a `--version` flag is created automatically for you.  If you do not want it, you can disable it via `#[clap(disable_version_flag)]`.

The change in behavior is that clap would try to discern when to automatically apply `disable_version_flag` and we removed all of that logic.  A summary of what was changed and why can be found in the [announcement](https://epage.github.io/blog/2022/09/clap4/#removing-implicit-version-help-behavior).  The changelog has a one-line item about it.

---

_Closed by @epage on 2022-09-29 14:02_

---

_Comment by @bonigarcia on 2022-09-29 14:40_

Thanks a lot for the tip. I tried that as follows:

```
/// Selenium-Manager: Automated driver management for Selenium
#[derive(Parser, Debug)]
#[clap(version, about, long_about = None, disable_version_flag = true)]
struct Cli {
    /// Browser type (e.g., chrome, firefox, edge)
    #[clap(short, long, value_parser)]
    browser: String,

    /// Major browser version (e.g., 105, 106, etc.)
    #[clap(short, long, value_parser, default_value = "")]
    version: String,

    /// Display DEBUG messages
    #[clap(short, long)]
    debug: bool,

    /// Display TRACE messages
    #[clap(short, long)]
    trace: bool,
}
```

Nevertheless, when trying to see the help, the my app version is missing:

```
$ cargo run -- --help

Selenium-Manager: Automated driver management for Selenium

Usage: selenium-manager.exe [OPTIONS] --browser <BROWSER>

Options:
  -b, --browser <BROWSER>  Browser type (e.g., chrome, firefox, edge)
  -v, --version <VERSION>  Major browser version (e.g., 105, 106, etc.) [default: ]
  -d, --debug              Display DEBUG messages
  -t, --trace              Display TRACE messages
  -h, --help               Print help information
```

In clap 3.2.22, the version has shown as follows:

```
$ cargo run -- --help

selenium-manager 0.1.0
Selenium-Manager: Automated driver management for Selenium

Usage: selenium-manager.exe [OPTIONS] --browser <BROWSER>

Options:
  -b, --browser <BROWSER>  Browser type (e.g., chrome, firefox, edge)
  -v, --version <VERSION>  Major browser version (e.g., 105, 106, etc.) [default: ]
  -d, --debug              Display DEBUG messages
  -t, --trace              Display TRACE messages
  -h, --help               Print help information
```

Is there any issue with the new version or am I doing something wrong? Thanks again.


---

_Comment by @epage on 2022-09-29 14:47_

Another change in v4 is we no longer display name, version, and author in the `-h` output by default (that is what the `--version` flag is for...).

You can override the `help_template` attribute to get the old behavior.  https://github.com/clap-rs/clap/blob/master/tests/derive/utils.rs#L10 contains the old template.

---

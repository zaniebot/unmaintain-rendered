---
number: 1341
title: Disable optional suggestion features not working
type: issue
state: closed
author: Folyd
labels: []
assignees: []
created_at: 2018-09-14T11:35:56Z
updated_at: 2018-09-16T15:11:52Z
url: https://github.com/clap-rs/clap/issues/1341
synced_at: 2026-01-07T13:12:19-06:00
---

# Disable optional suggestion features not working

---

_Issue opened by @Folyd on 2018-09-14 11:35_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

rustc 1.29.0 (aa3ca1994 2018-09-11)

### Affected Version of clap

2.32

### Bug  Summary
I have disabled *suggestion* feature on Cargo.toml, see following config:
```toml
[dependencies.clap]
version = "2.32"
default-features = false
features = [ "color", "vec_map"]
```
But still not working.

```sh
$ cargo run --release lo
    Finished release [optimized] target(s) in 0.10s
     Running `/Users/wichna/RustProjects/anyshortcut-cli/target/release/anyshortcut-cli lo`
error: The subcommand 'lo' wasn't recognized
        Did you mean 'login'?

If you believe you received this message in error, try re-running with 'anyshortcut-cli -- lo'

USAGE:
    anyshortcut-cli [ARGS]
    anyshortcut-cli <SUBCOMMAND>

For more information try --help
```
Here is my cli app help message:
```
$ cargo run --release
    Finished release [optimized] target(s) in 0.09s
     Running `/Users/wichna/RustProjects/anyshortcut-cli/target/release/anyshortcut-cli`
anyshortcut-cli 0.1.0
A blaze fast way to launch your favorite website in Terminal.

USAGE:
    anyshortcut-cli [ARGS]
    anyshortcut-cli <SUBCOMMAND>

OPTIONS:
    -h, --help
            Print this help message.

    -V, --version
            Show version information.


ARGS:
    <PRIMARY_KEY | COMPOUND_KEY>
            Using primary shortcut key (A~Z|0~9) or compound shortcut key (AA~ZZ) to open the url.

    <SECONDARY_KEY>
            Use secondary shortcut key (A~Z|0~9) to open the url.


SUBCOMMANDS:
    list      List shortcuts.
    login     Login with the token
    logout    Logout and clean local data.
    sync      Sync all shortcuts after login.
```

---

_Renamed from "Disable Optional suggestion features not working" to "Disable optional suggestion features not working" by @Folyd on 2018-09-14 11:36_

---

_Comment by @Folyd on 2018-09-16 15:11_

I know why it didn't work because I have another *clap* build dependencies which didn't disable the default features. So I fix by following configuration:
```toml
[dependencies.clap]
version = "2.32"
default-features = false
features = [ "color", "vec_map" ]

[build-dependencies.clap]
version = "2.32"
default-features = false
```

---

_Closed by @Folyd on 2018-09-16 15:11_

---

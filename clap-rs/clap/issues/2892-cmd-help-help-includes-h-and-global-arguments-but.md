---
number: 2892
title: "`cmd help help` includes `-h` and global arguments but errors when you pass them in"
type: issue
state: closed
author: epage
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2021-10-16T00:57:13Z
updated_at: 2021-12-13T22:15:58Z
url: https://github.com/clap-rs/clap/issues/2892
synced_at: 2026-01-10T01:27:28Z
---

# `cmd help help` includes `-h` and global arguments but errors when you pass them in

---

_Issue opened by @epage on 2021-10-16 00:57_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.55.0 (c8dfcfe04 2021-09-06)

### Clap Version

master

### Minimal reproducible code

```rust
fn main() {
    use clap::*;
    let mut app = App::new("test_app")
        .arg(Arg::new("config").short('c').global(true))
        .arg(Arg::new("v").short('v'))
        .subcommand(
            App::new("test")
                .about("Subcommand")
                .arg(Arg::new("debug").short('d')),
        );

    app.get_matches_mut();
}
```


### Steps to reproduce the bug with the above code

`cargo run -- help help`

### Actual Behaviour

```
test-clap-help 

Print this message or the help of the given subcommand(s)

USAGE:
    test-clap help [OPTIONS] [SUBCOMMAND]...

ARGS:
    <SUBCOMMAND>...    The subcommand whose help message to display

OPTIONS:
    -c            
    -h, --help    Print help information
```

### Expected Behaviour

```
test-clap-help 

Print this message or the help of the given subcommand(s)

USAGE:
    test-clap help [OPTIONS] [SUBCOMMAND]...

ARGS:
    <SUBCOMMAND>...    The subcommand whose help message to display
```

or for being able to run `cargo run -- help -c` but that produces
```
error:  The subcommand '-c' wasn't recognized

USAGE:
    test-clap <subcommands>

For more information try --help
```
because we are operating on raw data without the parser having pulled out `-c` or `-h`, so we look those up as if they were subcommands which fails

### Additional Context

This used to be worse but #2887 made some progress towards this.

This seems to be a regression compared to clap2
```
test-clap-help 
Prints this message or the help of the given subcommand(s)

USAGE:
    test-clap help [subcommand]...

ARGS:
    <subcommand>...    The subcommand whose help message to display
```

### Debug Output

_No response_

---

_Label `T: bug` added by @epage on 2021-10-16 00:57_

---

_Label `C: help message` added by @epage on 2021-10-16 00:57_

---

_Label `C: subcommands` added by @epage on 2021-10-16 00:57_

---

_Label `C: parsing` added by @epage on 2021-10-16 00:57_

---

_Added to milestone `3.0` by @epage on 2021-10-16 00:57_

---

_Comment by @epage on 2021-10-16 00:58_

I recommend we defer this out of 3.0.

I put this in the 3.0 milestone because it is a regression.  I'm assuming fixing this will not be a breaking change and this is a corner of a corner case, so I think its fine for the initial release.  My only concern is what other issues might be lingering from the same root cause.

---

_Comment by @pksunkara on 2021-10-16 01:16_

This might be because we manipulate help and version global args when starting up. But this subcommand gets added during parsing (which I don't think is good code). If we can move the subcommand adding to before, that should fix it.

---

_Removed from milestone `3.0` by @epage on 2021-10-16 02:01_

---

_Added to milestone `3.1` by @epage on 2021-10-16 02:01_

---

_Referenced in [epage/clapng#222](../../epage/clapng/issues/222.md) on 2021-12-06 22:18_

---

_Label `C: subcommands` removed by @epage on 2021-12-08 20:06_

---

_Label `A-parsing` removed by @epage on 2021-12-08 20:06_

---

_Removed from milestone `3.1` by @epage on 2021-12-08 20:06_

---

_Referenced in [clap-rs/clap#3169](../../clap-rs/clap/pulls/3169.md) on 2021-12-13 22:02_

---

_Closed by @pksunkara on 2021-12-13 22:15_

---

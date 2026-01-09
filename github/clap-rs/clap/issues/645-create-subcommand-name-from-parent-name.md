---
number: 645
title: Create SubCommand name from parent name
type: issue
state: closed
author: SuperFluffy
labels: []
assignees: []
created_at: 2016-09-01T21:35:34Z
updated_at: 2018-08-02T03:29:53Z
url: https://github.com/clap-rs/clap/issues/645
synced_at: 2026-01-07T13:12:19-06:00
---

# Create SubCommand name from parent name

---

_Issue opened by @SuperFluffy on 2016-09-01 21:35_

Is there a way to have the application name displayed as `app_name sub_cmd version`? Right now, if the App carries the name `foo` but has the binary `bar` (as per `Cargo.toml`), invoking `./bar -h` will show `foo <version>`, whereas `./bar help subcmd` will show `bar-subcmd <version>`, which I think ios quite confusing.

Take this MWE:

``` rust
extern crate clap;

use clap::{App, AppSettings, SubCommand};

fn main() {
    let args = App::new("blast")
                   .setting(AppSettings::GlobalVersion) // all subcommands have version "1.2.3" now
                   .version("1.2.3")
                   .subcommand(SubCommand::with_name("status"))
                   .get_matches();
}
```

And the respective outputs:

``` bash
./target/debug/test -h
blast 1.2.3

USAGE:
    test [SUBCOMMAND]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

SUBCOMMANDS:
    help      Prints this message or the help of the given subcommand(s)
    status
```

And with the subcommand:

``` bash
% ./target/debug/test -h
test-status 1.2.3

USAGE:
    test status

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information
```


---

_Comment by @kbknapp on 2016-09-05 18:02_

This is because some people wish to use the name, "My Awesome Whatchabob" as the app name, but the binary is something like, `maw` So when one requests `maw -V` They want, "My Awesome Whatchabob v2.12" displayed.

Now when it comes to subcommands, it's more akin to the path one takes to get there, `maw` may have subcommand `qux` and displaying, `My Awesome Whatchabob-qux v1.1` may work in this simple example, but not when names get more complex. In that case it's easier to use the binary name `maw-qux v1.1`

This really only comes in to play when the binary name and application name don't match (which happens, but is also somewhat rare).

If you wish to force the binary name, you can do so withe [`App::bin_name`](https://docs.rs/clap/2.11.0/clap/struct.App.html#method.bin_name) method, in which case subcommands would then be displayed as `My Awesome Whatchabob-qux`


---

_Label `T: RFC / question` added by @kbknapp on 2016-09-05 18:02_

---

_Closed by @kbknapp on 2016-10-05 16:34_

---

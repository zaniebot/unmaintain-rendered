```yaml
number: 591
title: subcommand help does not show in color.
type: issue
state: closed
author: laishulu
labels: []
assignees: []
created_at: 2016-07-23T09:28:22Z
updated_at: 2018-08-02T03:29:52Z
url: https://github.com/clap-rs/clap/issues/591
synced_at: 2026-01-12T16:14:09Z
```

# subcommand help does not show in color.

---

_@laishulu_

I have a subcommand `foo`, I run `myapp help foo`, I still can't see any color, even I have the following codes.

``` rust
    let yaml = load_yaml!("cli.yml");
    let app_m = clap::App::from_yaml(yaml)
        .setting(clap::AppSettings::SubcommandRequired)
        .global_setting(clap::AppSettings::ColoredHelp)
        .get_matches();
```

the clap version is 2.9.2

Related with issue: https://github.com/kbknapp/clap-rs/issues/538


---

_Referenced in [clap-rs/clap#538](../../clap-rs/clap/issues/538.md) on 2016-07-23 19:36_

---

_Comment by @kbknapp on 2016-07-23 19:47_

I just tested this, and I'm not able to reproduce...

Here is the test case I used that worked:

``` toml
# Cargo.toml
[dependencies]
clap = { version = "*", features = ["yaml"]}
```

``` yaml
# app.yml
name: yml_app
version: 1.0
about: an example using a .yml file to build a CLI
author: Kevin K. <kbknapp@gmail.com>

settings:
    - SubcommandRequired

global_settings:
    - ColoredHelp

subcommands:
    - subcmd:
        about: demos subcommands from yaml
        version: 0.1
        author: Kevin K. <kbknapp@gmail.com>
```

``` rust
// main.rs
#[macro_use]
extern crate clap;

use clap::{App, Arg, AppSettings};

fn main() {
    let yml = load_yaml!("app.yml");
    let m = App::from_yaml(yml)
        .get_matches();
}
```

Running `myapp help subcmd` produces colored help messages.


---

_Label `T: RFC / question` added by @kbknapp on 2016-07-23 19:48_

---

_Comment by @laishulu on 2016-07-23 20:05_

OK now, with the following lines in the yml file

``` yaml
global_settings:
    - ColoredHelp
```

Previously you gave me the wrong instruction here https://github.com/kbknapp/clap-rs/issues/538#issuecomment-234732736


---

_Closed by @laishulu on 2016-07-23 20:05_

---

_Comment by @kbknapp on 2016-07-23 20:08_

:+1: 


---

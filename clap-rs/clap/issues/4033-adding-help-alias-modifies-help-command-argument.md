```yaml
number: 4033
title: Adding help alias modifies help command argument rendering?
type: issue
state: closed
author: SUPERCILEX
labels: []
assignees: []
created_at: 2022-08-06T00:01:51Z
updated_at: 2022-08-11T01:46:53Z
url: https://github.com/clap-rs/clap/issues/4033
synced_at: 2026-01-12T16:14:15Z
```

# Adding help alias modifies help command argument rendering?

---

_@SUPERCILEX_

Adding `#[clap(mut_arg("help", |arg| arg.short_alias('?')))]` changes the way the help command is rendered. See this diff: https://github.com/SUPERCILEX/ftzz/commit/6daf6aa7cc59b6c72a97ab34e3a76983691fee61. Seems like a bug that adding the alias makes the help command show up inside itself. Or not (as long as its consistent I don't mind).

---

_Comment by @epage on 2022-08-06 00:11_

Could you follow the [bug report template](https://github.com/clap-rs/clap/blob/master/.github/ISSUE_TEMPLATE/bug_report.yml)?  Being able to have small reproduction cases, debug traces before/after, etc are a big help to triaging this.

---

_Comment by @SUPERCILEX on 2022-08-06 01:36_

```rust
#!/usr/bin/env -S rust-script --debug

//! ```cargo
//! [dependencies]
//! clap = { version = "3.2.16", features = ["derive"] }
//! ```

use clap::{Parser, Subcommand};

#[derive(Parser)]
#[clap(mut_arg("help", |arg| arg.short_alias('?')))]
struct Cli {
  #[clap(subcommand)]
  command: Commands,
}

#[derive(Subcommand)]
enum Commands { Foo }

fn main() {
    let mut _command = Cli::parse();
}
```

Run:

```
$ ./foo.rs help help
foo_bcd390fa607fcb4d15f74b68-help 
Print this message or the help of the given subcommand(s)

USAGE:
    foo_bcd390fa607fcb4d15f74b68 help [SUBCOMMAND]...

ARGS:
    <SUBCOMMAND>...    The subcommand whose help message to display

OPTIONS:
    -h, --help    Print help information
```

---

_Comment by @SUPERCILEX on 2022-08-06 01:37_

Note that `$ ./foo.rs help -h` doesn't work so those options shouldn't show up.

---

_Comment by @epage on 2022-08-09 18:35_

FYI there are still parts of the template missing, like debug output.

My first guess is that this is because we do this
```rust
            help_subcmd = help_subcmd
                .setting(AppSettings::DisableHelpFlag)
                .unset_global_setting(AppSettings::PropagateVersion);
```

but when we `_build_self`, we overwrite settings with global settings.  We need to also set the global setting to disable it.

---

_Comment by @epage on 2022-08-10 14:13_

Correction, the challenge is with
```rust
        {
            let generated_help_pos = self
                .args
                .args()
                .position(|x| x.id == Id::help_hash() && x.provider == ArgProvider::Generated);

            if let Some(index) = generated_help_pos {
                debug!("Command::_check_help_and_version: Removing generated help");
                self.args.remove(index);
            }
```
So if a user modifies it, it becomes `GeneratedMutated`.
The comments suggest that that is meant to deal with another problem,.

---

_Referenced in [clap-rs/clap#4054](../../clap-rs/clap/pulls/4054.md) on 2022-08-10 14:15_

---

_Referenced in [clap-rs/clap#4056](../../clap-rs/clap/pulls/4056.md) on 2022-08-11 00:51_

---

_Closed by @epage on 2022-08-11 01:46_

---

_Closed by @epage on 2022-08-11 01:46_

---

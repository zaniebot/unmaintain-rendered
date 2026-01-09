---
number: 6073
title: Add some test cases for Command aliases
type: issue
state: closed
author: GilShoshan94
labels: []
assignees: []
created_at: 2025-07-21T20:08:32Z
updated_at: 2025-07-30T02:16:26Z
url: https://github.com/clap-rs/clap/issues/6073
synced_at: 2026-01-07T13:12:20-06:00
---

# Add some test cases for Command aliases

---

_Issue opened by @GilShoshan94 on 2025-07-21 20:08_

Hi,
The tests for aliases (of arguments and subcommands) are distributed between several files:

For Arg:
-tests/builder/arg_aliases_short.rs
-tests/builder/arg_aliases.rs
It covers well everything I think regarding aliases.

For Command (on the subcommand):
-tests/builder/flag_subcommands.rs
-tests/builder/subcommands.rs
It does not 100% fully cover in my opinion as **it is missing the following cases:**

[visible_aliases](https://docs.rs/clap/4.5.41/clap/struct.Command.html#method.visible_aliases) (but visible_alias is tested, but only to match the help, not tested with a try_get_matches_from)
[visible_short_flag_alias](https://docs.rs/clap/4.5.41/clap/struct.Command.html#method.visible_short_flag_alias) (but visible_short_flag_aliases is properly tested)
[visible_long_flag_alias](https://docs.rs/clap/4.5.41/clap/struct.Command.html#method.visible_long_flag_alias) (but visible_long_flag_aliases is properly tested)

---

**Either we should add the few missing case, or we should maybe consolidate all the aliases cases into one place** (expect for the help that are fully covered in `tests/builder/help.rs`).

---

Here a code for inspiration that covers all the aliases and check for matching.
```rust
use clap::{Arg, Command};

fn setup_aliases() -> Command {
    Command::new("ctest")
        .version("0.1")
        .arg(
            Arg::new("dest")
                .short('d')
                .long("destination")
                .value_name("FILE")
                .help("File to save into")
                .long_help("The Filepath to save into the result")
                .short_alias('q')
                .short_aliases(['w', 'e'])
                .alias("arg-alias")
                .aliases(["do-stuff", "do-tests"])
                .visible_short_alias('t')
                .visible_short_aliases(['i', 'o'])
                .visible_alias("file")
                .visible_aliases(["into", "to"])
                .action(ArgAction::Set),
        )
        .subcommand(
            Command::new("rev")
                .short_flag('r')
                .long_flag("inplace")
                .about("In place")
                .long_about("Change mode to work in place on source")
                .alias("subc-alias")
                .aliases(["subc-do-stuff", "subc-do-tests"])
                .short_flag_alias('j')
                .short_flag_aliases(['k', 'l'])
                .long_flag_alias("subc-long-flag-alias")
                .long_flag_aliases(["subc-long-do-stuff", "subc-long-do-tests"])
                .visible_alias("source")
                .visible_aliases(["from", "onsource"])
                .visible_short_flag_alias('s')
                .visible_short_flag_aliases(['f', 'g'])
                .visible_long_flag_alias("origin")
                .visible_long_flag_aliases(["path", "tryfrom"])
                .arg(
                    Arg::new("input")
                        .value_name("INPUT")
                        .help("The source file"),
                ),
        )
}

#[test]
fn all_aliases_work() {
    let app = setup_aliases();

    for arg_al in [
        "-d",
        "--destination",
        "-q",
        "-w",
        "-e",
        "--arg-alias",
        "--do-stuff",
        "--do-tests",
        "-t",
        "-i",
        "-o",
        "--file",
        "--into",
        "--to",
    ] {
        let m = app
            .clone()
            .try_get_matches_from(vec!["ctest", arg_al, "filepath"]);
        assert!(m.is_ok(), "{}", m.unwrap_err());
        let m = m.unwrap();
        assert!(m.subcommand_name().is_none());
    }

    for subc_al in [
        "rev",
        "-r",
        "--inplace",
        "subc-alias",
        "subc-do-stuff",
        "subc-do-tests",
        "-j",
        "-k",
        "-l",
        "--subc-long-flag-alias",
        "--subc-long-do-tests",
        "source",
        "from",
        "onsource",
        "-s",
        "-f",
        "-g",
        "--origin",
        "--path",
        "--tryfrom",
    ] {
        let m = app
            .clone()
            .try_get_matches_from(vec!["ctest", subc_al, "filepath"]);
        assert!(m.is_ok(), "{}", m.unwrap_err());
        let m = m.unwrap();
        assert!(m.subcommand_name().is_some());
    }
}
```

---

_Referenced in [clap-rs/clap#6068](../../clap-rs/clap/pulls/6068.md) on 2025-07-21 21:27_

---

_Comment by @GilShoshan94 on 2025-07-21 21:30_

Closed by https://github.com/clap-rs/clap/pull/6068

---

_Closed by @epage on 2025-07-30 02:16_

---

```yaml
number: 2499
title: "[v3 Bug] `flag_subcommands` doesn't work well with list of arguments"
type: issue
state: closed
author: rami3l
labels:
  - C-bug
  - ":money_with_wings: $5"
assignees: []
created_at: 2021-05-23T10:03:01Z
updated_at: 2021-05-26T23:44:50Z
url: https://github.com/clap-rs/clap/issues/2499
synced_at: 2026-01-12T16:14:13Z
```

# [v3 Bug] `flag_subcommands` doesn't work well with list of arguments

---

_@rami3l_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.52.1 (9bc8c42bb 2021-05-09)

### Clap Version

3.0.0-beta.2

### Minimal reproducible code

```rust
//# clap = "3.0.0-beta.2"

use clap::{App, AppSettings, Arg};

fn match_from(call: &[&str]) {
    let matches = App::new("pacaptr")
        .setting(AppSettings::SubcommandRequiredElseHelp)
        .subcommand(
            App::new("sync")
                .short_flag('S')
                .long_flag("sync")
                .about("Synchronize packages.")
                .arg(
                    Arg::new("info")
                        .long("info")
                        .short('i')
                        .about("view package information"),
                )
                .arg(
                    Arg::new("package")
                        .about("packages")
                        .multiple(true)
                        .takes_value(true),
                ),
        )
        .arg(
            Arg::new("pm")
                .long("pm")
                .multiple(false)
                .takes_value(true)
                .global(true),
        )
        .get_matches_from(call);

    assert_eq!(Some("mockpm"), matches.value_of("pm"));
    match matches.subcommand() {
        Some(("sync", sync_matches)) => {
            let packages: Vec<_> = sync_matches.values_of("package").unwrap().collect();
            let values = packages.join(", ");

            assert!(sync_matches.is_present("info"));
            assert_eq!(values, "docker");
        }
        _ => panic!(),
    }
}

fn main() {
    // These examples pass...
    match_from(&["pacaptr", "--pm", "mockpm", "--sync", "-i", "docker"]);
    match_from(&["pacaptr", "--pm=mockpm", "--sync", "-i", "docker"]);
    match_from(&["pacaptr", "-Si", "--pm", "mockpm", "docker"]);
    match_from(&["pacaptr", "-Si", "docker", "--pm", "mockpm"]);
    // But any of these doesn't:
    // thread 'main' panicked at 'assertion failed: sync_matches.is_present(\"info\")', src/main.rs:40:13
    // match_from(&["pacaptr", "--pm", "mockpm", "-Si", "docker"]);
    // match_from(&["pacaptr", "--pm=mockpm", "-Si", "docker"]);
}
```


### Steps to reproduce the bug with the above code

`cargo run`

### Actual Behaviour

When I run `pacaptr --pm mockpm -Si docker`:
- `--sync` is read properly.
- `--info` is **not read**.
- `packages == &["-Si", "docker"]`

### Expected Behaviour

When I run `pacaptr --pm mockpm -Si docker`:
- `--sync` is read properly.
- `--info` is read properly.
- `packages == &["docker"]`

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `T: bug` added by @rami3l on 2021-05-23 10:03_

---

_Comment by @pksunkara on 2021-05-23 10:18_

Can you please reduce the code and behavior more by removing the unrelated pm arg?

---

_Label `:money_with_wings: $5` added by @pksunkara on 2021-05-23 10:18_

---

_Label `C: subcommands` added by @pksunkara on 2021-05-23 10:18_

---

_Label `C: flags` added by @pksunkara on 2021-05-23 10:18_

---

_Comment by @rami3l on 2021-05-23 10:57_

@pksunkara Just experimented, and unfortunately I can't: this issue is also gone once I remove the `--pm <name>` arg (either from `call`, like `pacaptr -Si docker`, or from the codebase, see [example 23](https://github.com/clap-rs/clap/blob/master/examples/23_flag_subcommands_pacman.rs)), so it's not unrelated.

---

_Referenced in [rami3l/pacaptr#109](../../rami3l/pacaptr/pulls/109.md) on 2021-05-25 20:17_

---

_Comment by @rami3l on 2021-05-26 09:07_

@pksunkara Although it’s a bit hard to make sense of the `clap` code base for me, I tried to dig a little deeper into this bug. Let’s take the example of `pacaptr --pm mockpm -Si docker`.

In the parsing process, we are saving `cur_idx` to `skip_idxs`:
https://github.com/clap-rs/clap/blob/bddb63effe55ee490cd85372f1b541ce9e2be383/src/parse/parser.rs#L409-L415

And when parsing the flag `-i` somehow we have `arg = "Si"`, but `cur_idx == 3`. (Is this because the argument is at index 3? Because when `--pm mockpm` is not present, `cur_idx == 1` is the correct value to be later passed to `skip`.)

https://github.com/clap-rs/clap/blob/bddb63effe55ee490cd85372f1b541ce9e2be383/src/parse/parser.rs#L1063-L1064

… so the following `for` loop is completely skipped, since `arg` is only of length 2, and thus the `-i` flag gets ignored.

https://github.com/clap-rs/clap/blob/bddb63effe55ee490cd85372f1b541ce9e2be383/src/parse/parser.rs#L1077-L1080

I think I might be able to write a bug fix soon.


---

_Referenced in [clap-rs/clap#2501](../../clap-rs/clap/pulls/2501.md) on 2021-05-26 20:00_

---

_Renamed from "[v3 Bug?] `flag_subcommands` doesn't work well with list of arguments" to "[v3 Bug] `flag_subcommands` doesn't work well with list of arguments" by @rami3l on 2021-05-26 21:30_

---

_Closed by @pksunkara on 2021-05-26 23:44_

---

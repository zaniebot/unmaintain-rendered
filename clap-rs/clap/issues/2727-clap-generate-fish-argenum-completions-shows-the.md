---
number: 2727
title: "clap_generate fish: ArgEnum completions shows the description of the argument"
type: issue
state: closed
author: ModProg
labels:
  - C-bug
assignees: []
created_at: 2021-08-20T00:37:31Z
updated_at: 2021-08-25T15:48:28Z
url: https://github.com/clap-rs/clap/issues/2727
synced_at: 2026-01-10T01:27:24Z
---

# clap_generate fish: ArgEnum completions shows the description of the argument

---

_Issue opened by @ModProg on 2021-08-20 00:37_

### Rust Version

rustc 1.54.0 (a178d0322 2021-07-26)

### Clap Version

master

### Minimal reproducible code

```rs
use clap::{ArgEnum, Clap, IntoApp};
use clap_generate::generators;

fn main() {
    clap_generate::generate::<generators::Fish, _>(
        &mut App::into_app(),
        "clap-issue",
        &mut std::io::stdout(),
    );
}

#[derive(Clap)]
struct App {
    #[clap(subcommand)]
    arg: A,
}

#[derive(Clap)]
enum A {
    A {
        /// This should not be in the completion
        #[clap(arg_enum)]
        arg: AA,
    },
}

#[derive(ArgEnum)]
enum AA {
    AA,
    BB,
}
```

### Steps to reproduce the bug with the above code

`cargo run`

### Actual Behaviour

```sh
complete -c clap-issue -n "__fish_use_subcommand" -s h -l help -d 'Print help information'
complete -c clap-issue -n "__fish_use_subcommand" -s V -l version -d 'Print version information'
complete -c clap-issue -n "__fish_use_subcommand" -f -a "a"
complete -c clap-issue -n "__fish_use_subcommand" -f -a "help" -d 'Print this message or the helpof the given subcommand(s)'
complete -c clap-issue -n "__fish_seen_subcommand_from a" -d 'This should not be in the completion' -r -f -a "aa bb"
complete -c clap-issue -n "__fish_seen_subcommand_from a" -s h -l help -d 'Print help information'
complete -c clap-issue -n "__fish_seen_subcommand_from a" -s V -l version -d 'Print version information'
complete -c clap-issue -n "__fish_seen_subcommand_from help" -s h -l help -d 'Print help information'
complete -c clap-issue -n "__fish_seen_subcommand_from help" -s V -l version -d 'Print version information'
```

### Expected Behaviour

```sh
complete -c clap-issue -n "__fish_use_subcommand" -s h -l help -d 'Print help information'
complete -c clap-issue -n "__fish_use_subcommand" -s V -l version -d 'Print version information'
complete -c clap-issue -n "__fish_use_subcommand" -f -a "a"
complete -c clap-issue -n "__fish_use_subcommand" -f -a "help" -d 'Print this message or the helpof the given subcommand(s)'
complete -c clap-issue -n "__fish_seen_subcommand_from a" -r -f -a "aa bb"
complete -c clap-issue -n "__fish_seen_subcommand_from a" -s h -l help -d 'Print help information'
complete -c clap-issue -n "__fish_seen_subcommand_from a" -s V -l version -d 'Print version information'
complete -c clap-issue -n "__fish_seen_subcommand_from help" -s h -l help -d 'Print help information'
complete -c clap-issue -n "__fish_seen_subcommand_from help" -s V -l version -d 'Print version information'
```

### Additional Context

The problem is that in fish the description will be put behind every alternative like so:
```
❯ clap-issue a
aa  (This should not be in the completion)  bb  (This should not be in the completion)
```

A more reasonable description would be https://github.com/clap-rs/clap/discussions/2728

### Debug Output

_No response_

---

_Label `T: bug` added by @ModProg on 2021-08-20 00:37_

---

_Comment by @ModProg on 2021-08-20 22:07_

This cannot be solved by just removing the description as that would remove it fom the option completion as well: `--smth (this is the description`.

The solution is, to add the options first, and then only the description:
```fish
complete -c clap-issue -n "__fish_seen_subcommand_from test" -l case -r -f -a "a b c d"
complete -c clap-issue -n "__fish_seen_subcommand_from test" -l case -d 'the case to test' -r -f 
```

---

_Referenced in [clap-rs/clap#2730](../../clap-rs/clap/pulls/2730.md) on 2021-08-20 22:56_

---

_Closed by @pksunkara on 2021-08-25 15:48_

---

---
number: 2724
title: "global_setting DisableVersionFlag and DisableHelpFlag don't remove auto completion for subcommands fully"
type: issue
state: closed
author: ModProg
labels:
  - C-bug
  - E-medium
  - A-completion
  - ":money_with_wings: $5"
assignees: []
created_at: 2021-08-18T22:14:39Z
updated_at: 2021-12-13T22:15:59Z
url: https://github.com/clap-rs/clap/issues/2724
synced_at: 2026-01-07T13:12:19-06:00
---

# global_setting DisableVersionFlag and DisableHelpFlag don't remove auto completion for subcommands fully

---

_Issue opened by @ModProg on 2021-08-18 22:14_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.54.0 (a178d0322 2021-07-26)

### Clap Version

master

### Minimal reproducible code

```rust
use clap::{AppSettings, Clap, IntoApp};
use clap_generate::generators;

fn main() {
    // App::parse();

    clap_generate::generate::<generators::Fish, _>(
        &mut App::into_app(),
        "command",
        &mut std::io::stdout(),
    );
}

#[derive(Clap)]
#[clap(global_setting=AppSettings::DisableVersionFlag, global_setting=AppSettings::DisableHelpFlag)]
struct App {
    #[clap(subcommand)]
    subcommand: A,
}

#[derive(Clap)]
enum A {
    A,
}
```


### Steps to reproduce the bug with the above code

add clap and clap_generate (master) to your project
`cargo run`

### Actual Behaviour

The generated completions are:
```sh
complete -c command -n "__fish_use_subcommand" -f -a "a"
complete -c command -n "__fish_use_subcommand" -f -a "help" -d 'Print this message or the help of the given subcommand(s)'
complete -c command -n "__fish_seen_subcommand_from a" -l help -d 'Print help information'
complete -c command -n "__fish_seen_subcommand_from a" -l version -d 'Print version information'
complete -c command -n "__fish_seen_subcommand_from help" -l help -d 'Print help information'
complete -c command -n "__fish_seen_subcommand_from help" -l version -d 'Print version information'
```

### Expected Behaviour

The generated completions should be without the --help and --version:
```sh
complete -c command -n "__fish_use_subcommand" -f -a "a"
complete -c command -n "__fish_use_subcommand" -f -a "help" -d 'Print this message or the help of the given subcommand(s)'
```

### Additional Context

_No response_

### Debug Output

```
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/clap-issue`
[            clap::build::app]  App::_build
[            clap::build::app]  App::_propagate:clap-issue
[            clap::build::app]  App::_check_help_and_version
[            clap::build::app]  App::_check_help_and_version: Removing generated help
[            clap::build::app]  App::_check_help_and_version: Removing generated version
[            clap::build::app]  App::_check_help_and_version: Building help subcommand
[            clap::build::app]  App::_propagate_global_args:clap-issue
[            clap::build::app]  App::_derive_display_order:clap-issue
[            clap::build::app]  App::_derive_display_order:a
[            clap::build::app]  App::_derive_display_order:help
[clap::build::app::debug_asserts]       App::_debug_asserts
[            clap::build::app]  App::_build_bin_names
[            clap::build::app]  App::_build_bin_names:iter: bin_name set...
[            clap::build::app]  No
[            clap::build::app]  App::_build_bin_names:iter: Setting bin_name of clap-issue to command a
[            clap::build::app]  App::_build_bin_names:iter: Calling build_bin_names from...a
[            clap::build::app]  App::_build_bin_names
[            clap::build::app]  App::_build_bin_names:iter: bin_name set...
[            clap::build::app]  No
[            clap::build::app]  App::_build_bin_names:iter: Setting bin_name of clap-issue to command help
[            clap::build::app]  App::_build_bin_names:iter: Calling build_bin_names from...help
[            clap::build::app]  App::_build_bin_names
[clap_generate::generators::shells::fish]       gen_fish_inner
[clap_generate::generators::shells::fish]       gen_fish_inner: bin_name=command
[   clap_generate::generators]  flags: name=clap-issue
[clap_generate::generators::shells::fish]       gen_fish_inner
[clap_generate::generators::shells::fish]       gen_fish_inner: bin_name=a
[   clap_generate::generators]  flags: name=a
[clap_generate::generators::shells::fish]       gen_fish_inner
[clap_generate::generators::shells::fish]       gen_fish_inner: bin_name=help
[   clap_generate::generators]  flags: name=help
complete -c command -n "__fish_use_subcommand" -f -a "a"
complete -c command -n "__fish_use_subcommand" -f -a "help" -d 'Print this message or the help of the given subcommand(s)'
complete -c command -n "__fish_seen_subcommand_from a" -l help -d 'Print help information'
complete -c command -n "__fish_seen_subcommand_from a" -l version -d 'Print version information'
complete -c command -n "__fish_seen_subcommand_from help" -l help -d 'Print help information'
complete -c command -n "__fish_seen_subcommand_from help" -l version -d 'Print version information'
```

---

_Label `T: bug` added by @ModProg on 2021-08-18 22:14_

---

_Label `C: generator` added by @pksunkara on 2021-08-19 06:43_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2021-08-19 06:43_

---

_Label `D: easy` added by @pksunkara on 2021-08-19 06:43_

---

_Referenced in [epage/clapng#201](../../epage/clapng/issues/201.md) on 2021-12-06 21:20_

---

_Label `D: easy` removed by @epage on 2021-12-13 17:16_

---

_Label `E-easy` added by @epage on 2021-12-13 17:16_

---

_Comment by @epage on 2021-12-13 17:20_

For
```rust
use clap::{AppSettings, IntoApp, Parser};
use clap_generate::generators;

fn main() {
    // App::parse();

    clap_generate::generate::<_, _>(
        generators::Fish,
        &mut App::into_app(),
        "command",
        &mut std::io::stdout(),
    );
}

#[derive(Parser)]
#[clap(global_setting=AppSettings::DisableVersionFlag, global_setting=AppSettings::DisableHelpFlag)]
struct App {
    #[clap(subcommand)]
    subcommand: A,
}

#[derive(Parser)]
enum A {
    A,
}
```
We now produce
```fish
complete -c command -n "__fish_use_subcommand" -f -a "a"
complete -c command -n "__fish_use_subcommand" -f -a "help" -d 'Print this message or the help of the given subcommand(s)'
complete -c command -n "__fish_seen_subcommand_from help" -s h -l help -d 'Print help information'
```

---

_Comment by @epage on 2021-12-13 17:37_

The remaining issue is that we propagate `DisableHelpFlag` (`App::_propagate`) before we add the `help` subcommand (`_check_help_and_version`).

Swapping these actions caused failures among completion tests.  I did not verify if they were representing fixes or not.

If we can't go that route, some options include
- Manually propagating to the newly generated `help`
- Putting `help` in from the beginning, like we do `--help`, and remove it if needed

---

_Label `E-easy` removed by @epage on 2021-12-13 17:37_

---

_Label `E-medium` added by @epage on 2021-12-13 17:37_

---

_Referenced in [clap-rs/clap#3169](../../clap-rs/clap/pulls/3169.md) on 2021-12-13 22:02_

---

_Closed by @pksunkara on 2021-12-13 22:15_

---

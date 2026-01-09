---
number: 2009
title: "`clap_generate::generate()` panics on subcommands"
type: issue
state: closed
author: cpick
labels:
  - C-bug
  - A-completion
assignees: []
created_at: 2020-07-11T19:28:25Z
updated_at: 2021-03-30T21:32:07Z
url: https://github.com/clap-rs/clap/issues/2009
synced_at: 2026-01-07T13:12:19-06:00
---

# `clap_generate::generate()` panics on subcommands

---

_Issue opened by @cpick on 2020-07-11 19:28_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closes issues

### Code

```sh
$ cargo new generate-panic
$ cd generate-panic
$ cat <<EOF >>Cargo.toml
clap = "3.0.0-beta.1"
clap_generate = "3.0.0-beta.1"
EOF
```

```rust
use clap::{Clap, FromArgMatches, IntoApp};
use clap_generate::{generate, generators::Bash};

#[derive(Clap)]
enum Command {
    Generate,
}

#[derive(Clap)]
struct Arguments {
    #[clap(subcommand)]
    command: Command,
}

fn main() {
    let mut app = Arguments::into_app();
    let Arguments { command: _c, } = Arguments::from_arg_matches(&app.get_matches_mut());

    for sc in app.get_subcommands() {
        println!(
            "name: '{}' bin_name: {:?}",
            sc.get_name(),
            sc.get_bin_name()
        );
    }

    let bin_name = app.get_bin_name().unwrap().to_owned();
    generate::<Bash, _>(&mut app, bin_name, &mut std::io::stdout());
}
```

### Steps to reproduce the issue

1. Run `cargo run generate`
2. The following panic is produced:
>     Finished dev [unoptimized + debuginfo] target(s) in 0.02s
>      Running `target/debug/generate-panic generate`
> name: 'generate' bin_name: Some("generate-panic generate")
> name: 'help' bin_name: None
> thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', /Users/cpick/.cargo/registry/src/github.com-1ecc6299db9ec823/clap_generate-3.0.0-beta.1/src/generators/mod.rs:105:31
> note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

### Version

* **Rust**: rustc 1.44.1 (c7087fe00 2020-06-17)
* **Clap**: 3.0.0-beta.1 for `clap` and `clap_generate`, I have also reproduced with the current `master` (d8fccdb0ea48268192f2cfc1564c7e0f80d763fc)

### Actual Behavior Summary

Attempting to generate bash completions by running the code above as described in the "steps to reproduce" section causes a panic when trying to call [`sc.get_bin_name().unwrap()`](https://github.com/clap-rs/clap/blob/5239d2c3718732dce3dc639213f494b5f469f922/clap_generate/src/generators/mod.rs#L105) on any subcommand other than the one that was run (in the case of the example, the auto-generated `help` command).

The code above prints all the subcommands bin names and `help`s is `None`.

### Expected Behavior Summary

I expected bash completions to be generated.

### Additional context

None that I can think of.

### Debug output

Compile clap with `debug` feature:

```toml
[dependencies]
clap = { version = "*", features = ["debug"] }
```

The output may be very long, so feel free to link to a gist or attach a text file

<details>
<summary> Debug Output </summary>
<pre>
<code>

[Running 'cargo run generate']
   Compiling clap_derive v3.0.0-beta.1
   Compiling clap v3.0.0-beta.1
   Compiling clap_generate v3.0.0-beta.1
   Compiling generate-panic v0.1.0 (/Users/cpick/src/generate-panic)
    Finished dev [unoptimized + debuginfo] target(s) in 9.86s
     Running `target/debug/generate-panic generate`
[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_derive_display_order:generate-panic
[            clap::build::app] 	App::_derive_display_order:generate
[            clap::build::app] 	App::_create_help_and_version
[            clap::build::app] 	App::_create_help_and_version: Building --help
[            clap::build::app] 	App::_create_help_and_version: Building --version
[            clap::build::app] 	App::_create_help_and_version: Building help
[            clap::build::app] 	App::_debug_asserts
[            clap::build::arg] 	Arg::_debug_asserts:help
[            clap::build::arg] 	Arg::_debug_asserts:version
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::_build
[         clap::parse::parser] 	Parser::_verify_positionals
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing '"generate"' ([103, 101, 110, 101, 114, 97, 116, 101])
[         clap::parse::parser] 	Parser::is_new_arg: "generate":NotFound
[         clap::parse::parser] 	Parser::is_new_arg: arg_allows_tac=false
[         clap::parse::parser] 	Parser::is_new_arg: probably value
[         clap::parse::parser] 	Parser::is_new_arg: starts_new_arg=false
[         clap::parse::parser] 	Parser::possible_subcommand: arg="generate"
[         clap::parse::parser] 	Parser::get_matches_with: possible_sc=true, sc=Some("generate")
[         clap::parse::parser] 	Parser::parse_subcommand
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=[]
[            clap::build::app] 	App::_propagate:generate-panic
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_derive_display_order:generate
[            clap::build::app] 	App::_create_help_and_version
[            clap::build::app] 	App::_create_help_and_version: Building --help
[            clap::build::app] 	App::_create_help_and_version: Building --version
[            clap::build::app] 	App::_debug_asserts
[            clap::build::arg] 	Arg::_debug_asserts:help
[            clap::build::arg] 	Arg::_debug_asserts:version
[         clap::parse::parser] 	Parser::parse_subcommand: About to parse sc=generate
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::_build
[         clap::parse::parser] 	Parser::_verify_positionals
[         clap::parse::parser] 	Parser::remove_overrides
[      clap::parse::validator] 	Validator::validate
[         clap::parse::parser] 	Parser::add_defaults
[      clap::parse::validator] 	Validator::validate_conflicts
[      clap::parse::validator] 	Validator::validate_exclusive
[      clap::parse::validator] 	Validator::gather_conflicts
[      clap::parse::validator] 	Validator::validate_required: required=ChildGraph([])
[      clap::parse::validator] 	Validator::gather_requirements
[      clap::parse::validator] 	Validator::validate_required_unless
[      clap::parse::validator] 	Validator::validate_matched_args
[         clap::parse::parser] 	Parser::remove_overrides
[      clap::parse::validator] 	Validator::validate
[         clap::parse::parser] 	Parser::add_defaults
[      clap::parse::validator] 	Validator::validate_conflicts
[      clap::parse::validator] 	Validator::validate_exclusive
[      clap::parse::validator] 	Validator::gather_conflicts
[      clap::parse::validator] 	Validator::validate_required: required=ChildGraph([])
[      clap::parse::validator] 	Validator::gather_requirements
[      clap::parse::validator] 	Validator::validate_required_unless
[      clap::parse::validator] 	Validator::validate_matched_args
[    clap::parse::arg_matcher] 	ArgMatcher::get_global_values: global_arg_vec=[]
name: 'generate' bin_name: Some("generate-panic generate")
name: 'help' bin_name: None
[clap_generate::generators::shells::bash] 	all_options_for_path: path=generate-panic
[   clap_generate::generators] 	subcommands: name=generate-panic
[   clap_generate::generators] 	subcommands: Has subcommands...true
[   clap_generate::generators] 	subcommands:iter: name=generate, bin_name=generate-panic generate
thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', /Users/cpick/.cargo/registry/src/github.com-1ecc6299db9ec823/clap_generate-3.0.0-beta.1/src/generators/mod.rs:105:31
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
[Finished running. Exit status: 101]

</code>
</pre>
</details>


---

_Label `T: bug` added by @cpick on 2020-07-11 19:28_

---

_Added to milestone `3.0` by @pksunkara on 2020-07-12 09:02_

---

_Assigned to @pksunkara by @pksunkara on 2020-08-16 07:55_

---

_Comment by @newAM on 2020-08-22 15:20_

Can this issue have the `C: generator` label added to it?

---

_Label `C: generator` added by @CreepySkeleton on 2020-08-22 16:44_

---

_Label `C: subcommands` added by @CreepySkeleton on 2020-08-22 16:44_

---

_Label `P2: need to have` added by @CreepySkeleton on 2020-08-22 16:44_

---

_Referenced in [clap-rs/clap#2098](../../clap-rs/clap/issues/2098.md) on 2020-08-22 20:09_

---

_Comment by @gbrlsnchs on 2020-12-21 13:27_

I'm facing this issue even without any custom subcommands.

---

_Comment by @pksunkara on 2021-02-28 12:38_

@logansquirel This one.

---

_Comment by @pksunkara on 2021-03-10 20:16_

@logansquirel Did you get a chance to look at this? This might have already been fixed on master.

---

_Comment by @logansquirel on 2021-03-11 07:20_

> @logansquirel Did you get a chance to look at this? This might have already been fixed on master.

`Cargo.toml`:
```toml
[package]
name = "clap_issue_2009"
version = "0.1.0"
authors = ["Logan SQUIREL <logan.squirel@protonmail.com>"]
edition = "2018"

[dependencies]
clap = { git = "https://github.com/clap-rs/clap" }
clap_generate = { git = "https://github.com/clap-rs/clap" }
```

`main.rs`:
```rust
use clap::{Clap, FromArgMatches, IntoApp};
use clap_generate::{generate, generators::Bash};

#[derive(Clap)]
enum Command {
    Generate,
}

#[derive(Clap)]
struct Arguments {
    #[clap(subcommand)]
    command: Command,
}

fn main() {
    let mut app = Arguments::into_app();
    let Arguments { command: _c, } = Arguments::from_arg_matches(&app.get_matches_mut());
    let bin_name = app.get_bin_name().unwrap().to_owned();
    generate::<Bash, _>(&mut app, bin_name, &mut std::io::stdout());
}
```

Running:
```shell
❯ cargo run generate
_clap_issue_2009() {
    local i cur prev opts cmds
    COMPREPLY=()
    cur="${COMP_WORDS[COMP_CWORD]}"
    prev="${COMP_WORDS[COMP_CWORD-1]}"
    cmd=""
    opts=""

    for i in ${COMP_WORDS[@]}
    do
        case "${i}" in
            clap_issue_2009)
                cmd="clap_issue_2009"
                ;;

            generate)
                cmd+="__generate"
                ;;
            help)
                cmd+="__help"
                ;;
            *)
                ;;
        esac
    done

    case "${cmd}" in
        clap_issue_2009)
            opts=" -h -V  --help --version  generate help"
            if [[ ${cur} == -* || ${COMP_CWORD} -eq 1 ]] ; then
                COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
                return 0
            fi
            case "${prev}" in

                *)
                    COMPREPLY=()
                    ;;
            esac
            COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
            return 0
            ;;

        clap_issue_2009__generate)
            opts=" -h -V  --help --version  "
            if [[ ${cur} == -* || ${COMP_CWORD} -eq 2 ]] ; then
                COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
                return 0
            fi
            case "${prev}" in

                *)
                    COMPREPLY=()
                    ;;
            esac
            COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
            return 0
            ;;
        clap_issue_2009__help)
            opts=" -h -V  --help --version  "
            if [[ ${cur} == -* || ${COMP_CWORD} -eq 2 ]] ; then
                COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
                return 0
            fi
            case "${prev}" in

                *)
                    COMPREPLY=()
                    ;;
            esac
            COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
            return 0
            ;;
    esac
}

complete -F _clap_issue_2009 -o bashdefault -o default clap_issue_2009
```

This <s>might have already been</s> is fixed on master.

---

_Closed by @ldm0 on 2021-03-11 07:23_

---

_Comment by @regexident on 2021-03-27 19:03_

Any chance for cutting a beta release with this included? It's currently a blocker for several projects of mine.

---

_Comment by @pksunkara on 2021-03-30 19:13_

I don't think so. We are breaking quite a few things with every commit and I don't think releasing a beta right now would be a sensible idea. I am aiming to do an RC soon™️

---

_Comment by @regexident on 2021-03-30 21:32_

The sound you just heard is structopt's sigh of relief. 😄 #notdeadjustyet

---

---
number: 4822
title: bash generated completions invalid when command name contains spaces
type: issue
state: open
author: mjpieters
labels:
  - C-bug
  - M-breaking-change
  - A-builder
assignees: []
created_at: 2023-04-03T19:05:00Z
updated_at: 2023-11-12T04:03:57Z
url: https://github.com/clap-rs/clap/issues/4822
synced_at: 2026-01-10T01:28:02Z
---

# bash generated completions invalid when command name contains spaces

---

_Issue opened by @mjpieters on 2023-04-03 19:05_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.67.0 (fc594f156 2023-01-24)

### Clap Version

4.2.1

### Minimal reproducible code

**`Cargo.toml`**

```toml
[package]
name = "mcre_clap_generate"
version = "0.1.0"
edition = "2021"
[dependencies]
clap = { version = "4.2.1", features = ["derive"] }
clap_complete = "4.2.0"
```

**`main.rs`**:

```rust
use std::path::PathBuf;
use clap::{CommandFactory, Parser, ValueHint, error::Result};
use clap_complete::{generate_to, shells};

#[derive(Parser)]
pub enum SubCommand {
    Completions {
        /// The output directory to which the file should be written.
        #[arg(value_hint = ValueHint::DirPath)]
        output_directory: PathBuf,
    },
}

#[derive(Parser)]
#[command(
    name = "MCRE for clap generate output",
    about = "This is minimal, complete reproducible example for clap generate output producing invalid Bash completions",
)]
struct Cli {
    #[command(subcommand)]
    pub cmd: Option<SubCommand>,
}

fn main() -> Result<()> {
    let opt = Cli::parse();
    if let Some(SubCommand::Completions {
        output_directory,
    }) = &opt.cmd
    {
        generate_to(shells::Bash, &mut Cli::command(), "mcre_clap_generate", output_directory)?;
        generate_to(shells::Zsh, &mut Cli::command(), "mcre_clap_generate", output_directory)?;
    }
    Ok(())
}
```


### Steps to reproduce the bug with the above code

```
$ cargo build
...
$ mkdir -p /tmp/sample_output; ./target/debug/mcre_clap_generate completions /tmp/sample_output
```

### Actual Behaviour

The generated bash and zsh completions files are not valid, because either generator uses `cmd.get_name()` instead of `cmd.get_bin_name()` as the parent command.

You can see this most easily when trying to execute the generated bash output:

```sh
$ bash /tmp/sample_output/mcre_clap_generate.bash
/tmp/sample_output/mcre_clap_generate.bash: line 15: syntax error near unexpected token `for'
/tmp/sample_output/mcre_clap_generate.bash: line 15: `            MCRE for clap generate output,completions)'
$ head -n30 /tmp/sample_output/mcre_clap_generate.bash
_mcre_clap_generate() {
    local i cur prev opts cmd
    COMPREPLY=()
    cur="${COMP_WORDS[COMP_CWORD]}"
    prev="${COMP_WORDS[COMP_CWORD-1]}"
    cmd=""
    opts=""

    for i in ${COMP_WORDS[@]}
    do
        case "${cmd},${i}" in
            ",$1")
                cmd="mcre_clap_generate"
                ;;
            MCRE for clap generate output,completions)
                cmd="MCRE for clap generate output__completions"
                ;;
            MCRE for clap generate output,help)
                cmd="MCRE for clap generate output__help"
                ;;
            MCRE for clap generate output__help,completions)
                cmd="MCRE for clap generate output__help__completions"
                ;;
            MCRE for clap generate output__help,help)
                cmd="MCRE for clap generate output__help__help"
                ;;
            *)
                ;;
        esac
    done
```

The Zsh generated output is similarly broken; using the `shellcheck` linter:

```
$ shellcheck /tmp/sample_output/_mcre_clap_generate 

In /tmp/sample_output/_mcre_clap_generate line 23:
    case $state in
    ^-- SC1009 (info): The mentioned syntax error was in this case expression.


In /tmp/sample_output/_mcre_clap_generate line 24:
    (MCRE for clap generate output)
    ^-- SC1073 (error): Couldn't parse this case item. Fix to allow more checks.
          ^-- SC1072 (error): Expected ) to open a new case item. Fix any mentioned problems and try again.
          ^-- SC1085 (error): Did you forget to move the ;; after extending this case item?

For more information:
  https://www.shellcheck.net/wiki/SC1085 -- Did you forget to move the ;; aft...
  https://www.shellcheck.net/wiki/SC1072 -- Expected ) to open a new case ite...
  https://www.shellcheck.net/wiki/SC1073 -- Couldn't parse this case item. Fi...
```

### Expected Behaviour

The parent name for a completion subcommand should be `cmd.get_bin_name()` (in the MRCE: `mrce_clap_generate`), and not `cmd.get_name()` (`MCRE for clap generate output` in the MRCE).

If I manually replace `MCRE for clap generate output` with `mcre_clap_generate` in the generated output, the completions work as expected.

### Additional Context

_No response_

### Debug Output

```
[clap_builder::builder::command]        Command::_do_parse
[clap_builder::builder::command]        Command::_build: name="MCRE for clap generate output"
[clap_builder::builder::command]        Command::_propagate:MCRE for clap generate output
[clap_builder::builder::command]        Command::_check_help_and_version:MCRE for clap generate output expand_help_tree=false
[clap_builder::builder::command]        Command::long_help_exists
[clap_builder::builder::command]        Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]        Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]        Command::_propagate_global_args:MCRE for clap generate output
[clap_builder::builder::debug_asserts]  Command::_debug_asserts
[clap_builder::builder::debug_asserts]  Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]  Command::_verify_positionals
[clap_builder::parser::parser]  Parser::get_matches_with
[clap_builder::parser::parser]  Parser::get_matches_with: Begin parsing '"completions"'
[clap_builder::parser::parser]  Parser::possible_subcommand: arg=Ok("completions")
[clap_builder::parser::parser]  Parser::get_matches_with: sc=Some("completions")
[clap_builder::parser::parser]  Parser::parse_subcommand
[ clap_builder::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]  Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]  Usage::get_required_usage_from: ret_val=[]
[clap_builder::builder::command]        Command::_build_subcommand Setting bin_name of completions to "mcre_clap_generate completions"
[clap_builder::builder::command]        Command::_build_subcommand Setting display_name of completions to "MCRE for clap generate output-completions"
[clap_builder::builder::command]        Command::_build: name="completions"
[clap_builder::builder::command]        Command::_propagate:completions
[clap_builder::builder::command]        Command::_check_help_and_version:completions expand_help_tree=false
[clap_builder::builder::command]        Command::long_help_exists
[clap_builder::builder::command]        Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]        Command::_propagate_global_args:completions
[clap_builder::builder::debug_asserts]  Command::_debug_asserts
[clap_builder::builder::debug_asserts]  Arg::_debug_asserts:output_directory
[clap_builder::builder::debug_asserts]  Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]  Command::_verify_positionals
[clap_builder::parser::parser]  Parser::parse_subcommand: About to parse sc=completions
[clap_builder::parser::parser]  Parser::get_matches_with
[clap_builder::parser::parser]  Parser::get_matches_with: Begin parsing '"/tmp/sample_output"'
[clap_builder::parser::parser]  Parser::possible_subcommand: arg=Ok("/tmp/sample_output")
[clap_builder::parser::parser]  Parser::get_matches_with: sc=None
[clap_builder::parser::parser]  Parser::get_matches_with: Positional counter...1
[clap_builder::parser::parser]  Parser::get_matches_with: Low index multiples...false
[clap_builder::parser::parser]  Parser::resolve_pending: id="output_directory"
[clap_builder::parser::parser]  Parser::react action=Set, identifier=Some(Index), source=CommandLine
[clap_builder::parser::parser]  Parser::remove_overrides: id="output_directory"
[clap_builder::parser::arg_matcher]     ArgMatcher::start_custom_arg: id="output_directory", source=CommandLine
[clap_builder::builder::command]        Command::groups_for_arg: id="output_directory"
[clap_builder::parser::arg_matcher]     ArgMatcher::start_custom_arg: id="Completions", source=CommandLine
[clap_builder::parser::parser]  Parser::push_arg_values: ["/tmp/sample_output"]
[clap_builder::parser::parser]  Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::parser::arg_matcher]     ArgMatcher::needs_more_vals: o=output_directory, pending=0
[clap_builder::parser::arg_matcher]     ArgMatcher::needs_more_vals: expected=1, actual=0
[clap_builder::parser::parser]  Parser::react not enough values passed in, leaving it to the validator to complain
[clap_builder::parser::parser]  Parser::add_defaults
[clap_builder::parser::parser]  Parser::add_defaults:iter:output_directory:
[clap_builder::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]  Parser::add_default_value:iter:output_directory: doesn't have default vals
[clap_builder::parser::parser]  Parser::add_defaults:iter:help:
[clap_builder::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]  Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]       Validator::validate
[clap_builder::builder::command]        Command::groups_for_arg: id="output_directory"
[clap_builder::parser::validator]       Conflicts::gather_direct_conflicts id="output_directory", conflicts=[]
[clap_builder::parser::validator]       Conflicts::gather_direct_conflicts id="Completions", conflicts=[]
[clap_builder::parser::validator]       Validator::validate_conflicts
[clap_builder::parser::validator]       Validator::validate_exclusive
[clap_builder::parser::validator]       Validator::validate_conflicts::iter: id="output_directory"
[clap_builder::parser::validator]       Conflicts::gather_conflicts: arg="output_directory"
[clap_builder::parser::validator]       Conflicts::gather_conflicts: conflicts=[]
[clap_builder::parser::validator]       Validator::validate_required: required=ChildGraph([Child { id: "output_directory", children: [] }])
[clap_builder::parser::validator]       Validator::gather_requires
[clap_builder::parser::validator]       Validator::gather_requires:iter:"output_directory"
[clap_builder::parser::validator]       Validator::gather_requires:iter:"Completions"
[clap_builder::parser::validator]       Validator::gather_requires:iter:"Completions":group
[clap_builder::parser::validator]       Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::parser]  Parser::add_defaults
[clap_builder::parser::parser]  Parser::add_defaults:iter:help:
[clap_builder::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]  Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]       Validator::validate
[clap_builder::parser::validator]       Validator::validate_conflicts
[clap_builder::parser::validator]       Validator::validate_exclusive
[clap_builder::parser::validator]       Validator::validate_required: required=ChildGraph([])
[clap_builder::parser::validator]       Validator::gather_requires
[clap_builder::parser::validator]       Validator::validate_required: is_exclusive_present=false
[clap_builder::parser::arg_matcher]     ArgMatcher::get_global_values: global_arg_vec=[]
[clap_builder::builder::command]        Command::_build: name="MCRE for clap generate output"
[clap_builder::builder::command]        Command::_propagate:MCRE for clap generate output
[clap_builder::builder::command]        Command::_check_help_and_version:MCRE for clap generate output expand_help_tree=true
[clap_builder::builder::command]        Command::long_help_exists
[clap_builder::builder::command]        Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]        Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]        Command::_propagate_global_args:MCRE for clap generate output
[clap_builder::builder::debug_asserts]  Command::_debug_asserts
[clap_builder::builder::debug_asserts]  Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]  Command::_verify_positionals
[clap_builder::builder::command]        Command::_build: name="completions"
[clap_builder::builder::command]        Command::_propagate:completions
[clap_builder::builder::command]        Command::_check_help_and_version:completions expand_help_tree=true
[clap_builder::builder::command]        Command::long_help_exists
[clap_builder::builder::command]        Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]        Command::_propagate_global_args:completions
[clap_builder::builder::debug_asserts]  Command::_debug_asserts
[clap_builder::builder::debug_asserts]  Arg::_debug_asserts:output_directory
[clap_builder::builder::debug_asserts]  Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]  Command::_verify_positionals
[clap_builder::builder::command]        Command::_build: name="help"
[clap_builder::builder::command]        Command::_propagate:help
[clap_builder::builder::command]        Command::_check_help_and_version:help expand_help_tree=true
[clap_builder::builder::command]        Command::long_help_exists
[clap_builder::builder::command]        Command::_propagate_global_args:help
[clap_builder::builder::debug_asserts]  Command::_debug_asserts
[clap_builder::builder::debug_asserts]  Command::_verify_positionals
[clap_builder::builder::command]        Command::_build: name="completions"
[clap_builder::builder::command]        Command::_propagate:completions
[clap_builder::builder::command]        Command::_check_help_and_version:completions expand_help_tree=true
[clap_builder::builder::command]        Command::long_help_exists
[clap_builder::builder::command]        Command::_propagate_global_args:completions
[clap_builder::builder::debug_asserts]  Command::_debug_asserts
[clap_builder::builder::debug_asserts]  Command::_verify_positionals
[clap_builder::builder::command]        Command::_build: name="help"
[clap_builder::builder::command]        Command::_propagate:help
[clap_builder::builder::command]        Command::_check_help_and_version:help expand_help_tree=true
[clap_builder::builder::command]        Command::long_help_exists
[clap_builder::builder::command]        Command::_propagate_global_args:help
[clap_builder::builder::debug_asserts]  Command::_debug_asserts
[clap_builder::builder::debug_asserts]  Command::_verify_positionals
[clap_builder::builder::command]        Command::_build_bin_names
[ clap_builder::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]  Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]  Usage::get_required_usage_from: ret_val=[]
[clap_builder::builder::command]        Command::_build_bin_names:iter: bin_name set...
[clap_builder::builder::command]        Command::_build_bin_names:iter: Setting usage_name of completions to "mcre_clap_generate completions"
[clap_builder::builder::command]        Command::_build_bin_names:iter: Setting bin_name of completions to "mcre_clap_generate completions"
[clap_builder::builder::command]        Command::_build_bin_names:iter: Setting display_name of completions to "MCRE for clap generate output-completions"
[clap_builder::builder::command]        Command::_build_bin_names
[ clap_builder::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]  Usage::get_required_usage_from: unrolled_reqs=["output_directory"]
[ clap_builder::output::usage]  Usage::get_required_usage_from:iter:"output_directory" arg is_present=false
[ clap_builder::output::usage]  Usage::get_required_usage_from: ret_val=[StyledStr("<OUTPUT_DIRECTORY>")]
[clap_builder::builder::command]        Command::_build_bin_names:iter: bin_name set...
[clap_builder::builder::command]        Command::_build_bin_names:iter: Setting usage_name of help to "mcre_clap_generate help"
[clap_builder::builder::command]        Command::_build_bin_names:iter: Setting bin_name of help to "mcre_clap_generate help"
[clap_builder::builder::command]        Command::_build_bin_names:iter: Setting display_name of help to "MCRE for clap generate output-help"
[clap_builder::builder::command]        Command::_build_bin_names
[ clap_builder::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]  Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]  Usage::get_required_usage_from: ret_val=[]
[clap_builder::builder::command]        Command::_build_bin_names:iter: bin_name set...
[clap_builder::builder::command]        Command::_build_bin_names:iter: Setting usage_name of completions to "mcre_clap_generate help completions"
[clap_builder::builder::command]        Command::_build_bin_names:iter: Setting bin_name of completions to "mcre_clap_generate help completions"
[clap_builder::builder::command]        Command::_build_bin_names:iter: Setting display_name of completions to "MCRE for clap generate output-help-completions"
[clap_builder::builder::command]        Command::_build_bin_names
[ clap_builder::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]  Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]  Usage::get_required_usage_from: ret_val=[]
[clap_builder::builder::command]        Command::_build_bin_names:iter: bin_name set...
[clap_builder::builder::command]        Command::_build_bin_names:iter: Setting usage_name of help to "mcre_clap_generate help help"
[clap_builder::builder::command]        Command::_build_bin_names:iter: Setting bin_name of help to "mcre_clap_generate help help"
[clap_builder::builder::command]        Command::_build_bin_names:iter: Setting display_name of help to "MCRE for clap generate output-help-help"
[clap_builder::builder::command]        Command::_build_bin_names
[ clap_builder::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]  Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]  Usage::get_required_usage_from: ret_val=[]
```

---

_Label `C-bug` added by @mjpieters on 2023-04-03 19:05_

---

_Comment by @epage on 2023-04-03 19:40_

`bin_name` would not work for this
- Its for usage and includes all parents
- It can be affected by what the current process is called

What we should do is add an assert that `name` does not have a space.  

---

_Label `M-breaking-change` added by @epage on 2023-04-03 19:40_

---

_Label `A-builder` added by @epage on 2023-04-03 19:40_

---

_Added to milestone `5.0` by @epage on 2023-04-03 19:40_

---

_Comment by @mjpieters on 2023-04-03 19:48_

> `bin_name` would not work for this
> 
>    * Its for usage and includes all parents
>    * It can be affected by what the current process is called

Then why is the second half of the generated output using `get_bin_name()`, via `utils::subcommands()`? Because *those* are valid:

```
$ tail -n75 /tmp/sample_output/mcre_clap_generate.bash
    case "${cmd}" in
        mcre_clap_generate)
            opts="-h --help completions help"
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
        mcre_clap_generate__completions)
            opts="-h --help <OUTPUT_DIRECTORY>"
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
        mcre_clap_generate__help)
            opts="completions help"
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
        mcre_clap_generate__help__completions)
            opts=""
            if [[ ${cur} == -* || ${COMP_CWORD} -eq 3 ]] ; then
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
        mcre_clap_generate__help__help)
            opts=""
            if [[ ${cur} == -* || ${COMP_CWORD} -eq 3 ]] ; then
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
```



---

_Comment by @mjpieters on 2023-04-03 20:07_

I've understood the structure of the bash completion function to be roughly this:

```
    for i in ${COMP_WORDS[@]}
    do
        case "${cmd},${i}" in
            # expand cmd to cmd__possible_part2_word
        esac
    done

    case "${cmd}" in
        # provide completion words for each group and subcommand
    esac
```

The first part is generated by `shells::bash::Bash::all_subcommands()`, the second part mostly by `shells::bash::Bash::subcommand_details()` (the first option uses `cmd.get_bin_name()`). The latter uses `crate::generator::utils::all_subcommands()`, a method that produces a vector of `vec![sc.get_name().to_string(), sc.get_bin_name().unwrap().to_string()]` two-value vectors, and the details are generated using only the second element from those (so `sc.get_bin_name().unwrap().to_string()`).

Hence my assumption that the first part should also use those exact same strings.

---

_Referenced in [Nukesor/pueue#426](../../Nukesor/pueue/issues/426.md) on 2023-04-03 20:52_

---

_Comment by @tv42 on 2023-11-12 04:02_

I triggered something very similar by having a crate named `foo_bar` (because `foo` was taken), with the binary in `src/bin/foo/main.rs` and thus named simply `foo`. I called `clap_complete::generate("bash", &mut command, "foo", &mut std::io::stdout());` and the completion looked like

```
    for i in ${COMP_WORDS[@]}
    do
        case "${cmd},${i}" in
            ",$1")
                cmd="foo"
                ;;
            foo__bar,quux)
                cmd="foo__bar__quux"
                ;;
```

Once I added `#[command(name = "foo")]` into my derived `clap::Parser` struct, the completion script switched from `foo__bar` to `foo`.

---

---
number: 5372
title: "clap_complete: always fail to generate into a tempfile"
type: issue
state: closed
author: poliorcetics
labels:
  - C-bug
assignees: []
created_at: 2024-02-23T02:21:52Z
updated_at: 2024-03-11T23:10:19Z
url: https://github.com/clap-rs/clap/issues/5372
synced_at: 2026-01-10T01:28:11Z
---

# clap_complete: always fail to generate into a tempfile

---

_Issue opened by @poliorcetics on 2024-02-23 02:21_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.76.0 (07dca489a 2024-02-04)

### Clap Version

4.5.1

### Minimal reproducible code

```toml
[package]
name = "clap-repro"
version = "0.1.0"
edition = "2021"

[dependencies]
clap = "4.5.1"
clap_complete = "4.5.1"
tempfile = "3.10.0"

```

```rust
use std::io::{Read, Write};

use clap::{Arg, ArgAction, Command};

fn main() {
    let mut app = app();

    println!("——————————————————————— Complete to stdout");

    clap_complete::generate(
        clap_complete::shells::Bash,
        &mut app,
        "just",
        &mut std::io::stdout(),
    );

    println!("——————————————————————— DONE");
    println!("——————————————————————— Complete to file");

    let mut file = tempfile::tempfile().unwrap();
    file.write_all(b"").unwrap(); // Try with file creation ?
    clap_complete::generate(clap_complete::shells::Bash, &mut app, "just", &mut file);
    file.flush().unwrap(); // Try with flushing ?
    let mut buffer = String::new();
    file.read_to_string(&mut buffer).unwrap();
    println!("{buffer}");
    println!("——————————————————————— DONE");
}

fn app() -> Command {
    Command::new(env!("CARGO_PKG_NAME")).arg(
        Arg::new("check")
            .long("check")
            .action(ArgAction::SetTrue)
            .help("Help for checl"),
    )
}
```


### Steps to reproduce the bug with the above code

`cargo run` produces:

<details><summary>Details</summary>
<p>

```
——————————————————————— Complete to stdout
_just() {
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
                cmd="just"
                ;;
            *)
                ;;
        esac
    done

    case "${cmd}" in
        just)
            opts="-h --check --help"
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
    esac
}

if [[ "${BASH_VERSINFO[0]}" -eq 4 && "${BASH_VERSINFO[1]}" -ge 4 || "${BASH_VERSINFO[0]}" -gt 4 ]]; then
    complete -F _just -o nosort -o bashdefault -o default just
else
    complete -F _just -o bashdefault -o default just
fi
——————————————————————— DONE
——————————————————————— Complete to file

——————————————————————— DONE
```

</p>
</details> 

### Actual Behaviour

- Generating to `stdout` works
- Generating to `file` does not works

### Expected Behaviour

Both should work

### Additional Context

Found while upgrading `just` to `clap 4` in https://github.com/casey/just/pull/1924

### Debug Output

with both `clap` and `clap_complete` in `debug`:

<details><summary>Details</summary>
<p>

```
——————————————————————— Complete to stdout
[clap_builder::builder::command]Command::_build: name="clap-repro"
[clap_builder::builder::command]Command::_propagate:clap-repro
[clap_builder::builder::command]Command::_check_help_and_version:clap-repro expand_help_tree=true
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:clap-repro
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:check
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::builder::command]Command::_build_bin_names
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
[ clap_complete::shells::bash] 	all_options_for_path: path=just
[clap_complete::generator::utils] 	shorts: name=clap-repro
[clap_complete::generator::utils] 	longs: name=clap-repro
[clap_complete::generator::utils] 	subcommands: name=clap-repro
[clap_complete::generator::utils] 	subcommands: Has subcommands...false
[ clap_complete::shells::bash] 	option_details_for_path: path=just
[ clap_complete::shells::bash] 	all_subcommands
[ clap_complete::shells::bash] 	subcommand_details
[clap_complete::generator::utils] 	subcommands: name=clap-repro
[clap_complete::generator::utils] 	subcommands: Has subcommands...false
_just() {
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
                cmd="just"
                ;;
            *)
                ;;
        esac
    done

    case "${cmd}" in
        just)
            opts="-h --check --help"
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
    esac
}

if [[ "${BASH_VERSINFO[0]}" -eq 4 && "${BASH_VERSINFO[1]}" -ge 4 || "${BASH_VERSINFO[0]}" -gt 4 ]]; then
    complete -F _just -o nosort -o bashdefault -o default just
else
    complete -F _just -o bashdefault -o default just
fi
——————————————————————— DONE
——————————————————————— Complete to file
[clap_builder::builder::command]Command::_build: name="clap-repro"
[clap_builder::builder::command]Command::_build: already built
[clap_builder::builder::command]Command::_build_bin_names
[clap_builder::builder::command]Command::_build_bin_names: already built
[ clap_complete::shells::bash] 	all_options_for_path: path=just
[clap_complete::generator::utils] 	shorts: name=clap-repro
[clap_complete::generator::utils] 	longs: name=clap-repro
[clap_complete::generator::utils] 	subcommands: name=clap-repro
[clap_complete::generator::utils] 	subcommands: Has subcommands...false
[ clap_complete::shells::bash] 	option_details_for_path: path=just
[ clap_complete::shells::bash] 	all_subcommands
[ clap_complete::shells::bash] 	subcommand_details
[clap_complete::generator::utils] 	subcommands: name=clap-repro
[clap_complete::generator::utils] 	subcommands: Has subcommands...false

——————————————————————— DONE
```

</p>
</details> 

---

_Label `C-bug` added by @poliorcetics on 2024-02-23 02:21_

---

_Comment by @epage on 2024-02-23 03:02_

FYI I just tried this with v3 and I got the same result:
```rust
#!/usr/bin/env nargo
---
[dependencies]
clap = "3"
clap_complete = "3"
tempfile = "3.10.0"
---

use std::io::{Read, Write};

use clap::{Arg, ArgAction, Command};

fn main() {
    let mut app = app();

    println!("——————————————————————— Complete to stdout");
    clap_complete::generate(
        clap_complete::shells::Bash,
        &mut app,
        "just",
        &mut std::io::stdout(),
    );
    println!("——————————————————————— DONE");

    println!("——————————————————————— Complete to file");
    let mut file = tempfile::tempfile().unwrap();
    file.write_all(b"").unwrap(); // Try with file creation ?
    clap_complete::generate(clap_complete::shells::Bash, &mut app, "just", &mut file);
    file.flush().unwrap(); // Try with flushing ?
    let mut buffer = String::new();
    file.read_to_string(&mut buffer).unwrap();
    println!("{buffer}");
    println!("——————————————————————— DONE");
}

fn app() -> Command<'static> {
    Command::new(env!("CARGO_PKG_NAME")).arg(
        Arg::new("check")
            .long("check")
            .action(ArgAction::SetTrue)
            .help("Help for checl"),
    )
}
```

I even made it independent of clap
```
    println!("——————————————————————— Complete to file");
    let mut file = tempfile::tempfile().unwrap();
    file.write_all(b"hello").unwrap(); // Try with file creation ?
    file.flush().unwrap(); // Try with flushing ?
    let mut buffer = String::new();
    file.read_to_string(&mut buffer).unwrap();
    println!("{buffer}");
    println!("——————————————————————— DONE");
```
and I still got no output.

---

_Comment by @poliorcetics on 2024-02-23 17:30_

I tried with `let mut file = File::options().create(true).read(true).write(true).truncate(true).open("just-completions").unwrap()` and still got nothing so I'm thinking `tempfile` is not responsible here

---

_Referenced in [casey/just#1924](../../casey/just/pulls/1924.md) on 2024-02-26 08:57_

---

_Comment by @poliorcetics on 2024-03-11 23:10_

Found the bug, it's not clap-complete or tempfile, it's me being bad at files: I need to use `file.rewind().unwrap();` before reading the written file, else I'm reading from the end of the file so there is nothing left to read ... 

---

_Closed by @poliorcetics on 2024-03-11 23:10_

---

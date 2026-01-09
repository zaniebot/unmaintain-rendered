---
number: 3022
title: "clap_generate: zsh broken with two multi length arguments"
type: issue
state: closed
author: Morganamilo
labels:
  - C-bug
  - E-medium
  - A-completion
assignees: []
created_at: 2021-11-13T22:47:09Z
updated_at: 2023-02-23T17:55:20Z
url: https://github.com/clap-rs/clap/issues/3022
synced_at: 2026-01-07T13:12:19-06:00
---

# clap_generate: zsh broken with two multi length arguments

---

_Issue opened by @Morganamilo on 2021-11-13 22:47_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.58.0-nightly (8b09ba6a5 2021-11-09)

### Clap Version

3.0.0-beta.5

### Minimal reproducible code

```rust
use clap::{App, IntoApp, Parser};
use clap_generate::{generate, Shell};
use std::io::stdout;

#[derive(Parser)]
pub struct Args {
    pub targets: Vec<String>,
    #[clap(required = true, raw = true)]
    pub files: Vec<String>,
}

fn main() {
    let mut app: App = Args::into_app();
    generate(Shell::Zsh, &mut app, "bug", &mut stdout());
}
```

### Steps to reproduce the bug with the above code

With the completion installed:

```
% command <tab>
_arguments:comparguments:325: doubled rest argument definition: *::file -- Files to search for:
```


### Actual Behaviour

So I have a program that has a usage like this: `program: <targets>... -- <files>...`

The zsh completion seems to really not like this and spits out an error when tab is hit:

`_arguments:comparguments:325: doubled rest argument definition: *::file -- Files to search for:`

The completion generated is:

```sh
#compdef bug

autoload -U is-at-least

_bug() {
    typeset -A opt_args
    typeset -a _arguments_options
    local ret=1

    if is-at-least 5.2; then
        _arguments_options=(-s -S -C)
    else
        _arguments_options=(-s -C)
    fi

    local context curcontext="$curcontext" state line
    _arguments "${_arguments_options[@]}" \
'-h[Print help information]' \
'--help[Print help information]' \
'*::targets:' \
'*::files:' \
&& ret=0
}

(( $+functions[_bug_commands] )) ||
_bug_commands() {
    local commands; commands=()
    _describe -t commands 'bug commands' commands "$@"
}

_bug "$@"%
```

### Expected Behaviour

Don't be broken

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `T: bug` added by @Morganamilo on 2021-11-13 22:47_

---

_Label `C: generator` added by @pksunkara on 2021-11-14 00:33_

---

_Comment by @epage on 2021-11-14 01:52_

Thanks for reporting this!

One challenge with completions is we have to effectively re-implement clap's argument parsing for each shell we support.  Example of other issues that look like they stem from this:
- https://github.com/clap-rs/clap/issues/2729
- https://github.com/clap-rs/clap/issues/2750
- https://github.com/clap-rs/clap/issues/2145
- https://github.com/clap-rs/clap/issues/1822
- https://github.com/clap-rs/clap/issues/1764

This is making me think that https://github.com/clap-rs/clap/issues/1232 is even more important so we can share parsing logic between different shells *and* clap and more easily test it.

---

_Comment by @pksunkara on 2021-11-14 02:41_

To be more explicitly clear, it's not the parsing logic we need to share but rather the parsing requirements.

---

_Referenced in [clap-rs/clap#1232](../../clap-rs/clap/issues/1232.md) on 2021-11-15 14:31_

---

_Referenced in [epage/clapng#238](../../epage/clapng/issues/238.md) on 2021-12-06 22:24_

---

_Referenced in [clap-rs/clap#3166](../../clap-rs/clap/issues/3166.md) on 2021-12-13 17:55_

---

_Label `E-medium` added by @epage on 2021-12-13 22:13_

---

_Referenced in [denoland/deno#13702](../../denoland/deno/issues/13702.md) on 2022-02-17 13:27_

---

_Referenced in [clap-rs/clap#3508](../../clap-rs/clap/issues/3508.md) on 2022-04-24 14:34_

---

_Comment by @drahnr on 2022-04-24 15:08_

The same issue arises with a more common pattern too (see https://github.com/drahnr/clap-completion-zoink/blob/84a3eb106309d066e80645f60a3065525d18c5a4 for a full working example):

```
#[derive(Debug, PartialEq, Eq, clap::Parser)]
#[clap(rename_all = "kebab-case")]
struct Common {
    pub paths: Vec<PathBuf>,
}

#[derive(Debug, PartialEq, Eq, clap::Subcommand)]
#[clap(rename_all = "kebab-case")]
enum Sub {
    Comp {
        #[clap(long)]
        shell: Shell,
    },
// imagine more subcommands here
}

#[derive(clap::Parser, Debug)]
#[clap(author, version, about, long_about = None)]
#[clap(rename_all = "kebab-case")]
struct Args {
    #[clap(flatten)]
    pub common: Common,

    #[clap(subcommand)]
    pub command: Sub,
}
```

On tab completion results in 

```
./target/debug/clap-completion-zoink 
_arguments:comparguments:325: doubled rest argument definition: *::: :->clap-completion-zoink
``` 

```
#compdef clap-completion-zoink

autoload -U is-at-least

_clap-completion-zoink() {
    typeset -A opt_args
    typeset -a _arguments_options
    local ret=1

    if is-at-least 5.2; then
        _arguments_options=(-s -S -C)
    else
        _arguments_options=(-s -C)
    fi

    local context curcontext="$curcontext" state line
    _arguments "${_arguments_options[@]}" \
'-h[Print help information]' \
'--help[Print help information]' \
'-V[Print version information]' \
'--version[Print version information]' \
'*::paths:' \
":: :_clap-completion-zoink_commands" \
"*::: :->clap-completion-zoink" \
&& ret=0
    case $state in
    (clap-completion-zoink)
        words=($line[2] "${words[@]}")
        (( CURRENT += 1 ))
        curcontext="${curcontext%:*:*}:clap-completion-zoink-command-$line[2]:"
        case $line[2] in
            (comp)
_arguments "${_arguments_options[@]}" \
'--shell=[]:SHELL: ' \
'-h[Print help information]' \
'--help[Print help information]' \
&& ret=0
;;
(help)
_arguments "${_arguments_options[@]}" \
'*::subcommand -- The subcommand whose help message to display:' \
&& ret=0
;;
        esac
    ;;
esac
}

(( $+functions[_clap-completion-zoink_commands] )) ||
_clap-completion-zoink_commands() {
    local commands; commands=(
'comp:' \
'help:Print this message or the help of the given subcommand(s)' \
    )
    _describe -t commands 'clap-completion-zoink commands' commands "$@"
}
(( $+functions[_clap-completion-zoink__comp_commands] )) ||
_clap-completion-zoink__comp_commands() {
    local commands; commands=()
    _describe -t commands 'clap-completion-zoink comp commands' commands "$@"
}
(( $+functions[_clap-completion-zoink__help_commands] )) ||
_clap-completion-zoink__help_commands() {
    local commands; commands=()
    _describe -t commands 'clap-completion-zoink help commands' commands "$@"
}

_clap-completion-zoink "$@"
```

---

_Comment by @TD-Sky on 2022-11-10 08:39_

I meet the same bug.

clap version:
- clap-4.0.22
- clap_complete-4.0.5

The `Cli`: [cli.rs](https://github.com/TD-Sky/conceal/blob/zsh-complete/src/cli.rs)

The code to generate completions: [build.rs](https://github.com/TD-Sky/conceal/blob/zsh-complete/build.rs)

The zsh completion script: [_cnc.rs](https://github.com/TD-Sky/conceal/blob/zsh-complete/completions/_cnc)

## Error

```zsh
_arguments:comparguments:327: doubled rest argument definition: *::: :->cnc
```

---

_Referenced in [oberblastmeister/trashy#57](../../oberblastmeister/trashy/issues/57.md) on 2022-11-27 09:25_

---

_Referenced in [garden-rs/garden#10](../../garden-rs/garden/issues/10.md) on 2023-01-08 04:22_

---

_Referenced in [clap-rs/clap#4612](../../clap-rs/clap/pulls/4612.md) on 2023-01-09 10:58_

---

_Comment by @davvid on 2023-01-09 11:01_

I believe https://github.com/clap-rs/clap/pull/4612 may resolve this issue. I ran into this while creating https://github.com/davvid/garden so I figured we might as well fix it (rather than [workaround it](https://davvid.github.io/garden/commands.html#zsh)).

Kindly test it out and let me know if it works for y'all as well.

---

_Comment by @TD-Sky on 2023-01-11 07:48_

> I believe #4612 may resolve this issue. I ran into this while creating https://github.com/davvid/garden so I figured we might as well fix it (rather than [workaround it](https://davvid.github.io/garden/commands.html#zsh)).
> 
> Kindly test it out and let me know if it works for y'all as well.

Have you not fully fixed this feature yet? The completion doesn't break now, but it also does nothing.

Here are the Cargo.toml and the completion script: https://gist.github.com/TD-Sky/59eb28d92f8dae38129182e66da6b00c

---

_Comment by @davvid on 2023-01-13 06:56_

@TD-Sky I spent some minutes trying to compile your project and looking into what might be u. It doesn't look like the project you shared (conceal) compiles in the two branches I tried -- main and zsh-complete.

If you have a working branch that that is made worse by these changes then please share the details.

Feel free to take a look at how I'm setting up the subcommands but we probably shouldn't clutter this issue with too many project-specific details. Anyways, here's some working examples if you want to see them:

This is the top-level parser:
https://github.com/davvid/garden/blob/main/src/cli.rs#L50

And this is where the sub-commands are defined:
https://github.com/davvid/garden/blob/main/src/cli.rs#L94

---

_Comment by @TD-Sky on 2023-01-16 08:34_

> @TD-Sky I spent some minutes trying to compile your project and looking into what might be u. It doesn't look like the project you shared (conceal) compiles in the two branches I tried -- main and zsh-complete.
> 
> If you have a working branch that that is made worse by these changes then please share the details.
> 
> Feel free to take a look at how I'm setting up the subcommands but we probably shouldn't clutter this issue with too many project-specific details. Anyways, here's some working examples if you want to see them:
> 
> This is the top-level parser: https://github.com/davvid/garden/blob/main/src/cli.rs#L50
> 
> And this is where the sub-commands are defined: https://github.com/davvid/garden/blob/main/src/cli.rs#L94

I have tried to compile with your patches in the local part of `conceal` branch `zsh-complete` and shared the Cargo.toml and completion script on gist. Should I push the patched version so you can try again?

I have read the code your arguments parser. But you don't use `flatten` as I did. Is it possible there are two different bugs cause your patches don't work for me?

---

_Closed by @epage on 2023-02-23 17:55_

---

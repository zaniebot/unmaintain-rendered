---
number: 2485
title: clap generates invalid bash completions for ArgEnum
type: issue
state: closed
author: ajeetdsouza
labels:
  - C-bug
  - A-completion
  - ":money_with_wings: $5"
assignees: []
created_at: 2021-05-20T05:18:03Z
updated_at: 2021-10-14T21:42:35Z
url: https://github.com/clap-rs/clap/issues/2485
synced_at: 2026-01-10T01:27:19Z
---

# clap generates invalid bash completions for ArgEnum

---

_Issue opened by @ajeetdsouza on 2021-05-20 05:18_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.52.1 (9bc8c42bb 2021-05-09)

### Clap Version

3.0.0-beta.2

### Minimal reproducible code

```rust
use clap::{ArgEnum, Clap, IntoApp};

#[derive(Clap, Debug)]
pub struct App {
    #[clap(arg_enum)]
    pub shell: Shell,
}

#[derive(ArgEnum, Debug)]
pub enum Shell {
    Bash,
    Fish,
    Zsh,
}

fn main() {
    let mut app = App::into_app();
    clap_generate::generate::<clap_generate::generators::Bash, _>(
        &mut app,
        "play",
        &mut std::io::stdout(),
    );
}
```

### Steps to reproduce the bug with the above code

```shell
cargo run
```

### Actual Behaviour

Assuming a binary name of `foo`, if I type in `foo`<kbd>Tab</kbd>, I get the following completions:

```
-h         --help     <shell>    -V         --version
```

### Expected Behaviour

The `<shell>` value is a placeholder, it's not supposed to be used in completions.

I should have gotten the options of `bash`, `fish`, `zsh` instead.

### Additional Context

The full generated bash completions are:

```bash
_play() {
    local i cur prev opts cmds
    COMPREPLY=()
    cur="${COMP_WORDS[COMP_CWORD]}"
    prev="${COMP_WORDS[COMP_CWORD-1]}"
    cmd=""
    opts=""

    for i in ${COMP_WORDS[@]}
    do
        case "${i}" in
            play)
                cmd="play"
                ;;
            
            *)
                ;;
        esac
    done

    case "${cmd}" in
        play)
            opts=" -h -V  --help --version  <shell> "
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

complete -F _play -o bashdefault -o default play
```

### Debug Output

_No response_

---

_Label `T: bug` added by @ajeetdsouza on 2021-05-20 05:18_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2021-05-20 09:28_

---

_Label `C: generator` added by @pksunkara on 2021-05-20 09:28_

---

_Referenced in [clap-rs/clap#2490](../../clap-rs/clap/pulls/2490.md) on 2021-05-20 12:41_

---

_Referenced in [ryought/dbgphmm#37](../../ryought/dbgphmm/issues/37.md) on 2021-07-11 22:54_

---

_Closed by @bors[bot] on 2021-10-14 21:42_

---

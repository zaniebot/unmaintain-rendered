```yaml
number: 912
title: "Help message doesn't output "
type: issue
state: closed
author: kirillbobyrev
labels:
  - C-bug
  - A-help
  - E-medium
  - E-help-wanted
assignees: []
created_at: 2017-03-21T18:50:25Z
updated_at: 2018-08-02T03:30:04Z
url: https://github.com/clap-rs/clap/issues/912
synced_at: 2026-01-12T16:14:10Z
```

# Help message doesn't output 

---

_@kirillbobyrev_

If YAML is used running `program --help` would produce

```
yml_app 1.0
an example using a .yml file to build a CLI
```

instead of

```
yml_app 1.0
Kevin K. <kbknapp@gmail.com>
an example using a .yml file to build a CLI
```

### Rust Version

`rustc 1.16.0 (30cf806ef 2017-03-10)`

### Affected Version of clap

`"clap 2.21.2 (registry+https://github.com/rust-lang/crates.io-index)",
name = "clap"
"checksum clap 2.21.2 (registry+https://github.com/rust-lang/crates.io-index)" = "58ad5e8142f3a5eab0c1cba5011aa383e009842936107fe4d94f1a8d380a1aec"`

### Steps to Reproduce the issue

I am using the code from YAML example of the clap-rs repo trunk.

### Sample Code or Link to Sample Code

**src/main.rs**
```rust
#[macro_use]
extern crate clap;

fn main() {
    use clap::App;
    let yml = load_yaml!("cli.yaml");
    let m = App::from_yaml(yml).get_matches();

    if let Some(mode) = m.value_of("mode") {
        match mode {
            "vi" => println!("You are using vi"),
            "emacs" => println!("You are using emacs..."),
            _      => unreachable!()
        }
    } else {
        println!("--mode <MODE> wasn't used...");
    }
}
```

**src/cli.yaml**
```yaml
name: yml_app
version: "1.0"
about: an example using a .yml file to build a CLI
author: Kevin K. <kbknapp@gmail.com>

# AppSettings can be defined as a list and are **not** ascii case sensitive
settings:
    - ArgRequiredElseHelp

# All Args must be defined in the 'args:' list where the name of the arg, is the
# key to a Hash object
args:
    # The name of this argument, is 'opt' which will be used to access the value
    # later in your Rust code
    - opt:
        help: example option argument from yaml
        short: o
        long: option
        multiple: true
        takes_value: true
    - pos:
        help: example positional argument from yaml
        index: 1
        # A list of possible values can be defined as a list
        possible_values:
            - fast
            - slow
    - flag:
        help: demo flag argument
        short: F
        multiple: true
        global: true
        # Conflicts, mutual overrides, and requirements can all be defined as a
        # list, where the key is the name of the other argument
        conflicts_with:
            - opt
        requires:
            - pos
    - mode:
        long: mode
        help: shows an option with specific values
        # possible_values can also be defined in this list format
        possible_values: [ vi, emacs ]
        takes_value: true
    - mvals:
        long: mult-vals
        help: demos an option which has two named values
        # value names can be described in a list, where the help will be shown
        # --mult-vals <one> <two>
        value_names:
            - one
            - two
    - minvals:
        long: min-vals
        multiple: true
        help: you must supply at least two values to satisfy me
        min_values: 2
    - maxvals:
        long: max-vals
        multiple: true
        help: you can only supply a max of 3 values for me!
        max_values: 3

# All subcommands must be listed in the 'subcommand:' object, where the key to
# the list is the name of the subcommand, and all settings for that command are
# are part of a Hash object
subcommands:
    # The nae of this subcommand will be 'subcmd' which can be accessed in your
    # Rust code later
    - subcmd:
        about: demos subcommands from yaml
        version: "0.1"
        author: Kevin K. <kbknapp@gmail.com>
        # Subcommand args are exactly like App args
        args:
            - scopt:
                short: B
                multiple: true
                help: example subcommand option
                takes_value: true
            - scpos1:
                help: example subcommand positional
                index: 1

# ArgGroups are supported as well, and must be sepcified in the 'groups:'
# object of this file
groups:
    # the name of the ArgGoup is specified here
    - min-max-vals:
        # All args and groups that are a part of this group are set here
        args:
            - minvals
            - maxvals
        # setting conflicts is done the same manner as setting 'args:'
        #
        # to make this group required, you could set 'required: true' but for
        # this example we won't do that.
```


---

_Comment by @kbknapp on 2017-03-22 00:42_

Interesting! I'd have thought this would be caught by a test! Ive got a broken finger right now and the cast is causing me to only type with one hand. But if someone wants to confirm this and take a stab at a fix it shlould be in `src/app/mod.rs` in the `From` impl.

---

_Label `C: help message` added by @kbknapp on 2017-03-22 00:43_

---

_Label `C: yaml parser` added by @kbknapp on 2017-03-22 00:43_

---

_Label `D: easy` added by @kbknapp on 2017-03-22 00:43_

---

_Label `M: help wanted` added by @kbknapp on 2017-03-22 00:43_

---

_Label `M: mentored` added by @kbknapp on 2017-03-22 00:43_

---

_Label `P2: need to have` added by @kbknapp on 2017-03-22 00:43_

---

_Label `T: bug` added by @kbknapp on 2017-03-22 00:43_

---

_Label `W: 2.x` added by @kbknapp on 2017-03-22 00:43_

---

_Referenced in [clap-rs/clap#914](../../clap-rs/clap/pulls/914.md) on 2017-03-22 10:17_

---

_Comment by @kirillbobyrev on 2017-03-22 14:04_

@kbknapp @crazymerlyn Thank you very much!

---

_Closed by @kbknapp on 2017-03-22 23:52_

---

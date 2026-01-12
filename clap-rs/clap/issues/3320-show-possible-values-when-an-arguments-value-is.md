```yaml
number: 3320
title: Show possible values when an arguments value is missing
type: issue
state: closed
author: epage
labels:
  - C-enhancement
  - A-help
  - E-medium
assignees: []
created_at: 2022-01-19T18:48:25Z
updated_at: 2022-02-02T20:49:36Z
url: https://github.com/clap-rs/clap/issues/3320
synced_at: 2026-01-12T16:14:14Z
```

# Show possible values when an arguments value is missing

---

_@epage_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

v3.0.6

### Describe your use case

Many CLIs allow using the argument without a value to list what values are available.  This allows a more targeted browsing than digging in `--help`

Today, clap just points people to help
```console
$ git-stack --color
    Finished dev [unoptimized + debuginfo] target(s) in 0.04s
     Running `/home/epage/src/personal/git-stack/target/debug/git-stack --color`
error: The argument '--color <WHEN>' requires a value but none was supplied

USAGE:
    git-stack [OPTIONS]

For more information try --help
```


### Describe the solution you'd like

For example:
```console
$ cargo test --test    
error: "--test" takes one argument.                                      
Available tests:                    
    builder     
    derive                                                               
    derive_ui                       
    examples                                                             
    macros                                                                                                                                         
    yaml                                                                 
```
(granted, this is dynamically generating the list)

We could also list the `PossibleValue::help` like in #3312.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @epage on 2022-01-19 18:48_

---

_Label `A-help` added by @epage on 2022-01-19 18:48_

---

_Label `E-medium` added by @epage on 2022-01-19 18:48_

---

_Comment by @epage on 2022-01-19 18:52_

Maybe we should also have the unknown value error point people to this

Today:
```console
$ git-stack --color lklk
    Finished dev [unoptimized + debuginfo] target(s) in 0.04s
     Running `/home/epage/src/personal/git-stack/target/debug/git-stack --color lklk`
error: Invalid value for '--color <WHEN>': Unknown color choice 'lklk'

For more information try --help

$ git-stack --color
    Finished dev [unoptimized + debuginfo] target(s) in 0.04s
     Running `/home/epage/src/personal/git-stack/target/debug/git-stack --color`
error: The argument '--color <WHEN>' requires a value but none was supplied

USAGE:
    git-stack [OPTIONS]

For more information try --help
```

Potential future:
```console
$ git-stack --color lklk
    Finished dev [unoptimized + debuginfo] target(s) in 0.04s
     Running `/home/epage/src/personal/git-stack/target/debug/git-stack --color lklk`
error: Invalid value for '--color <WHEN>': Unknown color choice 'lklk'

For valid color choices try `git-stack --color`
For more information try `git-stack --help`

$ git-stack --color
    Finished dev [unoptimized + debuginfo] target(s) in 0.04s
     Running `/home/epage/src/personal/git-stack/target/debug/git-stack --color`
error: The argument '--color <WHEN>' requires a value but none was supplied

Possible values:
- auto
- always
- never
- 
For more information try `git-stack --help`
```

---

_Comment by @epage on 2022-01-19 22:08_

Correction: looks like I wasn't using `possible_values`.  The error is actually
```console
$ cargo run -- --color foo
    Finished dev [unoptimized + debuginfo] target(s) in 0.02s
     Running `/home/epage/src/personal/concolor/target/debug/concolor-example --color foo`
error: "foo" isn't a valid value for '--color <WHEN>'
        [possible values: always, auto, never]

USAGE:
    concolor-example --color <WHEN>

For more information try --help
```

---

_Referenced in [clap-rs/clap#3394](../../clap-rs/clap/pulls/3394.md) on 2022-02-02 20:31_

---

_Closed by @epage on 2022-02-02 20:49_

---

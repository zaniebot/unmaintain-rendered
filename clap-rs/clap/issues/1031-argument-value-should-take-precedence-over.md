```yaml
number: 1031
title: Argument value should take precedence over subcommand with the same name
type: issue
state: closed
author: axelchalon
labels:
  - C-bug
assignees: []
created_at: 2017-08-16T14:14:43Z
updated_at: 2018-08-02T03:30:10Z
url: https://github.com/clap-rs/clap/issues/1031
synced_at: 2026-01-12T16:14:10Z
```

# Argument value should take precedence over subcommand with the same name

---

_@axelchalon_

_rustc 1.18.0 / clap 2.26.0_

When using the `argument value` notation (as opposed to `argument=value`), when a top-level argument has a value with the same name as a subcommand, Clap parses this as running the subcommand and using the top-level argument with no value (even though one is required).

### Sample Code / Steps to Reproduce the Issue
```
App::new("My program")
    .arg(Arg::from_usage("--ui-path=<PATH>"))
    .subcommand(SubCommand::with_name("signer"))
    .get_matches();
```

```
$ program --ui-path signer

error: The argument '--ui-path <PATH>' requires a value but none was supplied       

USAGE:               
    toast --ui-path <PATH> [SUBCOMMAND]   

For more information try --help                        
```

Thanks!

---

_Referenced in [openethereum/parity-ethereum#6356](../../openethereum/parity-ethereum/pulls/6356.md) on 2017-08-22 15:58_

---

_Comment by @kbknapp on 2017-10-04 03:30_

Agreed. Sorry for the delay, I've been traveling for work for the past month and a half. I'll mark this as a bug.

A workaround in the meantime is to use [`Arg::require_equals`](https://docs.rs/clap/2.26.2/clap/struct.Arg.html#method.require_equals) which requires `--ui-path=signer` and disallows `--ui-path signer`

Another workaround is to disallow subcommands when a valid arg has been found. I.e. disallow the `program <arg> <subcommand> <arg>` structure making it `program <args>` or `program <subcommand> <args>` but not mixing. This is done with [`AppSettings::ArgsNegateSubcommands`](https://docs.rs/clap/2.26.2/clap/enum.AppSettings.html#variant.ArgsNegateSubcommands)

---

_Label `C: subcommands` added by @kbknapp on 2017-10-04 03:31_

---

_Label `D: easy` added by @kbknapp on 2017-10-04 03:31_

---

_Label `P2: need to have` added by @kbknapp on 2017-10-04 03:31_

---

_Label `T: bug` added by @kbknapp on 2017-10-04 03:31_

---

_Label `W: 2.x` added by @kbknapp on 2017-10-04 03:31_

---

_Added to milestone `2.27.0` by @kbknapp on 2017-10-12 21:37_

---

_Closed by @kbknapp on 2017-10-25 01:49_

---

_Referenced in [clap-rs/clap#1721](../../clap-rs/clap/issues/1721.md) on 2020-04-16 12:25_

---

_Referenced in [clap-rs/clap#1834](../../clap-rs/clap/pulls/1834.md) on 2020-04-16 13:33_

---

_Referenced in [clap-rs/clap#1915](../../clap-rs/clap/issues/1915.md) on 2020-05-08 03:08_

---

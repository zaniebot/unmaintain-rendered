```yaml
number: 2865
title: Multicall should support programs that have a subcommand with the same name as the program
type: issue
state: closed
author: fishface60
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2021-10-12T21:08:53Z
updated_at: 2021-12-13T15:22:00Z
url: https://github.com/clap-rs/clap/issues/2865
synced_at: 2026-01-12T16:14:13Z
```

# Multicall should support programs that have a subcommand with the same name as the program

---

_@fishface60_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.0-beta.4

### Describe your use case

`AppSettings::Multicall` supports the use-case of hostname, which doesn't have applets available as subcommands for the program when run by its base program name rather than via a link.

This is implemented by giving preference to matching against subcommand names.

This makes us unable to support programs whose name as a noun is the same as what it does as a verb.
e.g. `badger` may be a program that sends nagging reminders until you confirm you've done the task. It doing this is badgering you, so the command to initiate it doing this could be `badger badger foo@example.com`.

It may be a multicall executable because part of its behaviour involves running a subcommand e.g. it sends data from a socket via pipe so it implements a significant portion of the behaviour of `nc`. It normally starts this subprocess by running `badger nc`, but it may be better optimised than the system `nc` and so would be useful to install a link to badger as nc.

### Describe the solution you'd like

Add an additional flag to change the argv[0] matching behaviour to check against program name first.
This is a slight optimisation if the program name is expected to match more than a subcommand.

### Alternatives, if applicable

We could WONTFIX this as being too obscure a use-case.

We could invert the behaviour so this is the default, and expect `hostname`-like programs to set their base program name to something they won't be installed as and only install links to their applets.

### Additional Context

_No response_

---

_Label `T: new feature` added by @fishface60 on 2021-10-12 21:08_

---

_Referenced in [clap-rs/clap#2861](../../clap-rs/clap/issues/2861.md) on 2021-10-12 21:09_

---

_Label `T: new feature` removed by @pksunkara on 2021-10-12 21:12_

---

_Label `C: settings` added by @pksunkara on 2021-10-12 21:12_

---

_Label `T: bug` added by @pksunkara on 2021-10-12 21:12_

---

_Referenced in [clap-rs/clap#3041](../../clap-rs/clap/pulls/3041.md) on 2021-11-19 22:17_

---

_Referenced in [epage/clapng#215](../../epage/clapng/issues/215.md) on 2021-12-06 22:18_

---

_Referenced in [epage/clapng#218](../../epage/clapng/issues/218.md) on 2021-12-06 22:18_

---

_Label `C: settings` removed by @epage on 2021-12-08 20:17_

---

_Label `A-parsing` added by @epage on 2021-12-08 20:17_

---

_Closed by @epage on 2021-12-13 15:22_

---

---
number: 2866
title: Multicall has a performance impact on start-up
type: issue
state: closed
author: fishface60
labels:
  - C-enhancement
  - A-parsing
assignees: []
created_at: 2021-10-12T21:18:58Z
updated_at: 2021-12-13T15:22:01Z
url: https://github.com/clap-rs/clap/issues/2866
synced_at: 2026-01-07T13:12:19-06:00
---

# Multicall has a performance impact on start-up

---

_Issue opened by @fishface60 on 2021-10-12 21:18_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.0-beta.4

### Describe your use case

`AppSettings::Multicall` causes it to check the executable name against all applet names first and then later against the program name.

If the base program name is not the same as any applets we can match against it first, which may be faster if we expect to access it via that name more often than as an applet link.


### Describe the solution you'd like

We can know in advance that the base program name is not the same as an applet by checking the name of an added subcommand against the base program name and set an internal flag to match against base program name first.

### Alternatives, if applicable

We could WONTFIX this as we don't expect program start-up to be hot code.

### Additional Context

_No response_

---

_Label `T: new feature` added by @fishface60 on 2021-10-12 21:18_

---

_Referenced in [clap-rs/clap#2861](../../clap-rs/clap/issues/2861.md) on 2021-10-12 21:19_

---

_Label `T: new feature` removed by @pksunkara on 2021-10-12 21:20_

---

_Label `T: enhancement` added by @pksunkara on 2021-10-12 21:20_

---

_Label `C: settings` added by @pksunkara on 2021-10-12 21:20_

---

_Referenced in [clap-rs/clap#3041](../../clap-rs/clap/pulls/3041.md) on 2021-11-19 22:17_

---

_Referenced in [epage/clapng#215](../../epage/clapng/issues/215.md) on 2021-12-06 22:18_

---

_Referenced in [epage/clapng#219](../../epage/clapng/issues/219.md) on 2021-12-06 22:18_

---

_Label `A-builder` added by @epage on 2021-12-08 20:16_

---

_Label `A-builder` removed by @epage on 2021-12-08 20:17_

---

_Label `C: settings` removed by @epage on 2021-12-08 20:17_

---

_Label `A-parsing` added by @epage on 2021-12-08 20:17_

---

_Closed by @epage on 2021-12-13 15:22_

---

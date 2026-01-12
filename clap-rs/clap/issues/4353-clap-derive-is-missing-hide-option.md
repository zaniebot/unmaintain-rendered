```yaml
number: 4353
title: "Clap derive is missing \"hide\" option"
type: issue
state: closed
author: Firstyear
labels:
  - C-enhancement
assignees: []
created_at: 2022-10-06T08:22:55Z
updated_at: 2022-10-07T00:14:50Z
url: https://github.com/clap-rs/clap/issues/4353
synced_at: 2026-01-12T16:14:15Z
```

# Clap derive is missing "hide" option

---

_@Firstyear_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.0.10

### Describe your use case

Reading https://docs.rs/clap/latest/clap/_derive/index.html it appears that the hide method ( https://docs.rs/clap/latest/clap/builder/struct.Command.html#method.hide ) is not exposed via the derive interface.

### Describe the solution you'd like

Either that:

* Hide is added to derive
* If it is already supported, that is is documented in the derive reference

### Alternatives, if applicable

n/a

### Additional Context

n/a

---

_Label `C-enhancement` added by @Firstyear on 2022-10-06 08:22_

---

_Referenced in [kanidm/kanidm#1095](../../kanidm/kanidm/pulls/1095.md) on 2022-10-06 08:24_

---

_Comment by @epage on 2022-10-06 11:55_

Under [Arg Attributes](https://docs.rs/clap/latest/clap/_derive/index.html#arg-attributes) is this line

> Raw attributes: Any [Arg method](https://docs.rs/clap/latest/clap/builder/struct.Arg.html) can also be used as an attribute, see [Terminology](https://docs.rs/clap/latest/clap/_derive/index.html#terminology) for syntax.

Any `Arg` builder method can be used in the derive, including `hide`.

For more on why it is documented this way and brainstorming for improving it, see https://github.com/clap-rs/clap/discussions/4090

---

_Closed by @epage on 2022-10-06 11:55_

---

_Comment by @Firstyear on 2022-10-07 00:14_

Thanks! I totally missed that it was extremely subtle :( 

---

_Referenced in [metalbear-co/mirrord#767](../../metalbear-co/mirrord/pulls/767.md) on 2022-11-27 20:43_

---

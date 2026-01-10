---
number: 2778
title: "Auto-generated completion scripts add `NuShell` support"
type: issue
state: closed
author: hustcer
labels:
  - C-enhancement
  - E-medium
  - A-completion
  - E-help-wanted
assignees: []
created_at: 2021-09-20T00:22:54Z
updated_at: 2023-05-24T20:52:49Z
url: https://github.com/clap-rs/clap/issues/2778
synced_at: 2026-01-10T01:27:25Z
---

# Auto-generated completion scripts add `NuShell` support

---

_Issue opened by @hustcer on 2021-09-20 00:22_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.0-beta.4

### Describe your use case

Auto-generated completion scripts add [NuShell](https://github.com/nushell/nushell) support

### Describe the solution you'd like

Auto-generated completion scripts add [NuShell](https://github.com/nushell/nushell) support, just as clap support (Bash, Zsh, Fish, Elvish and PowerShell), Thx

### Alternatives, if applicable

_No response_

### Additional Context

```
nu --version
───┬────────────────────┬────────────────────────────────────────────────────────────────────────────────────
 # │      Column0       │                                      Column1
───┼────────────────────┼────────────────────────────────────────────────────────────────────────────────────
 0 │ version            │ 0.37.0
 1 │ build_os           │ macos-x86_64
 2 │ rust_version       │ rustc 1.55.0
 3 │ cargo_version      │ cargo 1.55.0
 4 │ pkg_version        │ 0.37.0
 5 │ build_time         │ 2021-09-14 19:45:30 +00:00
 6 │ build_rust_channel │ release
 7 │ features           │ clipboard-cli, ctrlc, dataframe, default, rustyline, term, trash, uuid, which, zip
 8 │ installed_plugins  │
───┴────────────────────┴────────────────────────────────────────────────────────────────────────────────────
```

---

_Label `T: new feature` added by @hustcer on 2021-09-20 00:22_

---

_Referenced in [casey/just#974](../../casey/just/issues/974.md) on 2021-09-20 01:07_

---

_Label `C: generator` added by @pksunkara on 2021-09-20 05:16_

---

_Comment by @epage on 2021-09-20 17:01_

Somewhat unrelated but I wonder what the code size / compile time looks like for all of our generators and if there is anything we can help developers with development time as our generators expand in number of targets and capabilities.

---

_Comment by @pksunkara on 2021-09-20 21:09_

That was the intention for extra generators. For the basic common ones, we have them in the package itself, but if we were to implement this, we would create a new package called `clap_generator_nushell` or something.

One thing I am not sure if we can do in Rust because of these new const stuff is, can a user extend the `Shell` enum through 3rd party code (traits)?

---

_Comment by @epage on 2021-09-20 21:27_

A user could manually impl `ArgEnum` and flatten `Shell` into their enum or we could add `flatten` support but I'm not aware of a language feature that would help.

---

_Referenced in [epage/clapng#207](../../epage/clapng/issues/207.md) on 2021-12-06 22:13_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:09_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:09_

---

_Label `E-medium` added by @epage on 2021-12-13 17:51_

---

_Comment by @epage on 2021-12-13 18:05_

If nushell works like bash where you have to write your own parser, #3166 could be a big help towards this.

---

_Label `E-help-wanted` added by @epage on 2022-06-16 21:15_

---

_Referenced in [Schniz/fnm#800](../../Schniz/fnm/pulls/800.md) on 2022-08-17 07:03_

---

_Referenced in [Schniz/fnm#801](../../Schniz/fnm/pulls/801.md) on 2022-08-17 07:32_

---

_Comment by @TornaxO7 on 2022-09-28 23:30_

May I ask, what the state of this issue is?

---

_Comment by @jokeyrhyme on 2022-09-28 23:45_

I'm a huge fan of nushell, but it seems to be in a state of flux (going off the last couple of releases): https://www.nushell.sh/blog/

How likely is it that there are significant changes in the syntax or other workings relating to completion over on the nushell side?

---

_Comment by @epage on 2023-05-20 00:07_

Looks like someone created https://github.com/nibon7/clap_complete_nushell

---

_Referenced in [nibon7/clap_complete_nushell#1](../../nibon7/clap_complete_nushell/issues/1.md) on 2023-05-20 00:08_

---

_Referenced in [clap-rs/clap#4935](../../clap-rs/clap/pulls/4935.md) on 2023-05-23 15:11_

---

_Closed by @epage on 2023-05-24 20:52_

---

---
number: 3159
title: Clink completions for Windows users
type: issue
state: open
author: rashil2000
labels:
  - C-enhancement
  - E-medium
  - A-completion
assignees: []
created_at: 2021-12-12T07:31:36Z
updated_at: 2021-12-13T14:23:06Z
url: https://github.com/clap-rs/clap/issues/3159
synced_at: 2026-01-10T01:27:34Z
---

# Clink completions for Windows users

---

_Issue opened by @rashil2000 on 2021-12-12 07:31_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

master

### Describe your use case

While cmd.exe is not extensible out-of-the-box, there's a very popular framework known as [Clink](https://github.com/chrisant996/clink) (which is used by the Cmder package) that adds Readline support along with history, completions, autosuggestions and highlighting. A set of handwritten completions are already available here https://github.com/vladimir-kotikov/clink-completions as an example. 

As Clink does provide an official and extremely flexible API for writing completions, I feel this is something that Rust CLI programs can utilize through clap.


### Describe the solution you'd like

I'm not very familiar with Rust itself, but similar to `clap_generate/src/generators/shells/elvish.rs`, we can have a `clap_generate/src/generators/shells/cmd.rs` source file too.

### Alternatives, if applicable

_No response_

### Additional Context

The documentation for argument completion is available here - https://chrisant996.github.io/clink/clink.html#argument-completion

---

_Label `C-enhancement` added by @rashil2000 on 2021-12-12 07:31_

---

_Renamed from "Completions for cmd.exe" to "Clink completions for Windows users" by @epage on 2021-12-13 14:20_

---

_Label `A-completion` added by @epage on 2021-12-13 14:21_

---

_Label `E-medium` added by @epage on 2021-12-13 14:21_

---

_Comment by @epage on 2021-12-13 14:23_

Thanks!  Always good to support more shells!

Per our current practice as [documented in clap_generate](https://github.com/clap-rs/clap/blob/master/clap_generate/CONTRIBUTING.md) this will most likely start as a `clap_generate_clink`, like `clap_generate_fig`.

---

_Referenced in [clap-rs/clap#5379](../../clap-rs/clap/pulls/5379.md) on 2024-02-28 22:09_

---

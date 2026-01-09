---
number: 5562
title: Generate shell completions as part of the build process to assist cross compilation
type: issue
state: closed
author: Flowdalic
labels:
  - C-enhancement
  - A-completion
assignees: []
created_at: 2024-07-02T09:31:44Z
updated_at: 2024-07-06T11:39:14Z
url: https://github.com/clap-rs/clap/issues/5562
synced_at: 2026-01-07T13:12:20-06:00
---

# Generate shell completions as part of the build process to assist cross compilation

---

_Issue opened by @Flowdalic on 2024-07-02 09:31_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.8

### Describe your use case

Gentoo currently improves on Rust cross-compilation. However many Rust packages in Gentoo generate the shell completions files by invoking the resulting binary, which will then output the shell completion (files). Unfortunately, this approach does not work when cross-compiling, since the resulting binary will be a binary of a different architecture than the build host.

See also https://www.mail-archive.com/gentoo-dev@lists.gentoo.org/msg99709.html

### Describe the solution you'd like

The shell completion files should be an output of `cargo build` and not of the final binary.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @Flowdalic on 2024-07-02 09:31_

---

_Comment by @epage on 2024-07-05 16:02_

Not to sure what is expected of clap.  We [document both workflows](https://docs.rs/clap_complete/latest/clap_complete/) and a reason to generate completions as part of the program (rather than `cargo build`) is so that the completions can be generated on-demand, rather than statically, so that they "auto-update" with the program (for non-distro use cases).  In fact, I'm going to be encouraging that workflow with the new completion system (#3166) as there is a tighter coupling between the completions and the users program.

---

_Label `A-completion` added by @epage on 2024-07-05 16:02_

---

_Comment by @Flowdalic on 2024-07-06 08:41_

> Not to sure what is expected of clap. We [document both workflows](https://docs.rs/clap_complete/latest/clap_complete/)

I wasn't aware that clap is already able to generate the completions as part of the build. Thanks for pointing this out!

> In fact, I'm going to be encouraging that workflow

Feel free to encourage it. But with my package-maintainer hat on, there is no need for that if the application is installed via means of the Linux distribution. Because then, the completions are installed system wide via the package manager of the used Linux distribution and updated as soon as the package is updated. So, please keep the `generate_to` functionality, as it is relevant when packaging the application.

---

_Closed by @epage on 2024-07-06 11:39_

---

_Referenced in [clap-rs/clap#5668](../../clap-rs/clap/issues/5668.md) on 2024-08-12 15:15_

---

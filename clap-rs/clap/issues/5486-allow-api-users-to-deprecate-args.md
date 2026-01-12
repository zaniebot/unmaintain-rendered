```yaml
number: 5486
title: Allow API users to deprecate args
type: issue
state: closed
author: valadaptive
labels:
  - C-enhancement
assignees: []
created_at: 2024-05-04T02:04:23Z
updated_at: 2024-05-04T20:52:47Z
url: https://github.com/clap-rs/clap/issues/5486
synced_at: 2026-01-12T16:14:17Z
```

# Allow API users to deprecate args

---

_@valadaptive_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.4

### Describe your use case

This seems to be a prerequisite to https://github.com/rust-lang/cargo/issues/6790, or at the very least it would really help.

To stabilize Cargo's `--out-dir` flag, it likely will need to be renamed to something else. However, there are a lot of users of `--out-dir`, so it would be nice to be able to *deprecate* `--out-dir` and direct users to whatever it gets renamed to.

Such a scenario is applicable to other projects as well: API consumers may want to rename arguments or give them a deprecation period before removing them entirely.

### Describe the solution you'd like

A way to mark `Arg`s as deprecated, or warn when they are passed. Passing such an argument to the program will result in a warning being printed, telling the user to either stop using the argument or migrate to its replacement.

I don't entirely know what this API should look like--should it be a generic "warn on argument use" API or specifically designed around deprecation?

### Alternatives, if applicable

1. A way to specifically mark `Arg`s as deprecated, possibly allowing for deprecation-specific functionality like automatically redirecting users to their replacements.
2. A simpler "print a warning when argument passed" mechanism.

### Additional Context

_No response_

---

_Label `C-enhancement` added by @valadaptive on 2024-05-04 02:04_

---

_Comment by @epage on 2024-05-04 20:52_

Closing in favor of #3321.  If there is a reason to keep this open separately, let us know!

As for cargo, I wouldn't wait on anything like this.  We've already handled cases like this before by defining both attributes and suing `shell.warn` if the deprecated one is used. 

---

_Closed by @epage on 2024-05-04 20:52_

---

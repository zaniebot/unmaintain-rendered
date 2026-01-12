```yaml
number: 3760
title: "`max_values` confusingly returns `ErrorKind::UnknownArgument` when too many arguments are provided"
type: issue
state: open
author: SilvanCodes
labels:
  - C-enhancement
  - A-parsing
  - S-waiting-on-design
assignees: []
created_at: 2022-05-26T22:30:55Z
updated_at: 2022-05-26T23:53:42Z
url: https://github.com/clap-rs/clap/issues/3760
synced_at: 2026-01-12T16:14:15Z
```

# `max_values` confusingly returns `ErrorKind::UnknownArgument` when too many arguments are provided

---

_@SilvanCodes_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.1.15

### Describe your use case

I want to limit the maximum given arguments with a proper error message for the user when too many arguments are provided.

### Describe the solution you'd like

Instead of `ErrorKind::UnknownArgument` I would like `max_values` to return a new error kind like `ErrorKind::TooManyValues` mirroring the behavior of `min_values` with its error kind `ErrorKind::TooFewValues`.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @SilvanCodes on 2022-05-26 22:30_

---

_Comment by @epage on 2022-05-26 23:53_

With `TooFewValues`, there is a definitive case for us to identify and report an error: the parser was looking for more values and didn't see them.

`TooManyValues` is a bit trickier because the parser exhausted one argument and is processing the next, finding it doesn't exist.  We'd need to match against a trailing argument that took a value but there are no more positional or subcommands afterwards.  The error also might not make sense for an argument that just takes one value.   Or maybe it does.

Then it comes to the implementation.  Our parser isn't in a great state right now though hopefully some in going refactors will improve things a bit.

This isn't a "no" but that parts of the design need to be figured out and that we might wait for some refactorings before its practical to implement.

---

_Label `A-parsing` added by @epage on 2022-05-26 23:53_

---

_Label `S-waiting-on-design` added by @epage on 2022-05-26 23:53_

---

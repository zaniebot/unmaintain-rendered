```yaml
number: 4666
title: "Singular used even when multiple suggestions exist `note: subcommand 'foo', 'bar' exist`"
type: issue
state: closed
author: corneliusroemer
labels:
  - C-bug
assignees: []
created_at: 2023-01-23T19:52:43Z
updated_at: 2023-01-23T21:58:39Z
url: https://github.com/clap-rs/clap/issues/4666
synced_at: 2026-01-12T16:14:16Z
```

# Singular used even when multiple suggestions exist `note: subcommand 'foo', 'bar' exist`

---

_@corneliusroemer_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.66.1

### Clap Version

master

### Minimal reproducible code

There already exists a test that expects the wrong grammar (intentionally?):

https://github.com/clap-rs/clap/blob/90c042eeaef36ef6a1718b19cc9ea213ac249f6e/tests/builder/subcommands.rs#L123 directly

This can be confusing, as the user could think that usage of singular has meaning and starts to think about why singular is used.

### Steps to reproduce the bug with the above code

NA

### Actual Behaviour

```
note: subcommand 'test', 'temp' exists
```

### Expected Behaviour

```
note: subcommands 'test', 'temp' exist
```

-> Add a suffix `-s` `subcommand` and get rid of `exists` suffix `-s`

### Additional Context

Possible fix could be in https://github.com/clap-rs/clap/blob/90c042eeaef36ef6a1718b19cc9ea213ac249f6e/src/error/format.rs#L70-L85

Though this would require a bit of extra code = increased complexity.

Interestingly, the plural/singular of the `exist`/`exists` part is correctly implemented already in  https://github.com/clap-rs/clap/blob/90c042eeaef36ef6a1718b19cc9ea213ac249f6e/src/error/format.rs#L440-L460

### Debug Output

_No response_

---

_Label `C-bug` added by @corneliusroemer on 2023-01-23 19:52_

---

_Renamed from "Singular used even when multiple suggestions exist `note: subcommand 'foo', 'bar' exist`" to "Singular used even when multiple suggestions exist `note: subcommand 'foo', 'bar' exists`" by @corneliusroemer on 2023-01-23 19:52_

---

_Renamed from "Singular used even when multiple suggestions exist `note: subcommand 'foo', 'bar' exists`" to "Singular used even when multiple suggestions exist `note: subcommand 'foo', 'bar' exist`" by @corneliusroemer on 2023-01-23 20:38_

---

_Referenced in [clap-rs/clap#4667](../../clap-rs/clap/pulls/4667.md) on 2023-01-23 21:04_

---

_Closed by @epage on 2023-01-23 21:58_

---

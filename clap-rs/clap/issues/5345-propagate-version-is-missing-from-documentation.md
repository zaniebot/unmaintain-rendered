```yaml
number: 5345
title: propagate_version is missing from documentation
type: issue
state: closed
author: szabgab
labels:
  - C-enhancement
assignees: []
created_at: 2024-02-09T08:19:23Z
updated_at: 2024-02-09T15:18:42Z
url: https://github.com/clap-rs/clap/issues/5345
synced_at: 2026-01-12T16:14:16Z
```

# propagate_version is missing from documentation

---

_@szabgab_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5

### Describe your use case

[this example](https://github.com/clap-rs/clap/blob/master/examples/tutorial_derive/03_04_subcommands_alt.rs) uses `propagate_version`, but it is not included in [documentation](https://docs.rs/clap/latest/clap/_derive/index.html)

### Describe the solution you'd like

Please explain what that command does.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @szabgab on 2024-02-09 08:19_

---

_Comment by @epage on 2024-02-09 15:18_

Its covered in the [command attribute](https://docs.rs/clap/latest/clap/_derive/index.html#command-attributes) documentation in the blanket "Raw Attributes" section.

Closing in favor of the discussion at https://github.com/clap-rs/clap/discussions/4090

---

_Closed by @epage on 2024-02-09 15:18_

---

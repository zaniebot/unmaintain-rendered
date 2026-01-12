```yaml
number: 2557
title: "Allow different documentation for subcommands for SUBCOMMANDS list and subcommand's help"
type: issue
state: closed
author: anatawa12
labels:
  - A-help
  - ":money_with_wings: $5"
assignees: []
created_at: 2021-06-20T10:30:05Z
updated_at: 2021-06-20T15:28:51Z
url: https://github.com/clap-rs/clap/issues/2557
synced_at: 2026-01-12T16:14:13Z
```

# Allow different documentation for subcommands for SUBCOMMANDS list and subcommand's help

---

_@anatawa12_

### Discussed in https://github.com/clap-rs/clap/discussions/2556

<div type='discussions-op-text'>

<sup>Originally posted by **anatawa12** June 20, 2021</sup>
### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

= 3.0.0-beta.2

### Describe your use case

For a subcommand with long documentation, in the `SUBCOMMANDS` list in parent command, I want to show a summary of a command instead of long documentation.

### Describe the solution you'd like

Add an option to show only first line in the documentation on the subcommand's struct or field.
```
/// Simple description
/// 
/// long documentation
#[derive(Clap)]
#[clap(single_line_doc_in_parent_doc)]
pub struct SubcommandOptions {
}
```

### Alternatives, if applicable

_No response_

### Additional Context

I'm using #[derive(Clap)] pattern.
</div>

---

_Comment by @pksunkara on 2021-06-20 10:30_

The solution is to always show short help in the `SUBCOMMANDS` list.

---

_Label `:money_with_wings: $5` added by @pksunkara on 2021-06-20 10:31_

---

_Label `C: help message` added by @pksunkara on 2021-06-20 10:31_

---

_Label `C: subcommands` added by @pksunkara on 2021-06-20 10:31_

---

_Referenced in [clap-rs/clap#2558](../../clap-rs/clap/pulls/2558.md) on 2021-06-20 12:09_

---

_Closed by @pksunkara on 2021-06-20 15:28_

---

---
number: 5997
title: "FR: Prefix argument long and short aliases in help output with -- and -"
type: issue
state: closed
author: cenviity
labels:
  - C-enhancement
  - A-help
  - E-easy
assignees: []
created_at: 2025-05-10T14:32:23Z
updated_at: 2025-05-11T00:45:30Z
url: https://github.com/clap-rs/clap/issues/5997
synced_at: 2026-01-10T01:28:20Z
---

# FR: Prefix argument long and short aliases in help output with -- and -

---

_Issue opened by @cenviity on 2025-05-10 14:32_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

master

### Describe your use case

Currently, the help output shows aliases without leading `--` and `-`:

```
Options:
  -o, --opt <opt>  [short aliases: v]
  -f, --flag       [aliases: flag1] [short aliases: a, b, 🦆]
```

This makes the aliases appear like (sub)commands and hinder copy-pasting into the command line directly.

### Describe the solution you'd like

If the leading `--` and `-` are included, then the aliases will be formatted consistently with the arguments at the start of each line. It will also be easier to copy and paste the aliases correctly elsewhere:

```
Options:
  -o, --opt <opt>  [short aliases: -v]
  -f, --flag       [aliases: --flag1] [short aliases: -a, -b, -🦆]
```

### Alternatives, if applicable

Sticking with the status quo and relying on the user correctly prefixing long aliases with `--` and short aliases with `-` after identifying the aliases from the help output.

### Additional Context

_No response_

---

_Label `C-enhancement` added by @cenviity on 2025-05-10 14:32_

---

_Referenced in [clap-rs/clap#5996](../../clap-rs/clap/pulls/5996.md) on 2025-05-10 14:36_

---

_Label `A-help` added by @epage on 2025-05-11 00:44_

---

_Label `E-easy` added by @epage on 2025-05-11 00:44_

---

_Comment by @epage on 2025-05-11 00:44_

Huh, wonder why we didn't do that before.

---

_Closed by @epage on 2025-05-11 00:45_

---

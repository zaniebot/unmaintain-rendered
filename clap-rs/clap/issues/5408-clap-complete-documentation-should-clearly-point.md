```yaml
number: 5408
title: clap_complete documentation should clearly point to CommandFactory
type: issue
state: open
author: lolbinarycat
labels:
  - C-enhancement
assignees: []
created_at: 2024-03-21T01:35:47Z
updated_at: 2024-03-21T12:54:07Z
url: https://github.com/clap-rs/clap/issues/5408
synced_at: 2026-01-12T16:14:17Z
```

# clap_complete documentation should clearly point to CommandFactory

---

_@lolbinarycat_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

clap_complete 4.5.1

### Describe your use case

i am using clap_complete for the first time, and it took me a bit of poking around to figure out how to get a Command from a Parser (plus having good enough intuition to guess that such a function must exist, otherwise i may have given up)

### Describe the solution you'd like

add something like "for `Parser` users, see `clap::CommandFactory::command`" near the top of the clap_complete crate documentation.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @lolbinarycat on 2024-03-21 01:35_

---

_Comment by @epage on 2024-03-21 12:54_

I'm curious, did you look at the [example](https://github.com/clap-rs/clap/blob/master/clap_complete/examples/completion-derive.rs)?  

---

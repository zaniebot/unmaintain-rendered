---
number: 5611
title: "Missing methods `Command::get_arg` and `Command::get_group`"
type: issue
state: closed
author: noc7c9
labels:
  - C-enhancement
assignees: []
created_at: 2024-07-31T11:36:50Z
updated_at: 2024-08-01T01:05:48Z
url: https://github.com/clap-rs/clap/issues/5611
synced_at: 2026-01-07T13:12:20-06:00
---

# Missing methods `Command::get_arg` and `Command::get_group`

---

_Issue opened by @noc7c9 on 2024-07-31 11:36_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.11

### Describe your use case

There doesn't seem to be a way to directly get a reference to an existing `Arg` or `ArgGroup` in a `Command` using an id. I did manage to work around it using `Command::get_arguments().find(..)` and `Command::get_groups().find(..)`. 

Perhaps I'm missing something but it seems like an obvious gap in the API.

### Describe the solution you'd like

Add the `Command::get_arg(&self, id: &str) -> Option<&Arg>` and `Command::get_group(&self, id: &str) -> Option<&ArgGroup>` methods.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @noc7c9 on 2024-07-31 11:36_

---

_Comment by @epage on 2024-07-31 14:34_

You can do `command[id]` to get an ‘Arg`.

We have `find` and `find_group` internally. Since they offer so little value over doing it yourself, I'm tempted to leave them as private. We're trying to be judicious in helpers the API provides to help keep bloat down.

---

_Comment by @noc7c9 on 2024-08-01 01:05_

Ah didn't realize there was an `Index` impl. That definitely cleans things up a bit, thanks.

> We're trying to be judicious in helpers the API provides to help keep bloat down.

That's more than reasonable.

---

_Closed by @noc7c9 on 2024-08-01 01:05_

---

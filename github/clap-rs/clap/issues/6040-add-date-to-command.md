---
number: 6040
title: Add date to Command
type: issue
state: closed
author: dcampbell24
labels:
  - C-enhancement
assignees: []
created_at: 2025-06-22T19:11:55Z
updated_at: 2025-06-22T19:55:21Z
url: https://github.com/clap-rs/clap/issues/6040
synced_at: 2026-01-07T13:12:20-06:00
---

# Add date to Command

---

_Issue opened by @dcampbell24 on 2025-06-22 19:11_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

master

### Describe your use case

Optionally, change the display of the clap man page.

### Describe the solution you'd like

Using `clap_mangen` where a published date is supposed to go instead you display the application name and version. I propose you add a function to Command called `date`, e.g. cmd.date("June 2025"), which will cause that date to be displayed in the appropriate place (bottom center).

I would suggest that this function does something to the base clap command, but I can't find any examples of a date being displayed anywhere in the command.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @dcampbell24 on 2025-06-22 19:11_

---

_Comment by @dcampbell24 on 2025-06-22 19:55_

It already exists...

---

_Closed by @dcampbell24 on 2025-06-22 19:55_

---

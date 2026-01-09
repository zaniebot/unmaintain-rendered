---
number: 4962
title: "Allow obtaining \"about\" text as a &str"
type: issue
state: closed
author: lufte
labels:
  - C-enhancement
assignees: []
created_at: 2023-06-10T14:55:57Z
updated_at: 2023-06-23T21:54:33Z
url: https://github.com/clap-rs/clap/issues/4962
synced_at: 2026-01-07T13:12:20-06:00
---

# Allow obtaining "about" text as a &str

---

_Issue opened by @lufte on 2023-06-10 14:55_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4

### Describe your use case

My GUI application has an internal shell with multiple clap::Commands. In some parts of the app I want to show a list of all commands and their descriptions, but the `get_about` method returns a `&StyledStr` that won't fit everywhere a `&str` does. 

### Describe the solution you'd like

I would like a reference to the internal `String` so I can pass it to my GUI library directly instead of having to allocate a new `String` every time with `StyledStr.to_string()` or having to keep a separate copy.

### Alternatives, if applicable

_No response_

### Additional Context

Previous discussion in https://github.com/clap-rs/clap/discussions/4961

---

_Label `C-enhancement` added by @lufte on 2023-06-10 14:55_

---

_Comment by @epage on 2023-06-12 16:47_

> My GUI application has an internal shell with multiple clap::Commands. In some parts of the app I want to show a list of all commands and their descriptions, but the get_about method returns a &StyledStr that won't fit everywhere a &str does.

In seeing more of your use case, I'm actually thinking a `as_styled_str` would not fill your needs.

`as_styled_str` may included styled text which you wouldn't want to render without converting to HTML or stripping.

Is there a reason you can't just do `cmd.get_about().to_string()` and then do the relevant pass a reference to that string around?  Granted, it could run into ownership issues depending on your architecture.

Yes, of course, in all of this you can make the assumption within your application that the text has no ANSI formatting (for now, for derive users we might do more formatting on their behalf) but I would like to focus on solving the general use case problem before narrowing down on suggesting a hack for your specific application.

---

_Comment by @lufte on 2023-06-12 17:40_

> Is there a reason you can't just do cmd.get_about().to_string() and then do the relevant pass a reference to that string around? Granted, it could run into ownership issues depending on your architecture.

There's no reason, that's actually what I'm doing now. It just occurred to me that it's a waste that I need to keep two copies of the same string for each command: one is separate from the command and the other is inside of it, but inaccessible. I'm assuming the Command's internal String is not mutated to apply format to it, in which case it wouldn't be possible to obtain a reference to the original one and this use case wouldn't make sense anymore.

---

_Comment by @epage on 2023-06-23 18:07_

In that case, I think I lean towards closing this.  Sorry for the extra hoop of an Issue though I think it did help us to pull out the extra information on this.

---

_Closed by @epage on 2023-06-23 18:07_

---

_Comment by @lufte on 2023-06-23 21:54_

No worries, it definitely helped. And it was a super minor thing either way. Thanks!

---

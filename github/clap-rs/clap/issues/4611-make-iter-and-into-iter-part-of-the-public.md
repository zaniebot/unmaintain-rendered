---
number: 4611
title: "Make `iter` and `into_iter` part of the public interface in `StyledStr`"
type: issue
state: closed
author: makspll
labels:
  - C-enhancement
assignees: []
created_at: 2023-01-08T12:02:23Z
updated_at: 2023-01-09T22:11:14Z
url: https://github.com/clap-rs/clap/issues/4611
synced_at: 2026-01-07T13:12:20-06:00
---

# Make `iter` and `into_iter` part of the public interface in `StyledStr`

---

_Issue opened by @makspll on 2023-01-08 12:02_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.0.32

### Describe your use case

I am using clap to power an in-game console, I'd rather not parse ansii characters back to colour information to format the text.

### Describe the solution you'd like

Make the iteration functionality of StyledStr part of the public interface

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @makspll on 2023-01-08 12:02_

---

_Referenced in [makspll/bevy-console#37](../../makspll/bevy-console/issues/37.md) on 2023-01-08 12:26_

---

_Comment by @epage on 2023-01-09 16:06_

The internals of `StyledStr` are not public because they are subject to change which will happen soon.  Our plan is to write ANSI codes directly into a `String` and strip them when needed, so you doing that on your side aligns with where we are going.

The original reason we are doing this is to allow users to provide formatted text without having to also maintaining a bespoke formatting API for #1433, #3108.  We also found that the current approach is very expensive in binary size terms which are we working to reduce (#1365).

---

_Comment by @makspll on 2023-01-09 21:14_

Hmm so would formatting be done by writing ANSI into the `StyledStr` or `String` directly? Is it at least going to be possible to force ansii output, without using the cross-platform capabilities of termcolor ?

---

_Comment by @epage on 2023-01-09 21:52_

`StyledStr` will be a newtype around `String` with the same API as today (ability to get a `Display` with and without ansi codes).

Users can use their crate of choice (e.g. owo-colors) to write ansi codes to a `StyledStr`.  We will either 
- (no tty) Strip those codes
- (unix) Write the codes
- (windows) Output the codes if supported, otherwise turn them into wincon calls

As most of the work of `termcolor` will not be needed, my plan is to write a minimal crate to support the above policy.

---

_Comment by @makspll on 2023-01-09 22:11_

I see, thanks!

---

_Closed by @makspll on 2023-01-09 22:11_

---

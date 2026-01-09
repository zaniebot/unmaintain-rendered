---
number: 5543
title: "DisplayVersion \"errors\" don't really allow to distinguish between -V and --version"
type: issue
state: open
author: glandium
labels:
  - C-enhancement
  - M-breaking-change
  - S-waiting-on-design
assignees: []
created_at: 2024-06-20T07:46:47Z
updated_at: 2024-09-29T04:32:57Z
url: https://github.com/clap-rs/clap/issues/5543
synced_at: 2026-01-07T13:12:20-06:00
---

# DisplayVersion "errors" don't really allow to distinguish between -V and --version

---

_Issue opened by @glandium on 2024-06-20 07:46_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.7

### Describe your use case

`-V` and `--version` come with a message that is essentially `"{name} {version}\n"`, where version is `version` for `-V` and `long_version` for `--version`. On top of this difference in output, one might want to print out some license text on `--version` but not `-V`, or do some other things.

It's technically possible to distinguish by parsing the output for `format!("{}", error.render())`, but that's not super nice.

### Describe the solution you'd like

A breaking change would be to have DisplayVersion and DisplayLongVersion. I guess since ErrorKind is non_exhaustive, adding DisplayLongVersion wouldn't be breaking as long as it's not emitted by default. (meaning it would require some flag to enable)

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @glandium on 2024-06-20 07:46_

---

_Comment by @epage on 2024-06-20 21:33_

Is there a reason you are attempting to detect this vs specifying a long-version?

---

_Comment by @glandium on 2024-06-20 22:59_

Because I want to actually do something different after displaying the version. Specifically, I want to make -V show the version and --version show the long version *and* check for updates. Well, I already did, by checking `format!("{}", error.render())`.
 Also, in the hypothetical case of showing licensing terms, that somehow feels wrong to put in long_version.

---

_Label `M-breaking-change` added by @epage on 2024-06-21 17:47_

---

_Label `S-waiting-on-design` added by @epage on 2024-06-21 18:17_

---

_Comment by @epage on 2024-06-21 18:18_

> Also, in the hypothetical case of showing licensing terms, that somehow feels wrong to put in long_version.

`long_version` exists to control what is shown for `--version`, so it fits.

> --version show the long version and check for updates

That is an interesting case.  Besides splitting up the ErrorKind, I am wondering what other options are.  Some I could think of
- Manually implement the flags separately
- Allow providing custom `ArgAction`s so a user can implement a custom "version" within clap (which might help with other cases like #815)

Can't think of too much else.

---

_Comment by @glandium on 2024-06-21 20:27_

> Manually implement the flags separately

I actually rejected that one for my use-case, because I'm deriving Parser on an enum (with subcommand_required=true), which doesn't leave space for that, except if I add another layer to handle --help and --version.

> Allow providing custom ArgActions so a user can implement a custom "version" within clap

That's a variant of "Manually implement the flags separately" isn't it?

---

_Comment by @epage on 2024-06-21 20:39_

> I actually rejected that one for my use-case, because I'm deriving Parser on an enum (with subcommand_required=true), which doesn't leave space for that, except if I add another layer to handle --help and --version.

Not too sure which part is the blocker here
- It shouldn't be too much to move `Parser` onto a `struct` that wraps your enum
- You can use `subcommand_required = true` with `Option<Subcommand>` so the derive works properly
- You can mark the version flag(s) as excluding all and global so they mostly get the existing behavior

> > Allow providing custom ArgActions so a user can implement a custom "version" within clap

> That's a variant of "Manually implement the flags separately" isn't it?

In the ideal case, the interface would allow for all existing actions to be implemented as if they were third party.  This would allow you to put all of that logic (license and update checks) in the call and then ask for the program to exit.

---

_Comment by @glandium on 2024-06-21 20:58_

> Not too sure which part is the blocker here

I didn't mean it was a blocker, but it's unappealing in the amount of boilerplate it requires for something that the gross hack I came up with dealt with in 2 lines of code.

---

_Comment by @sunshowers on 2024-09-29 04:32_

FWIW I ran into this too while trying to do something similar to @glandium. I agree that in the ideal world I'd be able to pass in a callback or something similar.

---

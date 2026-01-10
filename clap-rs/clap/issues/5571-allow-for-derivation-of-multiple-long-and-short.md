---
number: 5571
title: Allow for derivation of multiple long and short flags for the same argument
type: issue
state: closed
author: Dyredhead
labels:
  - C-enhancement
assignees: []
created_at: 2024-07-07T23:19:34Z
updated_at: 2024-07-09T03:43:01Z
url: https://github.com/clap-rs/clap/issues/5571
synced_at: 2026-01-10T01:28:13Z
---

# Allow for derivation of multiple long and short flags for the same argument

---

_Issue opened by @Dyredhead on 2024-07-07 23:19_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.7

### Describe your use case

In my program I want to be able to derive multiple long and short flags for the same arguments. 
So for example say I have a program that takes in the name of an ice cream, the long flag could be --ice-cream. 
But that is sometimes a little long to type out every time and unfortunately the -i flag is already taken for another argument.

 It would be useful to be able to derive more than one long flag for a single argument. In this case that could be "--ic" and "--ice-cream".

### Describe the solution you'd like

The most obvious two solutions that comes to mind is to just allow Arg::long to be derived more than once:
![Screenshot_20240707_185601](https://github.com/clap-rs/clap/assets/24397516/0a6f0460-cb2e-4b28-9fa0-09da5560bb0c)
or to change Arg::long to take in a Vec<&str> rather than just a <&str> 
![Screenshot_20240707_185656](https://github.com/clap-rs/clap/assets/24397516/70f0708d-f95c-4521-b1cc-f60e876cc759)

I personally think the first one looks and feels better, and it also allows for clearly defined order, which can then be translated to the help dialogue.  

An example is how this would look is how what eza does in its manpage:
![Screenshot_20240707_190011](https://github.com/clap-rs/clap/assets/24397516/9acb2c31-a191-449b-a28f-3df2077ea81e)
and this would also be how the help dialogue looks like.  

Since changing the type of Arg::long is probably a breaking change, there could be a new type introduced, such as Arg::long_list which would implement this feature instead:
![Screenshot_20240707_190351](https://github.com/clap-rs/clap/assets/24397516/eedabffd-59df-49a0-beae-e0fb7cecb098)

If possible to implement, then the same should probably be done for the short flags as well.  

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @Dyredhead on 2024-07-07 23:19_

---

_Comment by @Dyredhead on 2024-07-07 23:32_

I see that this is possible using the visible aliases, but that only seems to be available for the builder pattern, and not the derive pattern. So i guess the questions is whether or not this is possible to do with the derive pattern?

---

_Closed by @Dyredhead on 2024-07-07 23:32_

---

_Reopened by @Dyredhead on 2024-07-07 23:32_

---

_Comment by @epage on 2024-07-08 15:06_

> but that only seems to be available for the builder pattern, and not the derive pattern

How so?

From [the derive reference](https://docs.rs/clap/latest/clap/_derive/index.html#terminology)

> Raw attributes are forwarded directly to the underlying [clap builder](https://docs.rs/clap/latest/clap/builder/index.html). Any [Command](https://docs.rs/clap/latest/clap/struct.Command.html), [Arg](https://docs.rs/clap/latest/clap/struct.Arg.html), or [PossibleValue](https://docs.rs/clap/latest/clap/builder/struct.PossibleValue.html) method can be used as an attribute.

As for making every builder method explicitly specified in the derive reference, see https://github.com/clap-rs/clap/discussions/4090

---

_Comment by @Dyredhead on 2024-07-08 19:38_

Ok I see, thank you.
I guess my question now is:
Is it possible to have the alias appear alongside the actual flags, rather than at the end of the doc comment in a 
`[aliases: ]`
Im trying to achieve a similar look to how `eza` does it

---

_Comment by @epage on 2024-07-08 19:54_

At this time, no.  That is being discussed in #5459.

---

_Closed by @Dyredhead on 2024-07-09 03:43_

---

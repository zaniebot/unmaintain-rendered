```yaml
number: 5377
title: Allow longer shorts
type: issue
state: open
author: h8sd
labels:
  - C-enhancement
  - A-parsing
  - S-waiting-on-design
assignees: []
created_at: 2024-02-26T08:31:59Z
updated_at: 2024-02-27T06:25:31Z
url: https://github.com/clap-rs/clap/issues/5377
synced_at: 2026-01-12T16:14:17Z
```

# Allow longer shorts

---

_@h8sd_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.1

### Describe your use case

Let `short` flag allow more characters than one to facilitate user input.

I know that short flags implement their combined behavior by default in clap, which prevents `short` flags from using more than two characters.
For example, `ls -l-a = ls -la`, which I think is a great feature.

However, implementing only this functionality ignores the case where the user does not want to enter the `long` flag if there are too many parameters.
For example, the user wants to type `xxx-oJ` instead of `xxx --oJ`

### Describe the solution you'd like

I have a solution, which is that clap can open a method to control whether I want this default composition behavior or not.

When I turn off the composition behavior, the `short` flag length is at least 1 or greater.
For example,` -oJ`, the output is json, there is no combined behavior here, just a single instance.

When I enable the composition behavior (which is the default), the `short` flag is limited to a length of 1.
Which is now the composition behavior.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @h8sd on 2024-02-26 08:31_

---

_Renamed from "Allow longer short with the addition of closed composite behavior methods" to "Allow longer shorts" by @epage on 2024-02-26 15:25_

---

_Label `A-parsing` added by @epage on 2024-02-26 15:29_

---

_Label `S-waiting-on-design` added by @epage on 2024-02-26 15:29_

---

_Comment by @epage on 2024-02-26 15:32_

Renamed the title to focus on the problem, rather than solution.

I know this has been discussed before but not finding the Issues or Discussions for it.  #1210 is either similar or the same thing, depending on your perspective.

There are several problems to solve
- API: With clap v3, we intentionally switched `short` from accepting `&str` to `char` to better represent this limitation in type system
- Behavior: we have to deal with people who want true shorts with combining of multiple flags + a value with people who want multi-character shorts.    There might even people who want both.
  - If its both, that adds its own complexities.
  - If its either, then we have to balance the scope of how much this is needed vs the compile time and binary size of making this large of a behavior swap conditional
- Implementation: this is a dramatic difference at the core of clap/s lexing and parsing

If this is more like #1210, then #2468 is likely the way forward.

---

_Comment by @h8sd on 2024-02-27 06:25_

I noticed that in v2, clap short allowed multiple characters, but only the first one was retrieved.
For example, `short("abc")`  appears as -a on the command line.

>  API: With clap v3, we intentionally switched `short` from accepting `&str` to `char` to better represent this limitation in type system

I think developing a way to regulate whether or not composition behavior is used is a very important feature for developers and users.
The right sacrifice can sometimes be a big help.
>Behavior: we have to deal with people who want true shorts with combining of multiple flags + a value with people who want multi-character shorts. There might even people who want both.
>* If its both, that adds its own complexities.
>* If its either, then we have to balance the scope of how much this is needed vs the compile time and binary size of making this large of a behavior swap conditional

I am aware of these issues and hope such issues can be resolved.

>If this is more like https://github.com/clap-rs/clap/issues/1210, then https://github.com/clap-rs/clap/issues/2468 is likely the way forward.

Finally, I wish clap better and better!



---

_Referenced in [PowerShell/DSC#983](../../PowerShell/DSC/issues/983.md) on 2025-07-23 02:10_

---

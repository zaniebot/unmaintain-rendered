---
number: 2790
title: Allow Windows users to opt into color support
type: issue
state: closed
author: pie-flavor
labels: []
assignees: []
created_at: 2021-09-24T19:00:29Z
updated_at: 2021-09-29T18:20:02Z
url: https://github.com/clap-rs/clap/issues/2790
synced_at: 2026-01-07T13:12:19-06:00
---

# Allow Windows users to opt into color support

---

_Issue opened by @pie-flavor on 2021-09-24 19:00_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

2.33.3

### Describe your use case

Clap currently automatically disables coloring support on Windows because Windows has not historically supported ANSI color codes and clap doesn't feel like using crossterm instead. However, the current version of Windows does support ANSI codes, and furthermore statically linking the MSVCRT on the current version will let the built binary support colors on prior versions. I am on the current version of Windows, so I would like to be able to use the coloring support Clap already has.

### Describe the solution you'd like

Add an opt-in feature `force-colors` that will prevent auto-disabling ANSI colors on Windows.

### Alternatives, if applicable

Use an arbitrary cfg flag instead, so that only the binary's build environment can set it and not their dependencies.

### Additional Context

_No response_

---

_Label `T: new feature` added by @pie-flavor on 2021-09-24 19:00_

---

_Comment by @epage on 2021-09-24 19:05_

This should be addressed in master / v3.  We switched to termcolor.

---

_Closed by @pksunkara on 2021-09-24 19:10_

---

_Comment by @pie-flavor on 2021-09-29 17:36_

I hadn't seen that, great! However, it would be great if this behavior was back ported to v2 as well, which would probably take the shape I described.

---

_Comment by @epage on 2021-09-29 18:20_

My understanding is we're only backporting critical fixes.  To put in context, its been over a year since the last commit to `v2-master`.

---

_Referenced in [kherge/rs.aws-login#13](../../kherge/rs.aws-login/issues/13.md) on 2022-01-02 06:22_

---

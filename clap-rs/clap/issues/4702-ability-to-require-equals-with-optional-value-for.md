```yaml
number: 4702
title: Ability to require_equals with optional value for long option, but require value for short option.
type: issue
state: open
author: tmccombs
labels:
  - C-enhancement
  - A-parsing
  - S-waiting-on-design
assignees: []
created_at: 2023-02-11T06:34:16Z
updated_at: 2023-02-13T16:04:23Z
url: https://github.com/clap-rs/clap/issues/4702
synced_at: 2026-01-12T16:14:16Z
```

# Ability to require_equals with optional value for long option, but require value for short option.

---

_@tmccombs_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.0

### Describe your use case

`mktemp` has the following option:

```
-p DIR, --tmpdir[=DIR]
```

If using the short option, `DIR` is required. But if using the long option, `DIR` is optional, and requires the "=". 

### Describe the solution you'd like

I'm not really sure. Perhaps the cleanest solution would be to be either some way to use the same id, and help text for multiple Args, or to have some way to provide separate options for the short and long variants.

### Alternatives, if applicable

It's possible to work around this by having separate `Arg`s for the short and long options. But this can be a little awkward to work with when consuming the arguments, and there isn't a way to combine the help text for the two options.

### Additional Context

See https://github.com/uutils/coreutils/issues/3454

---

_Label `C-enhancement` added by @tmccombs on 2023-02-11 06:34_

---

_Referenced in [uutils/coreutils#4342](../../uutils/coreutils/pulls/4342.md) on 2023-02-11 06:38_

---

_Comment by @epage on 2023-02-13 16:03_

I feel like this came up elsewhere but I'm not finding it.    Maybe its buried in #3030.

There is a sibling request of #4499.

---

_Label `A-parsing` added by @epage on 2023-02-13 16:03_

---

_Label `S-waiting-on-design` added by @epage on 2023-02-13 16:03_

---

_Comment by @epage on 2023-02-13 16:04_

My main concern is ensuring we give the user the ability to create the policy they need without bloating compile times, binary size, or the API.

Probably the first step in this is designing out the API.

---

_Referenced in [clap-rs/clap#3008](../../clap-rs/clap/issues/3008.md) on 2023-05-03 14:14_

---

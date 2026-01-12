```yaml
number: 6059
title: "Suggestion: formatting help for [default] values with comma seperator instead a spaces"
type: issue
state: closed
author: GilShoshan94
labels:
  - C-enhancement
  - S-waiting-on-decision
  - A-help
assignees: []
created_at: 2025-07-03T06:40:27Z
updated_at: 2025-07-03T16:37:13Z
url: https://github.com/clap-rs/clap/issues/6059
synced_at: 2026-01-12T16:14:17Z
```

# Suggestion: formatting help for [default] values with comma seperator instead a spaces

---

_@GilShoshan94_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4

### Describe your use case

We can have a list of default values for an argument (see [Arg::default_values](https://docs.rs/clap/latest/clap/struct.Arg.html#method.default_values)).

It is not entirely clear to me the concrete use case but when it happens, I see from the code in `help_template.rs` that the list is joined with simple space `.join(" ")`.

Shouldn't it be separated by commas `.join(", ")` instead like it is the case for `aliases` and `possible values` ?
It feels inconsistent to me right now.

If you want @epage I can do a quick PR for that.
But it could be considered a breaking change and maybe you prefer to leave it for the 5.0 ?

### Describe the solution you'd like

Separated by commas `.join(", ")` instead like it is the case for `aliases` and `possible values

### Alternatives, if applicable

_No response_

### Additional Context

(Encounter during PR #6057)

---

_Label `C-enhancement` added by @GilShoshan94 on 2025-07-03 06:40_

---

_Referenced in [clap-rs/clap#6057](../../clap-rs/clap/pulls/6057.md) on 2025-07-03 06:41_

---

_Label `A-help` added by @epage on 2025-07-03 14:14_

---

_Comment by @epage on 2025-07-03 14:15_

So we can have
```
[default values: one two]
[aliases: -?, --help-all]
[possible values: one, two, three]
```

The difference between these is that aliases and possible values are "one of these" while default values are "all of these" and default values are being listed like they would on the command line (e.g. `mycli one two`).

---

_Label `S-waiting-on-decision` added by @epage on 2025-07-03 14:15_

---

_Referenced in [clap-rs/clap#5925](../../clap-rs/clap/issues/5925.md) on 2025-07-03 14:36_

---

_Comment by @GilShoshan94 on 2025-07-03 16:37_

I must admit that I didn't understand what it meant to have several default values for one argument.
I was confused and was thinking:
_If the argument is not present in the command line args, it defaults to all those values? Or to a vector of those values?_

I could not find a practical example of [default_values](https://docs.rs/clap/latest/clap/struct.Arg.html#method.default_values).

But now I think I understand, if an argument is a vector and thus multiple values are allowed such `--option val1 val2 val3`, you can set the default to a list of values with `default_values`.

It is still one default value => a list of values.

I understand better now, I think.

I agree with you; I would not separate the list with comma since comma indicates "one of these".

I am closing this issue now as I don't see the need to change anything.

---

_Closed by @GilShoshan94 on 2025-07-03 16:37_

---

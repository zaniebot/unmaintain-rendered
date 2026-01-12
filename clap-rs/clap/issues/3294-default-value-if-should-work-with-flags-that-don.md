```yaml
number: 3294
title: "default_value_if should work with flags that don't take a value"
type: issue
state: closed
author: lefth
labels:
  - C-enhancement
  - A-derive
  - S-triage
assignees: []
created_at: 2022-01-15T05:38:13Z
updated_at: 2022-06-01T12:07:27Z
url: https://github.com/clap-rs/clap/issues/3294
synced_at: 2026-01-12T16:14:14Z
```

# default_value_if should work with flags that don't take a value

---

_@lefth_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.7

### Describe your use case

I want to have various specific flags imply a general flag, for example `--mystery` or `--suspense` implies `--fiction`. I don't want to require an extra "=true" on the flags.

### Describe the solution you'd like

Clap can almost support this use case: `fiction` should have `default_value_ifs` stating that any `mystery` or `suspense` argument gives the default value of `true`, and `takes_value` is false. However, `default_value_if` and `default_value_ifs` seem to need `takes_value`. They set it implicitly, and when it is turned off, the implication does not happen.

Or if the right answer is to use `min_value(0)`, that should be documented (see below).

### Alternatives, if applicable

An alternative solution is to override `parse`, `parse_from`, `try_parse`, and `try_parse_from` (and call `<Self as Parser>::*parse*`) and call a validation function to set implied values.

Another alternative solution would be to let the struct or app define a post-parse hook (the validation function mentioned above).

If "true" and "false" are not valid positional values, the easiest workaround is to use `min_values(0) ` instead of `takes_value(false)`. If this is the best solution for now, I suggest adding a note to the `takes_value` docs such as: "Note that if `takes_value(false)` cannot be used due to a default value being set by another argument's presence, `min_values(0)` is a reasonable substitute."

### Additional Context

This issue is shown in rust playground here: https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=4855a88381f65cef8d07f7eab4d41e78

---

_Label `C-enhancement` added by @lefth on 2022-01-15 05:38_

---

_Comment by @epage on 2022-01-17 15:51_

So there are ways around this in the builder API but this was reported against the derive API.

I have been considering re-doing flags so that they set a value, rather than relying on them being preset or not which would resolve this (and simplify several other APIs).

---

_Label `A-derive` added by @epage on 2022-01-17 15:51_

---

_Label `S-triage` added by @epage on 2022-01-17 15:51_

---

_Referenced in [clap-rs/clap#3775](../../clap-rs/clap/pulls/3775.md) on 2022-06-01 02:26_

---

_Closed by @epage on 2022-06-01 12:07_

---

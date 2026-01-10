---
number: 5698
title: "Add `default_values_if` to match `default_values`"
type: issue
state: closed
author: SuspiciousDuck
labels:
  - C-enhancement
  - A-builder
  - A-parsing
  - E-easy
assignees: []
created_at: 2024-08-24T05:13:40Z
updated_at: 2025-11-19T20:49:39Z
url: https://github.com/clap-rs/clap/issues/5698
synced_at: 2026-01-10T01:28:15Z
---

# Add `default_values_if` to match `default_values`

---

_Issue opened by @SuspiciousDuck on 2024-08-24 05:13_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.16

### Describe your use case

I have an argument which takes a tuple. I also have a previous argument which is a `String`. I want to create 2 different defaults, depending on what the `String` is. This is impossible to implement because `default_value_if` does not accept a list [] for the value.

### Describe the solution you'd like

I would like to see `default_values_if` and or `default_values_ifs` implemented.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @SuspiciousDuck on 2024-08-24 05:13_

---

_Label `A-builder` added by @epage on 2024-08-25 01:56_

---

_Label `A-parsing` added by @epage on 2024-08-25 01:56_

---

_Label `S-waiting-on-design` added by @epage on 2024-08-25 01:56_

---

_Renamed from "default_values_if" to "Add `default_values_if` to match `default_values`" by @epage on 2025-08-07 20:14_

---

_Label `S-waiting-on-design` removed by @epage on 2025-08-07 20:14_

---

_Label `E-easy` added by @epage on 2025-08-07 20:14_

---

_Comment by @epage on 2025-08-07 20:15_

Sorry for not responding earlier.  Looking at this again, it makes sense for us to update the conditional to support multiple values like the unconditional version.

---

_Comment by @cooronx on 2025-09-08 07:12_

hi @epage , I’d like to work on this issue as my first contribution to clap. Could you point me to where in the codebase I should start and any tests/examples to check the result? 

---

_Comment by @epage on 2025-09-08 15:47_

I'd recommend starting with the [`default_value_if`](https://docs.rs/clap/latest/clap/struct.Arg.html#method.default_value_if) and [`default_value_ifs`](https://docs.rs/clap/latest/clap/struct.Arg.html#method.default_value_ifs).  From there, you see that the field you'll be working with is
https://github.com/clap-rs/clap/blob/2c04acd3607e5c4676477ca14948419bb31c73a1/clap_builder/src/builder/arg.rs#L84

You'll need to update that to be similar to 
https://github.com/clap-rs/clap/blob/2c04acd3607e5c4676477ca14948419bb31c73a1/clap_builder/src/builder/arg.rs#L83

and then update the callers to be similar to `default_value` / `default_values`.

The compiler errors should then also point to you were in the parser this is used which is
https://github.com/clap-rs/clap/blob/2c04acd3607e5c4676477ca14948419bb31c73a1/clap_builder/src/parser/parser.rs#L1448
and update that in a similar way to `default_values` below it
https://github.com/clap-rs/clap/blob/2c04acd3607e5c4676477ca14948419bb31c73a1/clap_builder/src/parser/parser.rs#L1484-L1512

As for `examples/`, we don't provide those for every feature

As for tests, if you search in `tests/` you'll see that we generally group things and `default_value_if` tests are in 
https://github.com/clap-rs/clap/blob/master/tests/builder/default_vals.rs
Add tests similar to what we do for `default_value_if` and `default_values`

---

_Comment by @cooronx on 2025-09-10 09:06_

@epage which branch I should be working on(fork)?  currently working in the master branch, not sure if it's correct

---

_Comment by @epage on 2025-09-10 13:49_

All development happens on master.

---

_Referenced in [clap-rs/clap#6127](../../clap-rs/clap/pulls/6127.md) on 2025-09-13 14:03_

---

_Referenced in [clap-rs/clap#6129](../../clap-rs/clap/pulls/6129.md) on 2025-09-13 14:22_

---

_Closed by @epage on 2025-11-19 20:49_

---

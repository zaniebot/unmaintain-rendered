---
number: 3726
title: "derive error for missing `= value` for attributes is confusing"
type: issue
state: open
author: ravenexp
labels:
  - C-enhancement
  - E-easy
  - A-derive
assignees: []
created_at: 2022-05-13T07:12:27Z
updated_at: 2022-08-11T17:02:06Z
url: https://github.com/clap-rs/clap/issues/3726
synced_at: 2026-01-10T01:27:45Z
---

# derive error for missing `= value` for attributes is confusing

---

_Issue opened by @ravenexp on 2022-05-13 07:12_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.1.18

### Describe your use case

I'm trying to add an environment variable fallback for the `--password` option. I do not want the variable value to be accidentally revealed when `--help` is used.

I tried to write
```
#[clap(short, long, env = "PROGRAM_PASSWORD", hide_env_values)]
password: Option<String>,
```
but the error made me think that the attribute doesn't exist when it does:
```
error: unexpected attribute: hide_env_values
```
(Note: #3620 is for bool shorthand support)

### Describe the solution you'd like

A clearer error message

### Alternatives, if applicable



### Additional Context

_No response_

---

_Label `C-enhancement` added by @ravenexp on 2022-05-13 07:12_

---

_Referenced in [PyO3/maturin#915](../../PyO3/maturin/pulls/915.md) on 2022-05-13 08:49_

---

_Comment by @epage on 2022-05-13 12:01_

Boolean builder functions do not have an implied ` = true` but you have to set it manually.  The following should work
```rust
#[clap(short, long, env = "PROGRAM_PASSWORD", hide_env_values = true)]
password: Option<String>,
```

See https://github.com/clap-rs/clap/issues/3620

---

_Comment by @ravenexp on 2022-05-13 12:13_

Thanks! I should've tried that before opening an issue :face_exhaling: 

I was getting
```
error: unexpected attribute: hide_env_values
```

when compiling, and it is not mentioned anywhere in [the documentation](https://github.com/clap-rs/clap/blob/v3.1.18/examples/derive_ref/README.md#arg-attributes), so I just assumed that this attribute was not implemented.

---

_Comment by @epage on 2022-05-13 13:48_

Thats an artifact of how we parse the attributes.  It isn't in our hard coded list of attributes that work without a `=` or `()`.  It would be nice to improve that message but unsure what to say. 

---

_Renamed from "`hide_env_values()` can't be used via the Derive API" to "derive error for missing `= value` for attributes is confusing" by @epage on 2022-05-13 13:48_

---

_Label `E-easy` added by @epage on 2022-05-13 13:48_

---

_Label `A-derive` added by @epage on 2022-05-13 13:48_

---

_Comment by @danielparks on 2022-07-31 05:02_

Would you be opposed to adding the boolean attributes to the list that are accepted without a value? I imagine they all could be treated as true, but I would go through them and check. (To be clear, I’m volunteering.)

Otherwise, I suppose the error could be something like:

    error: unexpected attribute without value: hide_env_values

I‘m not sure how much clearer that is, though.

---

_Comment by @epage on 2022-08-01 17:07_

@danielparks see https://github.com/clap-rs/clap/issues/3620

---

```yaml
number: 4904
title: "First example of Arg::default_value_if uses the wrong predicate"
type: issue
state: open
author: piksel
labels:
  - C-bug
  - A-docs
assignees: []
created_at: 2023-05-14T16:20:44Z
updated_at: 2023-05-18T18:57:28Z
url: https://github.com/clap-rs/clap/issues/4904
synced_at: 2026-01-12T16:14:16Z
```

# First example of Arg::default_value_if uses the wrong predicate

---

_@piksel_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.69.0 (84c898d65 2023-04-16)

### Clap Version

4.2.2 (also confirmed present in master)

### Minimal reproducible code

Not applicable.

### Steps to reproduce the bug with the above code

- Read the examples in docs for [`Arg::default_value_if`](https://docs.rs/clap/latest/clap/builder/struct.Arg.html#method.default_value_if).
- Even though the docs says "using the same test", the predicate is changed in the second example (probably because otherwise it would not pass the assertion).


### Actual Behaviour

```rs
.default_value_if("flag", ArgPredicate::IsPresent, Some("default")))
```
The predicate used in the first example doesn't actually do anything, as it sets the default value regardless of whether the flag is passed.

### Expected Behaviour

Should use the same predicate as the second example:
```rs
.default_value_if("flag", "true", Some("default")))
```

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @piksel on 2023-05-14 16:20_

---

_Referenced in [clap-rs/clap#4918](../../clap-rs/clap/issues/4918.md) on 2023-05-18 18:54_

---

_Comment by @epage on 2023-05-18 18:57_

Looks like we have a bug and opened #4918 and the documentation was partially updated to as part of mass updates.

We could do short-term workaround but I'd rather wait for #4918

---

_Label `A-docs` added by @epage on 2023-05-18 18:57_

---

_Added to milestone `5.0` by @epage on 2023-05-18 18:57_

---

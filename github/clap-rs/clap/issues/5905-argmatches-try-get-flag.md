---
number: 5905
title: "ArgMatches::try_get_flag"
type: issue
state: open
author: Lucretiel
labels:
  - C-enhancement
assignees: []
created_at: 2025-02-07T22:38:28Z
updated_at: 2025-02-10T15:21:51Z
url: https://github.com/clap-rs/clap/issues/5905
synced_at: 2026-01-07T13:12:20-06:00
---

# ArgMatches::try_get_flag

---

_Issue opened by @Lucretiel on 2025-02-07 22:38_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

  4.5

### Describe your use case

I'm dynamically populating a `Command`, based on the contents of a struct. Because it's happening dynamically, I'm using the `try` versions of the getters on `ArgMatches`, to handle the case where things happened incorrectly. However, there's no `try` version of `get_flag`, which I use for my bool fields.

### Describe the solution you'd like

Would like to have a fallible `try_get_flag` on `ArgMatches`, to compliment all of the other `try_` versions of things. Currently there's no panic-free way to get an argument that you're expecting to be a `SetTrue`.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @Lucretiel on 2025-02-07 22:38_

---

_Comment by @epage on 2025-02-07 22:58_

Is there a reason you aren't using `try_get_one`?  `get_flag` is just a convenience helper for `get_one`.  I lean towards people using `try_get_one` when they need more rather than adding many variants of helpers (also not a fan of all of the `get_matches` / `parse` helpers and been considering reducing those)

---

_Comment by @Lucretiel on 2025-02-08 21:51_

I thought (because of the `one`) that `try_get_one` returns the value for an option that takes a single argument; that is, it returns `Some` when `--arg value`, `None` when `--arg` is omitted entirely, and an error otherwise. I haven't been able to find any precise documentation about the underlying "data model" used by `ArgMatches`; how it's populated from a `Command`, and *specifically* how the various `get_*` and `try_get`_*  methods interpret the stored data.

---

_Comment by @epage on 2025-02-10 15:21_

In clap v3, flags were a special case without a value in `ArgMatches`, instead users having to check for its presence.  In clap v4, we transition to always setting a value in `ArgMatches`, hence the names like `SetTrue` and `SetFalse` for flag `ArgAction`.

In fact, `SetTrue` is just a convenience helper for `Set` that implies
```rust
arg
  .value_parser(BoolValueParser::new())
  .default_missing_value("true")
  .default_value("false")
  .num_args(0)
```

so `get_flag` is a helper for `get_one` (used for `Set`) which is a helper for `get_many` (used for `Append`, `num_args`)

---

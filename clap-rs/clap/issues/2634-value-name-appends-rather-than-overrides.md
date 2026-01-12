```yaml
number: 2634
title: "`value_name` appends, rather than overrides"
type: issue
state: closed
author: epage
labels:
  - C-bug
assignees: []
created_at: 2021-07-28T14:52:19Z
updated_at: 2021-08-13T21:45:59Z
url: https://github.com/clap-rs/clap/issues/2634
synced_at: 2026-01-12T16:14:13Z
```

# `value_name` appends, rather than overrides

---

_@epage_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.53.0 (53cb7b09b 2021-06-17)

### Clap Version

3.0.0-beta.2

### Minimal reproducible code

#[clap(
    number_of_values = 1,
    long = "using",
    value_name = "pm",
    value_name = "foo",
)]
using: Option<String>,

### Steps to reproduce the bug with the above code

Run it

### Actual Behaviour

- `pm` and `foo` show up in `--help`
- Clap errors if there aren't two arguments

### Expected Behaviour

- Last one wins

### Additional Context

Split out of #2632.  Depending on what solution we go with #2633 could be simplified

There is a separate `value_names` that is documented for providing multiple.  That also appends

**This is either a bug in the documentation or in the code.**

Related: documentation also says `value_name` is cosmetic only but `value_name`, under certain situations, implies `number_of_values`.

Most of the time, people will not run into this.  Its when programmatically creating an argument, like in the derive macro.

### Debug Output

_No response_

---

_Label `T: bug` added by @epage on 2021-07-28 14:52_

---

_Added to milestone `3.0` by @pksunkara on 2021-07-30 09:32_

---

_Label `C: args` added by @pksunkara on 2021-08-02 08:56_

---

_Comment by @epage on 2021-08-13 18:16_

@pksunkara any opinion on whether this is a documentation or implementation bug?

---

_Comment by @pksunkara on 2021-08-13 19:01_

> Related: documentation also says value_name is cosmetic only

Docs say that the **name** is cosmetic. Not the function.

> This is either a bug in the documentation or in the code.

While most of the other methods just extend existing data, `default_value` and `default_values` which is probably the most similar to these functions override. That is probably what we want to do here too.

---

_Referenced in [clap-rs/clap#2690](../../clap-rs/clap/pulls/2690.md) on 2021-08-13 20:23_

---

_Closed by @pksunkara on 2021-08-13 21:45_

---

_Referenced in [solana-labs/solana-program-library#4511](../../solana-labs/solana-program-library/issues/4511.md) on 2023-06-08 23:33_

---

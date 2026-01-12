```yaml
number: 4009
title: "Documentation error: `ArgEnum` vs `ValueEnum`"
type: issue
state: closed
author: mfreeborn
labels:
  - C-bug
assignees: []
created_at: 2022-07-31T10:09:17Z
updated_at: 2022-08-01T20:34:43Z
url: https://github.com/clap-rs/clap/issues/4009
synced_at: 2026-01-12T16:14:15Z
```

# Documentation error: `ArgEnum` vs `ValueEnum`

---

_@mfreeborn_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.62.1

### Clap Version

3.2.16

### Minimal reproducible code

The first code block in this section, I believe, has an error: https://docs.rs/clap/latest/clap/_derive/_tutorial/index.html#enumerated-values

Specifically `ArgEnum` being derived on the `Mode` enum.

### Steps to reproduce the bug with the above code

.

### Actual Behaviour

.

### Expected Behaviour

I think `ArgEnum` in the code block should be `ValueEnum`

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @mfreeborn on 2022-07-31 10:09_

---

_Comment by @epage on 2022-08-01 14:02_

`ArgEnum` is a deprecated name for `ValueEnum`.  v4 work has started in master and it looks like I missed removing this deprecation; that will be addressed soon.

If someone feels the need to backport this to v3-master, let us know!

---

_Referenced in [clap-rs/clap#4008](../../clap-rs/clap/issues/4008.md) on 2022-08-01 18:11_

---

_Referenced in [clap-rs/clap#4015](../../clap-rs/clap/pulls/4015.md) on 2022-08-01 20:15_

---

_Closed by @epage on 2022-08-01 20:34_

---

_Referenced in [clap-rs/clap#4016](../../clap-rs/clap/pulls/4016.md) on 2022-08-01 20:43_

---

```yaml
number: 4400
title: "Allow using `ValueHint::CommandWithArguments` with `last` (without `trailing_var_args`)"
type: issue
state: closed
author: KSXGitHub
labels:
  - C-enhancement
assignees: []
created_at: 2022-10-18T11:49:36Z
updated_at: 2022-10-18T12:43:13Z
url: https://github.com/clap-rs/clap/issues/4400
synced_at: 2026-01-12T16:14:16Z
```

# Allow using `ValueHint::CommandWithArguments` with `last` (without `trailing_var_args`)

---

_@KSXGitHub_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.0.15

### Describe your use case

I want to a program like this:

```sh
my_cli file1 file2 -- command_name arg1 arg2
```

I also want to generate ZSH completions.

However, `ValueHint::CommandWithArguments` requires the use of `trailing_var_args` which can't be used with `last`.

### Describe the solution you'd like

This code should be valid:

```rust
#[derive(Parser)]
struct MyCli {
  files: Vec<PathBuf>,
  #[clap(last = true, value_hint = ValueHint::CommandWithArguments)]
  command: Vec<OsString>,
}
```

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @KSXGitHub on 2022-10-18 11:49_

---

_Referenced in [clap-rs/clap#4401](../../clap-rs/clap/pulls/4401.md) on 2022-10-18 12:29_

---

_Closed by @epage on 2022-10-18 12:41_

---

_Closed by @epage on 2022-10-18 12:41_

---

_Comment by @epage on 2022-10-18 12:43_

v4.0.17 is released with this fixed

---

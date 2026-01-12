```yaml
number: 3359
title: "`clap_man` doesn't show the right number of value names"
type: issue
state: open
author: epage
labels:
  - C-bug
  - E-help-wanted
  - E-easy
  - A-man
assignees: []
created_at: 2022-01-28T21:21:51Z
updated_at: 2022-06-16T21:11:51Z
url: https://github.com/clap-rs/clap/issues/3359
synced_at: 2026-01-12T16:14:14Z
```

# `clap_man` doesn't show the right number of value names

---

_@epage_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.57.0 (f1edd0429 2021-11-29)

### Clap Version

3.0.0-rc.0

### Minimal reproducible code

TBD

### Steps to reproduce the bug with the above code

TBD

### Actual Behaviour

When `--help` renders value_names, i think it repeats the last one until we've hit the max number of values

### Expected Behaviour

We literally show the value names as returned by the `Arg`

### Additional Context

Split out of #3174

### Debug Output

_No response_

---

_Label `C-bug` added by @epage on 2022-01-28 21:21_

---

_Label `E-easy` added by @epage on 2022-01-28 21:21_

---

_Label `A-man` added by @epage on 2022-01-28 21:21_

---

_Label `E-help-wanted` added by @epage on 2022-06-16 21:11_

---

---
number: 2736
title: "`clap_app!` documentation example is not accurate."
type: issue
state: closed
author: Shaddy
labels:
  - A-docs
assignees: []
created_at: 2021-08-25T15:46:29Z
updated_at: 2021-09-04T09:34:16Z
url: https://github.com/clap-rs/clap/issues/2736
synced_at: 2026-01-10T01:27:24Z
---

# `clap_app!` documentation example is not accurate.

---

_Issue opened by @Shaddy on 2021-08-25 15:46_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

2.33.3

### Where?

https://docs.rs/clap/2.33.3/clap/#quick-example

### What's wrong?

The documentation uses an example using three different clap app (default/hybrid, YAML, and macro) however, the `clap_app!` doesn't produce the example output.

```
OPTIONS:
    -c, --config <FILE>    Sets a custom config file
```

This option doesn't contain `<FILE>` but `<CONFIG>` because the macro version doesn't define a `value_name`.

Also, `-d` flag is missing.

### How to fix?

Use 

```
(@arg config: -c --config +takes_value value_name("FILE") "Sets a custom config file")
```

Instead of 

```
(@arg CONFIG: -c --config +takes_value "Sets a custom config file")
```

---

_Label `C: docs` added by @Shaddy on 2021-08-25 15:46_

---

_Comment by @Shaddy on 2021-08-25 16:11_

Actually I just realize that this is even better:

```
(@arg config: -c --config <FILE> +takes_value "Sets a custom config file")
```

---

_Comment by @Shaddy on 2021-08-26 11:16_

Also, doesn't `value_name / <VALUE_NAME>` make `takes_value(true)` redundant?

---

_Comment by @Shaddy on 2021-09-02 12:04_

Well, using `<FILE>` is not exactly the same as `value_name("FILE")`, the first implies +required (surprisingly) while the latter doesn't.

---

_Closed by @pksunkara on 2021-09-04 09:34_

---

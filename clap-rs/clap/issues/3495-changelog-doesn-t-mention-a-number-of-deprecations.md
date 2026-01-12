```yaml
number: 3495
title: "Changelog doesn't mention a number of deprecations"
type: issue
state: closed
author: glandium
labels:
  - C-bug
assignees: []
created_at: 2022-02-22T02:14:46Z
updated_at: 2022-02-28T17:48:00Z
url: https://github.com/clap-rs/clap/issues/3495
synced_at: 2026-01-12T16:14:15Z
```

# Changelog doesn't mention a number of deprecations

---

_@glandium_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.58.1 (db9d1b20b 2022-01-20)

### Clap Version

3.1.1

### Minimal reproducible code

Code using `ArgSettings::AllowInvalidUtf8`, `AppSettings::ArgRequiredElseHelp`, `AppSettings::DontCollapseArgsInUsage` or `AppSettings::SubcommandRequired`.

### Steps to reproduce the bug with the above code

Compile aforementioned code

### Actual Behaviour

Deprecation warnings are displayed such as:
```
warning: use of deprecated variant `clap::ArgSettings::AllowInvalidUtf8`: Replaced with `Arg::allow_invalid_utf8` and `Arg::is_allow_invalid_utf8_set`
warning: use of deprecated variant `clap::AppSettings::ArgRequiredElseHelp`: Replaced with `Command::arg_required_else_help` and `Command::is_arg_required_else_help_set`
warning: use of deprecated variant `clap::AppSettings::DontCollapseArgsInUsage`: Replaced with `Command::dont_collapse_args_in_usage` and `Command::is_dont_collapse_args_in_usage_set`
warning: use of deprecated variant `clap::AppSettings::SubcommandRequired`: Replaced with `Command::subcommand_required` and `Command::is_subcommand_required_set`
```

### Expected Behaviour

CHANGELOG should contain something about these deprecations but AFAICT, doesn't.

### Additional Context

The above may be non-exhaustive. This is the result of existing code that builds with no warning with clap 3.0 having been updated to 3.1. There may have been more deprecations that aren't documented in the changelog.

### Debug Output

_No response_

---

_Label `C-bug` added by @glandium on 2022-02-22 02:14_

---

_Comment by @epage on 2022-02-22 03:21_

This was covered by a catch-all item

> (builder) clap::AppSettings are nearly all deprecated and replaced with builder methods and getters (#2717)

That is a lot to individually specify and it didn't seem like much value since the compiler will tell people about these.

---

_Comment by @lmburns on 2022-02-23 16:06_

How are `AppSettings` and `ArgSettings` supposed to be used when they are derived? I haven't seen a way to use them except for a couple like `AppSettings::DeriveDisplayOrder` which aren't deprecated.

For example, how would I translate this to something that doesn't use deprecated functions? Are they not allowed unless the builder method is used instead?

```rust
#[derive(Parser, Default, Clone, Debug, PartialEq)]
#[clap(
    version = crate_version!(),
    author = <String as AsRef<str>>::as_ref(&APP_AUTHORS),
    about = <String as AsRef<str>>::as_ref(&APP_ABOUT),
    after_help =  <String as AsRef<str>>::as_ref(&AFTER_HELP),
    override_usage =  <String as AsRef<str>>::as_ref(&OVERRIDE_HELP),
    max_term_width(100),
    color(clap::ColorChoice::Auto),
    global_setting(AppSettings::DeriveDisplayOrder), 
    global_setting(AppSettings::InferSubcommands),
    global_setting(AppSettings::InferLongArgs),
    global_setting(AppSettings::DisableHelpSubcommand),
    global_setting(AppSettings::DontCollapseArgsInUsage),
    global_setting(AppSettings::UseLongFormatForHelpSubcommand), 
)]
pub(crate) struct Opts {}
```

---

_Comment by @epage on 2022-02-23 16:12_

Note that the following are builder methods:
- version
- author
- about
- after_help
- override_usage
- max_term_width
- color
- global_setting

So when it suggests to replace `AppSettings::InferLongArgs` with `Command::infer_long_args`, you would replace
```
    global_setting(AppSettings::InferLongArgs),
```
with
```
    infer_long_args = true
```

---

_Comment by @lmburns on 2022-02-23 16:15_

Ah, I see. I didn't realize that all the builder commands were translated like that. Thank you for the clarification!

---

_Closed by @epage on 2022-02-28 17:48_

---

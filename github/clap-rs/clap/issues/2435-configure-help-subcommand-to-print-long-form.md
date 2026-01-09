---
number: 2435
title: Configure help subcommand to print long form
type: issue
state: closed
author: pwinckles
labels:
  - A-help
  - E-help-wanted
  - ":money_with_wings: $5"
assignees: []
created_at: 2021-04-06T12:54:41Z
updated_at: 2021-04-12T23:12:52Z
url: https://github.com/clap-rs/clap/issues/2435
synced_at: 2026-01-07T13:12:19-06:00
---

# Configure help subcommand to print long form

---

_Issue opened by @pwinckles on 2021-04-06 12:54_

### Please complete the following tasks

I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
I have searched the existing issues

### Describe your use case

The `help` subcommand prints the short help form, and I would like to configure it to print the long form instead.

The long form is more inline with my expectations of what the `help` subcommand should return. For example, compare the output of `git help init`, `git init -h`, and `git init --help`. At least on my system, the subcommand produces the long form; not the short form.

### Describe the solution you'd like

Ideally this would be configurable via [AppSettings](https://docs.rs/clap/2.33.3/clap/enum.AppSettings.html).

### Alternatives, if applicable

_No response_

### Additional Context

_No response_



---

_Label `T: new feature` added by @pwinckles on 2021-04-06 12:54_

---

_Added to milestone `3.1` by @pksunkara on 2021-04-06 17:24_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2021-04-06 17:24_

---

_Label `C: help message` added by @pksunkara on 2021-04-06 17:24_

---

_Label `D: easy` added by @pksunkara on 2021-04-06 17:24_

---

_Label `help wanted` added by @pksunkara on 2021-04-06 17:24_

---

_Renamed from "Configure help subsommand to print long form" to "Configure help subcommand to print long form" by @pwinckles on 2021-04-06 22:01_

---

_Comment by @reaganmcf on 2021-04-09 22:09_

I would like to work on this 😄 

---

_Comment by @reaganmcf on 2021-04-10 02:23_

I have made a little bit of progress on this (added the setting and started integration tests). However, I am wondering how I would go about writing integration tests for this. Reading over the code base, I found `utils::compare_output()` but that seems to be for errors. 

(I am going to reformat and run clippy before I send the PR, so ignore that for now)

Here is an example of one of my tests, what is the best way to do this?
```rust
#[test]
fn subcommand_help_shows_long_form_with_setting_using_before_help() {
    let m = App::new("myprog")
        .setting(AppSettings::SubcommandHelpShowsLongForm)
        .subcommand(App::new("test")
            .before_help("short form help message")
            .before_long_help("long form help message"));
    assert!(utils::compare_output(
        m,
        "help test",
        SUBCOMMAND_HELP_LONG_FORM_USING_BEFORE_HELP,
        false)
    );
}
```

---

_Comment by @ldm0 on 2021-04-10 05:02_

@reaganmcf `compare_output()` is not only for errors. It can be used for both stdout and stderr. There is nothing wrong with you test.

---

_Comment by @reaganmcf on 2021-04-10 16:08_

@ldm0 Currently the test above is pacnicking. Here is is the output:
```rust
---- subcommand_help_shows_long_form_with_setting_using_before_help stdout ----
thread 'subcommand_help_shows_long_form_with_setting_using_before_help' panicked at 
'called `Result::unwrap_err()` on an `Ok`  value: ArgMatches { args: {}, subcommand: Some(SubCommand { id: test, name: "test", matches: ArgMatches { args: {}, subcommand: None } }) }', 
tests/utils.rs:36:19
```

---

_Comment by @ldm0 on 2021-04-10 16:17_

@reaganmcf Hmm, check this?:
```rust
    assert!(utils::compare_output(
        m,
        "myprog help test",
        SUBCOMMAND_HELP_LONG_FORM_USING_BEFORE_HELP,
        false)
    );
```

---

_Referenced in [clap-rs/clap#2440](../../clap-rs/clap/pulls/2440.md) on 2021-04-10 21:36_

---

_Closed by @pksunkara on 2021-04-12 23:12_

---

_Referenced in [clap-rs/clap#3440](../../clap-rs/clap/issues/3440.md) on 2022-02-11 18:41_

---

_Referenced in [clap-rs/clap#3451](../../clap-rs/clap/pulls/3451.md) on 2022-02-11 18:51_

---

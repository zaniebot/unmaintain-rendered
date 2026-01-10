---
number: 2873
title: "clap_derive users can't set both a default `help_heading` and an arg-specific `help_heading`"
type: issue
state: closed
author: epage
labels:
  - C-bug
  - A-derive
assignees: []
created_at: 2021-10-14T17:35:27Z
updated_at: 2021-10-15T08:42:55Z
url: https://github.com/clap-rs/clap/issues/2873
synced_at: 2026-01-10T01:27:28Z
---

# clap_derive users can't set both a default `help_heading` and an arg-specific `help_heading`

---

_Issue opened by @epage on 2021-10-14 17:35_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.55.0 (c8dfcfe04 2021-09-06)

### Clap Version

v3.0.0-beta.4

### Minimal reproducible code

Note: this assumes #2785 is fixed

```rust
use clap::{IntoApp, Parser};

#[test]
fn arg_help_heading_applied() {
    #[derive(Debug, Clone, Parser)]
    #[clap(help_heading = "DEFAULT")]
    struct CliOptions {
        #[clap(long)]
        #[clap(help_heading = Some("HEADING A"))]
        should_be_in_section_a: Option<u32>,

        #[clap(long)]
        should_be_in_default_section: Option<u32>,
    }

    let app = CliOptions::into_app();

    let should_be_in_section_a = app
        .get_arguments()
        .find(|a| a.get_name() == "should-be-in-section-a")
        .unwrap();
    assert_eq!(should_be_in_section_a.get_help_heading(), Some("HEADING A"));

    let should_be_in_default_section = app
        .get_arguments()
        .find(|a| a.get_name() == "DEFAULT")
        .unwrap();
    assert_eq!(should_be_in_default_section.get_help_heading(), Some("DEFAULT"));
}
```

### Steps to reproduce the bug with the above code

Run the test

### Actual Behaviour

`should_be_in_section_a` is in section `DEFAULT`

### Expected Behaviour

`should_be_in_section_a` is in section `HEADING A`

### Additional Context

PR #1211 originally added `help_heading` with the current priority
(`App::help_heading` wins).

In #1958, the PR author had proposed to change this

> Note that I made the existing arg's heading a priority over the one in App::help_heading

This was reverted on reviewer request because

> Thanks for the priority explanation. Yes, please make the app's help
> heading a priority. I can see how it would be useful when people might
> want to reuse args.

Re-using args is an important use case but this makes life difficult
for anyone using `help_heading` with `clap_derive` because the user
can only call `App::help_heading` once per struct.  Derive users can't get
per-field granularity with `App::help_heading` like the builder API.

As a bonus, I think this will be the least surprising to users.  Users
generally expect the more generic specification (App) to be overridden by the
more specific specification (Arg).  Honestly, I had thought this Issue's Expected Behavior is
how `help_heading` worked  until I dug into the code with #2872.

In the argument re-use cases, a workaround is for the reusable arguments
to **not** set their help_heading and require the caller to set it as
needed

Note: I have a fix ready, just blocked on #2872.

### Debug Output

_No response_

---

_Label `T: bug` added by @epage on 2021-10-14 17:35_

---

_Label `C: derive macros` added by @epage on 2021-10-14 17:35_

---

_Added to milestone `3.0` by @epage on 2021-10-14 17:35_

---

_Assigned to @epage by @epage on 2021-10-14 17:35_

---

_Referenced in [clap-rs/clap#2878](../../clap-rs/clap/pulls/2878.md) on 2021-10-14 21:47_

---

_Closed by @bors[bot] on 2021-10-15 08:42_

---

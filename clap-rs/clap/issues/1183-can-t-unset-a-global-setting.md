---
number: 1183
title: "Can't unset a global setting"
type: issue
state: closed
author: anna-is-cute
labels: []
assignees: []
created_at: 2018-02-14T17:03:07Z
updated_at: 2018-08-02T03:30:18Z
url: https://github.com/clap-rs/clap/issues/1183
synced_at: 2026-01-10T01:26:45Z
---

# Can't unset a global setting

---

_Issue opened by @anna-is-cute on 2018-02-14 17:03_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Affected Version of clap

2.29.4

### Expected Behavior Summary

Unsetting a setting that has been made global causes it to not affect the arg or subcommand.

### Actual Behavior Summary

The setting still applies.

### Sample Code or Link to Sample Code

```rust
extern crate clap;

use clap::{App, Arg, SubCommand, AppSettings};

fn main() {
  let args = App::new("app")
    .global_setting(AppSettings::ArgRequiredElseHelp)
    .arg(Arg::with_name("arg1")
      .takes_value(true)
      .multiple(false)
      .required(false))
    .subcommand(SubCommand::with_name("sub")
      .unset_setting(AppSettings::ArgRequiredElseHelp)
      .arg(Arg::with_name("arg2")
        .takes_value(true)
        .multiple(false)
        .required(false)))
    .get_matches();
  println!("{:#?}", args);
}
```

In the subcommand, the only arg is optional, so I don't want `ArgRequiredElseHelp`, but the vast majority of other subcommands (in my use case) all benefit from `ArgRequiredElseHelp`.

I can see this is because it's unset in `settings` but not `g_settings`. Should `unset_setting` do both or should `unset_global_setting` be added? I'd be happy to PR if I knew which direction to go in.

---

_Comment by @kbknapp on 2018-02-14 18:56_

You're spot on, and actually in v3 I side step this slightly by using `(un)set` and `(un)set_global` and actually differentiating the two. For v2 I think it's going to have to stay as is, because I fear changing it would be breaking change (i.e. if there are people relying the current behaviour).

I'm going to close this, however when v3 is released if this issue pops up please feel free to re-address it.

---

_Closed by @kbknapp on 2018-02-14 18:56_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2018-02-15 14:38_

---

_Referenced in [sigp/lighthouse#2798](../../sigp/lighthouse/pulls/2798.md) on 2021-11-15 14:36_

---

---
number: 706
title: "AppSettings::DeriveDisplayOrder doesn't propagate to sub commands correctly"
type: issue
state: closed
author: Nemo157
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2016-10-25T20:11:45Z
updated_at: 2018-08-02T03:29:55Z
url: https://github.com/clap-rs/clap/issues/706
synced_at: 2026-01-07T13:12:19-06:00
---

# AppSettings::DeriveDisplayOrder doesn't propagate to sub commands correctly

---

_Issue opened by @Nemo157 on 2016-10-25 20:11_

Adding `AppSettings::DeriveDisplayOrder` to the global settings of an `App` doesn't propagate down to the apps sub commands.

 The following example defines a subcommand with two args in reverse alphabetical order, running `--help` on the subcommand prints the arguments in alphabetical order with the `--help` and `--version` arguments also interspersed.

Moving the `.global_setting(AppSettings::DeriveDisplayOrder)` line down to the subcommand itself results in the correct ordering of the two arguments, so the setting itself is working but for some reason not propagating down correctly. 

---

Example code

``` rust
extern crate clap;

use clap::{App, Arg, SubCommand, AppSettings};

fn main() {
    App::new("MyApp")
        .global_setting(AppSettings::DeriveDisplayOrder)
        .subcommands(vec![
            SubCommand::with_name("test")
                .args(&[
                    Arg::with_name("second")
                        .long("second")
                        .help("second should come first"),
                    Arg::with_name("first")
                        .long("first")
                        .help("first should come second"),
                ])
        ])
        .get_matches();
}
```

---

Output (saved the above example into `examples/derive-display-order-bug.rs` and run via `cargo run --example derive-display-order-bug -- test --help`)

```
derive-display-order-bug-test

USAGE:
    derive-display-order-bug test [FLAGS]

FLAGS:
        --first      first should come second
    -h, --help       Prints help information
        --second     second should come first
    -V, --version    Prints version information
```


---

_Comment by @Nemo157 on 2016-10-25 20:20_

I had a quick look through the source. Seems `DeriveDisplayOrder` is applied while adding the `Arg` to the `Parser` for the `SubCommand`, which happens before even adding the sub command to the top level app and well before the global settings are propagated down just before actually parsing the incoming arguments. It appears `UnifiedHelpMessage` would likely exhibit the same issue.


---

_Comment by @Nemo157 on 2016-10-25 20:51_

Quick WIP commit of a working solution (at least for this example): https://github.com/Nemo157/clap-rs/tree/derive_order_at_print_time_2

Not sure if this will handle the interaction with `UnifiedHelpMessage` or not, I think it's pretty close to a workable solution though.

EDIT: Just realised this will break explicitly using the `display_order` method on args :(


---

_Comment by @kbknapp on 2016-10-26 01:33_

Thanks for such a detailed write up!

I see a possibly simple way to go about fixing this. Since you've already taken the time to check through the code and get a potential fix if you'd like, I'll give my suggestion and answer any questions you've got but let you take the crack at it?

Add an `Arg.user_disp_ord` (default of `999`) (and to subsequent `*Builder` structs) which gets set if the user passes in a custom display order. And then otherwise _always_ set the `disp_ord` number as if `DeriveDisplayOrder` was set.

Then upon propagating the global settings, if `DeriveDisplayOrder` _was_ set, do nothing unless `user_disp_ord` was anything other than `999` in which case set `disp_ord` to `user_disp_ord`. If it _wasn't_ set, set `disp_ord` to the `user_disp_ord` no matter what.

This is just off the top of my head though :stuck_out_tongue_winking_eye: 

Also, it sounds like I need to add a test to guard against this issue in the future!


---

_Label `T: bug` added by @kbknapp on 2016-10-26 01:33_

---

_Label `P2: need to have` added by @kbknapp on 2016-10-26 01:33_

---

_Label `C: args` added by @kbknapp on 2016-10-26 01:33_

---

_Label `C: help message` added by @kbknapp on 2016-10-26 01:33_

---

_Label `C: subcommands` added by @kbknapp on 2016-10-26 01:33_

---

_Label `D: easy` added by @kbknapp on 2016-10-26 01:33_

---

_Label `W: 2.x` added by @kbknapp on 2016-10-26 01:33_

---

_Label `C: settings` added by @kbknapp on 2016-10-26 01:33_

---

_Added to milestone `2.16.3` by @kbknapp on 2016-10-26 01:34_

---

_Comment by @kbknapp on 2016-10-26 01:34_

That solution would also allow the help generation code to remain unchanged.

I'm also open to other solutions, which I forgot to mention.


---

_Comment by @Nemo157 on 2016-10-26 07:16_

I can see how something like that could work, I should be able to throw something together within the next day.


---

_Comment by @Nemo157 on 2016-10-26 16:51_

New WIP commit: https://github.com/Nemo157/clap-rs/commit/2b78c9868acb0063f951311cdea40fbdfe40c501

Based on your suggestion but slightly different to also handle propagating `UnifiedHelpMessage` correctly. Keeps `disp_ord` on the `Arg` purely for the users setting and copies it directly to the other option/flag builders. Adds a new `unified_ord` value to the option/flag builders to keep track of the overall order between the two lists in-case it's needed. After propagation checks whether the display order needs to be derived then uses either the `unified_ord` or just the index in the list to update the `disp_ord` if the user hasn't customised it.


---

_Comment by @Nemo157 on 2016-10-26 16:53_

I'll write a couple of quick tests then make a PR for this.


---

_Comment by @kbknapp on 2016-10-27 02:09_

Excellent! If it passes the prior the tests and some new one(s) for this case I'm good with it! :+1:


---

_Closed by @homu on 2016-10-27 04:48_

---

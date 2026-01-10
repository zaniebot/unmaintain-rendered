---
number: 1184
title: No way to create a global template
type: issue
state: open
author: anna-is-cute
labels:
  - C-enhancement
  - A-help
  - A-builder
  - S-waiting-on-design
assignees: []
created_at: 2018-02-14T17:46:13Z
updated_at: 2024-06-07T06:53:18Z
url: https://github.com/clap-rs/clap/issues/1184
synced_at: 2026-01-10T01:26:45Z
---

# No way to create a global template

---

_Issue opened by @anna-is-cute on 2018-02-14 17:46_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Affected Version of clap

2.29.4

### Expected Behavior Summary

Being able to call `global_template` and have the template propagate down to subcommands.

### Actual Behavior Summary

For every single subcommand, I have to add a `template` call to get them all to match my custom template.

---

_Comment by @kbknapp on 2018-02-14 18:58_

Totally agree this is a lacking feature, thanks for the suggestion!

In order for me to actually get v3 released I have to put blinders on and only implement bug fixes or super trivial features which are simple to forward port to the v3 branch.

I don't know that this is one such issue. I'll leave it open for v3 as it is something I *do* want to have.

Having said all this, if someone implements this for v2 I'm not against the PR, so long as the understanding is that for v3 I may have to rip out the commit and re-do it depending how deep into the internals it gets to implement this. (for v3 the internals are quite different, so some changes aren't trivial to forward port).

---

_Label `C: help message` added by @kbknapp on 2018-02-14 18:59_

---

_Label `C: subcommands` added by @kbknapp on 2018-02-14 18:59_

---

_Label `D: intermediate` added by @kbknapp on 2018-02-14 18:59_

---

_Label `P3: want to have` added by @kbknapp on 2018-02-14 18:59_

---

_Label `C: settings` added by @kbknapp on 2018-02-14 18:59_

---

_Label `W: 3.x` added by @kbknapp on 2018-02-14 18:59_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2018-02-15 14:38_

---

_Added to milestone `v3-alpha.1` by @kbknapp on 2018-07-22 02:52_

---

_Label `M: mentored` added by @kbknapp on 2018-07-22 02:52_

---

_Label `good first issue` added by @kbknapp on 2018-07-22 02:52_

---

_Referenced in [TeXitoi/structopt#172](../../TeXitoi/structopt/issues/172.md) on 2019-03-19 00:54_

---

_Removed from milestone `v3-alpha.2` by @pksunkara on 2020-02-01 07:45_

---

_Added to milestone `v3.0` by @pksunkara on 2020-02-01 07:45_

---

_Removed from milestone `3.0` by @pksunkara on 2020-10-09 17:23_

---

_Added to milestone `3.1` by @pksunkara on 2020-10-09 17:23_

---

_Label `W: 3.x` removed by @pksunkara on 2020-10-09 17:23_

---

_Label `D: medium` removed by @pksunkara on 2020-10-09 17:24_

---

_Label `Z: mentored` removed by @pksunkara on 2020-10-09 17:24_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2020-10-09 17:24_

---

_Label `D: easy` added by @pksunkara on 2020-10-09 17:24_

---

_Comment by @pksunkara on 2020-10-09 17:33_

To solve this, we would either need to add a `global_help_template` function that would forward the template to children ([example](https://github.com/clap-rs/clap/pull/2143/files#diff-7ae8c41c3ed7d1c77421d580c63754fcR2296-R2301)) or we could add a setting called `PropagateTemplate` which does the same.

---

_Referenced in [epage/clapng#87](../../epage/clapng/issues/87.md) on 2021-12-06 16:40_

---

_Label `C: subcommands` removed by @epage on 2021-12-08 20:15_

---

_Label `C: settings` removed by @epage on 2021-12-08 20:15_

---

_Label `A-builder` added by @epage on 2021-12-08 20:15_

---

_Removed from milestone `3.1` by @epage on 2021-12-08 20:15_

---

_Label `D: easy` removed by @epage on 2021-12-09 18:05_

---

_Label `P3: want to have` removed by @epage on 2021-12-09 18:05_

---

_Label `E-easy` removed by @epage on 2021-12-09 18:05_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-09 18:05_

---

_Comment by @epage on 2021-12-09 18:07_

One idea I have for this (and other issues) is if we implemented a layered config. Some fields would be designated as layered, like this, and others would not, like a subcommands display order.  For layered fields, we propagate them to subcommands if they are set in the parent but not the sucommand, and then continue this recursively.

So build would have code like:
```
if let Some(help_template) = self.help_template.as_deref() {
    sub.help_template.get_or_insert(help_template)
}
```

---

_Comment by @pksunkara on 2021-12-10 02:10_

I think you might need to expand on this. I am quite confused reading your comment.

---

_Comment by @epage on 2021-12-10 18:15_

Yeah, I can see needing to re-phrase since i'm using a term in an adjacent domain for which we have feature requests ;).

What if we modeled clap's builder API after layered config?  What I mean is where you take configuration from different sources and merge them together.

The approach I've taken in several applications that I've been finding works well is that the config data tracks what is [explicitly set, using `Option`](https://github.com/epage/git-stack/blob/main/src/config.rs#L4).  You then build the final config by [merging all the sources](https://github.com/epage/git-stack/blob/main/src/config.rs#L38) by [reading each layer and reading the explicitly set values, giving precedence to the higher layer](https://github.com/epage/git-stack/blob/main/src/config.rs#L299).  All the config fields are private and we expose [getters that contain the default value](https://github.com/epage/git-stack/blob/main/src/config.rs#L319) so the callers don't care about these details.  Granted, when a user does a `--dump-config`, I want to [merge in the defaults](https://github.com/epage/git-stack/blob/main/src/config.rs#L183) so they can see them

So what this would look like in `clap` is an `App` would have all "global" fields be `Option<T>`.  When we build, we merge the parent `App`s global fields into its subcommand with code blocks similar to
```rust
if let Some(help_template) = self.help_template.as_deref() {
    subcommand.help_template.get_or_insert(help_template)
}
```
This will preserve the subcommands original help template.  If it was never explicitly set by the user, the parent command's help template would be used.

Depending on performance, we might also apply the default layer so that the accessors don't have to re-compute it on each call.

`Arg::display_order` is part way there at this point
- https://github.com/clap-rs/clap/blob/master/src/build/arg/mod.rs#L84
- https://github.com/clap-rs/clap/blob/master/src/build/arg/mod.rs#L4869
- https://github.com/clap-rs/clap/blob/master/src/build/app/mod.rs#L2877

---

_Referenced in [clap-rs/clap#2513](../../clap-rs/clap/issues/2513.md) on 2021-12-13 16:46_

---

_Label `C-enhancement` added by @epage on 2021-12-13 22:44_

---

_Referenced in [clap-rs/clap#2717](../../clap-rs/clap/issues/2717.md) on 2022-01-10 15:24_

---

_Label `:money_with_wings: $5` removed by @pksunkara on 2024-06-07 06:53_

---

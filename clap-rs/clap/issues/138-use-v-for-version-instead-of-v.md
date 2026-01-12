```yaml
number: 138
title: use -V for version instead of -v
type: issue
state: closed
author: kbknapp
labels:
  - M-breaking-change
  - S-waiting-on-decision
assignees: []
created_at: 2015-06-07T19:23:52Z
updated_at: 2024-01-31T02:10:24Z
url: https://github.com/clap-rs/clap/issues/138
synced_at: 2026-01-12T16:14:08Z
```

# use -V for version instead of -v

---

_@kbknapp_

In order to follow typical conventions where lowercase `-v` is for `verbose` and many programs use the capital `-V` for version (to include `rustc`). Currently, if one wants to use typical `-v` for verbose, that leaves _only_ the long for `version` with no option to "add" a short without manually implementing it (which isn't hard...but still).

There are two options for this:
1. Simply change default short of version to `-V` which is a "breaking" change (at least for downstream documentation).
2. Use short `-v` for version unless manually overridden, then use short `-V` if available. 

I like option 2 because it's a non-breaking change, the downside is it could be confusing if a program suddenly changes from `-v` to `-V` for version seemingly automagically. Or when subcommands have one `-v` but parents or other subcommands have `-V`....so maybe I actually prefer option 1.

TBD


---

_Label `breaking change` added by @kbknapp on 2015-06-07 19:30_

---

_Label `maybe` added by @kbknapp on 2015-06-07 19:30_

---

_Assigned to @kbknapp by @kbknapp on 2015-06-07 19:30_

---

_Added to milestone `1.0 Release` by @kbknapp on 2015-06-07 19:30_

---

_Closed by @kbknapp on 2015-06-17 00:51_

---

_Comment by @kbknapp on 2015-06-17 00:52_

`-V` is now the default. In order to stay more in line with other standard command line interfaces.

I have added `App::version_short()` and `App::help_short()` which allow you to override this default (and set it to anything you like).


---

_Referenced in [metamath/metamath-knife#102](../../metamath/metamath-knife/pulls/102.md) on 2023-02-20 17:24_

---

_Referenced in [deislabs/spiderlightning#380](../../deislabs/spiderlightning/issues/380.md) on 2023-04-12 16:52_

---

_Comment by @JimLynchCodes on 2023-05-21 05:26_

@kbknapp  honestly, I don't like this change.

Many popular cli tools (for example, npm) use the lower case -v for the version short flag.

The `App::version_short()` is cool, but I'm still not sure how to actually use it with the Clap derive syntax... also, what if you want to allow both -v and -V for short version flag?



---

_Comment by @epage on 2023-05-22 18:22_

imo `-v` is more generally recognized as `--verbose` and we do allow customizing it, so not too concerned about some tools using `-v` for `--version`.

`App::version_short` was more of a v2 way of customizing it.

The way to do this in clap today is something like:
```rust
#[derive(Parser)]
#[command(disable_version_flag = true)]
struct {
    /// Print version
    #[arg(short = 'v', short_alias = 'V', long, action = clap::builder::ArgAction::Version)]
    version: (),
}
```

---

_Comment by @fawdlstty on 2024-01-31 01:40_

Can we change the version retrieval method to lowercase? Verbose is not something everyone needs, but uppercase input is difficult, using lowercase to obtain the version is more reasonable

---

_Comment by @epage on 2024-01-31 02:10_

At this point, someone would need to put forward a strong case for changing the default.  As shown above, people are free to change this for their application as they wish.

---

_Referenced in [russellbanks/Komac#409](../../russellbanks/Komac/issues/409.md) on 2024-01-31 09:30_

---

_Referenced in [UCSD-E4E/e4e-data-management-rust#15](../../UCSD-E4E/e4e-data-management-rust/pulls/15.md) on 2024-02-14 03:55_

---

_Referenced in [theoforger/mastermind#2](../../theoforger/mastermind/issues/2.md) on 2024-09-13 22:57_

---

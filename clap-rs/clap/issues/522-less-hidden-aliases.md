```yaml
number: 522
title: Less hidden aliases
type: issue
state: closed
author: joshtriplett
labels:
  - C-enhancement
assignees: []
created_at: 2016-06-06T02:23:26Z
updated_at: 2018-08-02T03:29:50Z
url: https://github.com/clap-rs/clap/issues/522
synced_at: 2026-01-12T16:14:09Z
```

# Less hidden aliases

---

_@joshtriplett_

`.alias` creates a hidden subcommand.  For user convenience, I'd like to create an un-hidden subcommand, shown in help (with a help string like "alias for othercmd").


---

_Comment by @kbknapp on 2016-06-06 23:07_

Interesting, I hadn't thought about that, but I like it. It'd be an easy addition. What would you call it? `synonym`, `visible_alias`? I'm open to ideas.


---

_Label `T: enhancement` added by @kbknapp on 2016-06-06 23:07_

---

_Label `T: new feature` added by @kbknapp on 2016-06-06 23:07_

---

_Label `P4: nice to have` added by @kbknapp on 2016-06-06 23:07_

---

_Label `C: subcommands` added by @kbknapp on 2016-06-06 23:07_

---

_Label `D: easy` added by @kbknapp on 2016-06-06 23:07_

---

_Label `W: 2.x` added by @kbknapp on 2016-06-06 23:07_

---

_Comment by @joshtriplett on 2016-06-07 00:06_

I don't think `synonym` accurately conveys the distinction; if I saw functions for both `alias` and `synonym`, I couldn't guess at a glance about the distinction between them.

I'd suggest something explicit, like `visible_alias`.


---

_Added to milestone `2.6.0` by @kbknapp on 2016-06-08 02:04_

---

_Comment by @kbknapp on 2016-06-10 01:58_

This is implemented with #525 

From the commit message:

This commit adds support for visible aliases, which function exactly like aliases except that they
also appear in the help message, using the help string of the aliased subcommand.

i.e.

``` rust
App::new("myprog")
    .subcommand(SubCommand::with_name("test")
        .about("does testy things")
        .alias("invisible")
        .visible_alias("visible"));
```

When run with `myprog --help`, the output is:

```
myprog

USAGE:
    myprog [FLAGS] [SUBCOMMAND]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

SUBCOMMANDS:
    help            Prints this message or the help of the given subcommand(s)
    test|visible    does testy things
```


---

_Comment by @kbknapp on 2016-06-10 01:59_

Once #451 is implemented I'll update to 2.6.0 and upload to crates.io. So I'm hoping by tomorrow :wink:


---

_Closed by @homu on 2016-06-10 10:38_

---

_Referenced in [clap-rs/clap#526](../../clap-rs/clap/issues/526.md) on 2016-06-10 16:21_

---

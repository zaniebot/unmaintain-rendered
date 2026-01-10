---
number: 519
title: ColoredHelp is overridden by subcommands
type: issue
state: closed
author: clux
labels:
  - A-help
assignees: []
created_at: 2016-06-03T13:21:45Z
updated_at: 2018-08-02T03:29:50Z
url: https://github.com/clap-rs/clap/issues/519
synced_at: 2026-01-10T01:26:30Z
---

# ColoredHelp is overridden by subcommands

---

_Issue opened by @clux on 2016-06-03 13:21_

Currently `.setting(AppSettings::ColoredHelp)` has to be added to every subcommand even though it is set on the "global scope"

``` rs
App::new("foo")
        .setting(AppSettings::ColoredHelp)
        .subcommand(SubCommand::with_name("bar"))
```

``` sh
foo help # gives colored output
foo help bar # uncolored
```

It probably makes sense to inherit this value from parent, rather than adding it everywhere, right?


---

_Comment by @kbknapp on 2016-06-03 13:40_

Thanks for filing this! Great point, I hadn't considered that. There are an increasing number of settings that one would intuitively think would be propagated down through child commands, but aren't.

I'm thinking of adding an `App::global_setting(s)` to do just that. 


---

_Label `T: new feature` added by @kbknapp on 2016-06-03 13:40_

---

_Label `C: help message` added by @kbknapp on 2016-06-03 13:40_

---

_Label `C: subcommands` added by @kbknapp on 2016-06-03 13:40_

---

_Label `D: intermediate` added by @kbknapp on 2016-06-03 13:40_

---

_Label `P3: want to have` added by @kbknapp on 2016-06-03 13:40_

---

_Label `W: 2.x` added by @kbknapp on 2016-06-03 13:40_

---

_Label `C: settings` added by @kbknapp on 2016-06-03 13:40_

---

_Label `T: new setting` added by @kbknapp on 2016-06-03 13:40_

---

_Comment by @kbknapp on 2016-06-04 15:57_

#520 fixes this


---

_Comment by @clux on 2016-06-04 17:42_

Awesome, thank you!


---

_Closed by @homu on 2016-06-08 01:34_

---

_Added to milestone `2.6.0` by @kbknapp on 2016-06-08 02:06_

---

_Referenced in [clap-rs/clap#643](../../clap-rs/clap/issues/643.md) on 2016-09-01 16:34_

---

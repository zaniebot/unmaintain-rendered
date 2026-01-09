---
number: 590
title: "Add a App::unset_setting or App::unset_settings fn"
type: issue
state: closed
author: freshstrangemusic
labels:
  - C-enhancement
  - A-builder
  - E-medium
assignees: []
created_at: 2016-07-23T07:53:07Z
updated_at: 2018-08-02T03:29:52Z
url: https://github.com/clap-rs/clap/issues/590
synced_at: 2026-01-07T13:12:19-06:00
---

# Add a App::unset_setting or App::unset_settings fn

---

_Issue opened by @freshstrangemusic on 2016-07-23 07:53_

Currently if you would like to disable a setting, you have to use undocumented interfaces to do so. I would prefer to not have the `--help` option and instead only use the `help` subcommand so I currently have to do:

``` rust
let mut app = App::new("program")
    /* ... */;

app.p.unset(AppSettings::NeedsLongHelp);
```

It would be much more ideal to do:

``` rust
let app = App::new("program")
    .unset_setting(AppSettings::NeedsLongHelp)
    /* ... */;
```


---

_Comment by @kbknapp on 2016-07-23 20:00_

Good idea, thanks! :+1: 

I'll add this to the queue, and should a very easy addition if anyone would like to try out this as a first PR.


---

_Label `T: enhancement` added by @kbknapp on 2016-07-23 20:00_

---

_Label `T: new feature` added by @kbknapp on 2016-07-23 20:00_

---

_Label `P4: nice to have` added by @kbknapp on 2016-07-23 20:00_

---

_Label `C: app` added by @kbknapp on 2016-07-23 20:00_

---

_Label `D: easy` added by @kbknapp on 2016-07-23 20:00_

---

_Label `W: 2.x` added by @kbknapp on 2016-07-23 20:00_

---

_Label `M: mentored` added by @kbknapp on 2016-07-23 20:00_

---

_Label `C: settings` added by @kbknapp on 2016-07-23 20:00_

---

_Comment by @freshstrangemusic on 2016-07-24 01:05_

I would be willing to take this on.


---

_Referenced in [clap-rs/clap#598](../../clap-rs/clap/pulls/598.md) on 2016-07-24 02:02_

---

_Closed by @kbknapp on 2016-07-24 04:08_

---

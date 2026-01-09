---
number: 588
title: Strange behavior when using AllowLeadingHyphen
type: issue
state: closed
author: yodalee
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2016-07-22T01:46:03Z
updated_at: 2018-08-02T03:29:51Z
url: https://github.com/clap-rs/clap/issues/588
synced_at: 2026-01-07T13:12:19-06:00
---

# Strange behavior when using AllowLeadingHyphen

---

_Issue opened by @yodalee on 2016-07-22 01:46_

I'm trying to using clap-rs to build the interface of `chmod`. `chmod` accepts arguments like `-rwx` `+0700`, These are not real arguments so we cannot add them to arguments.
I try to setting like this:

```
.setting(AppSettings::AllowLeadingHyphen)
.arg(Arg::with_name("mode"))
.arg(Arg::with_name("debug").long("debug").short("d").help("open debug mode"))
```

with this will give error:

```
$ claptest -rwx -d`
error: Found argument '-d' which wasn't expected, or isn't valid in this context
```

Not sure how to set this correctly.


---

_Comment by @kbknapp on 2016-07-23 20:05_

This is a bug. Let me look into it a little deeper and I'll post back here with some findings. Thanks for filing this!


---

_Label `T: bug` added by @kbknapp on 2016-07-23 20:05_

---

_Label `P2: need to have` added by @kbknapp on 2016-07-23 20:05_

---

_Label `C: args` added by @kbknapp on 2016-07-23 20:05_

---

_Label `C: parsing` added by @kbknapp on 2016-07-23 20:05_

---

_Label `W: 2.x` added by @kbknapp on 2016-07-23 20:05_

---

_Label `C: settings` added by @kbknapp on 2016-07-23 20:05_

---

_Added to milestone `2.12.0` by @kbknapp on 2016-09-12 18:38_

---

_Comment by @kbknapp on 2016-09-13 02:01_

Sorry this took so long to fix, it's fixed in #660 and will be uploaded to crates.io shortly as v2.12.0


---

_Closed by @homu on 2016-09-13 02:45_

---

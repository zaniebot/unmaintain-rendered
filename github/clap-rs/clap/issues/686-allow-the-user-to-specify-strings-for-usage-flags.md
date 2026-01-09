---
number: 686
title: "Allow the user to specify strings for \"USAGE:\", \"FLAGS:\", \"OPTIONS:\", \"ARGS:\"..."
type: issue
state: closed
author: crowdagger
labels:
  - C-enhancement
  - A-help
assignees: []
created_at: 2016-10-11T19:53:35Z
updated_at: 2018-08-02T03:29:54Z
url: https://github.com/clap-rs/clap/issues/686
synced_at: 2026-01-07T13:12:19-06:00
---

# Allow the user to specify strings for "USAGE:", "FLAGS:", "OPTIONS:", "ARGS:"...

---

_Issue opened by @crowdagger on 2016-10-11 19:53_

Currently, the strings used in the help message "USAGE:", "FLAGS:", "OPTIONS:", "ARGS" are hardcoded in `src/app/help.rs`. 

Similarly, in the `create_help_and_version` method of `src/app/parser.rs`, the help messages for `-h` and `-v` are hardcoded to "Prints help information" and "Prints version information", and I don't think (I may be wrong on this?) they can be overriden. 

I think it would be nice to allow the user to set their own strings for those, e.g. to allow translation. 


---

_Comment by @kbknapp on 2016-10-12 01:12_

This can be done via [`App::template`](https://docs.rs/clap/2.14.0/clap/struct.App.html#method.template)s.

Here's a [video about templating](https://www.youtube.com/watch?v=mYFyprvW1r8&t=9m2s) as well.

There's also the [`AppSettings::UnifiedHelpMessage`](https://docs.rs/clap/2.14.0/clap/enum.AppSettings.html#variant.UnifiedHelpMessage) which simply combines `FLAGS` and `OPTIONS` into just `OPTIONS`.

I know that's not exactly what you're asking for, but at this time thats how it can be done. I'm not against giving a way to customize these strings, but am unsure about how to best do this.

As far as overriding the default help or version messages, I'm also open to giving ways to customize those...I just need to finish a few other issues first :stuck_out_tongue_winking_eye: 


---

_Label `T: enhancement` added by @kbknapp on 2016-10-12 01:13_

---

_Label `P4: nice to have` added by @kbknapp on 2016-10-12 01:13_

---

_Label `C: help message` added by @kbknapp on 2016-10-12 01:13_

---

_Label `W: 2.x` added by @kbknapp on 2016-10-12 01:13_

---

_Label `T: RFC / question` added by @kbknapp on 2016-10-12 01:13_

---

_Comment by @crowdagger on 2016-10-12 17:56_

Yes, it's possible to use template and overriding default implementations for help/version, just thought it could be an enhancement :) (Didn't know about the videos, that's really nice)


---

_Comment by @kbknapp on 2017-01-03 21:26_

@lise-henry is something like #805 what you're envisioning?

---

_Referenced in [clap-rs/clap#805](../../clap-rs/clap/issues/805.md) on 2017-01-03 21:27_

---

_Comment by @kbknapp on 2017-01-30 04:53_

I'm going to close this issue in favor of #805 

---

_Closed by @kbknapp on 2017-01-30 04:53_

---

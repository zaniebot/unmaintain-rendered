---
number: 644
title: Support a public interface to make your own completion code
type: issue
state: closed
author: kdar
labels:
  - C-enhancement
  - E-hard
  - A-completion
assignees: []
created_at: 2016-09-01T17:46:54Z
updated_at: 2018-08-02T03:29:53Z
url: https://github.com/clap-rs/clap/issues/644
synced_at: 2026-01-10T01:26:33Z
---

# Support a public interface to make your own completion code

---

_Issue opened by @kdar on 2016-09-01 17:46_

My use case here is I'm making a debugger for an emulator, and I use clap as my command handler and [linefeed](https://github.com/murarth/linefeed) as my readliner. Linefeed has a way to do tab completion in it, so it would be nice if I could get at all the subcommand/options/whatever from clap.


---

_Comment by @kbknapp on 2016-09-05 17:55_

I'm not against this, it just may be a while before I can get to something like this. So I'll keep it in the issue tracker for implementation, but need to knock out some other items first.

In the meantime, if you look at [src/completions.rs](https://github.com/kbknapp/clap-rs/blob/26d5207728115174f39fc995707c70d5591b1929/src/completions.rs) there's not inherently private about how it's done, but exposing those methods at this time would be a messy API. Namely the `get_*_for_path()` methods are how it's finding options, subcommands, etc. So one could almost copy/paste that into user code and get same affect until I'm able to get a good API for this.


---

_Label `T: enhancement` added by @kbknapp on 2016-09-05 17:56_

---

_Label `P4: nice to have` added by @kbknapp on 2016-09-05 17:56_

---

_Label `D: hard` added by @kbknapp on 2016-09-05 17:56_

---

_Label `W: 2.x` added by @kbknapp on 2016-09-05 17:56_

---

_Label `C: completion gen` added by @kbknapp on 2016-09-05 17:56_

---

_Comment by @kbknapp on 2016-10-10 19:13_

@kdar see also #568 as I plan on working on this in the near term


---

_Comment by @kbknapp on 2017-01-24 20:26_

I'm closing this as it's covered by #568

---

_Closed by @kbknapp on 2017-01-24 20:26_

---

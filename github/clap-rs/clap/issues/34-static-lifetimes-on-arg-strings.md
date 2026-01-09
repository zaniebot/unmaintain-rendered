---
number: 34
title: Static lifetimes on Arg strings
type: issue
state: closed
author: jhelwig
labels: []
assignees: []
created_at: 2015-03-26T22:56:02Z
updated_at: 2018-08-02T03:29:37Z
url: https://github.com/clap-rs/clap/issues/34
synced_at: 2026-01-07T13:12:19-06:00
---

# Static lifetimes on Arg strings

---

_Issue opened by @jhelwig on 2015-03-26 22:56_

So, I went to do a bit more dynamic generation (this time with the help text of one of my app's arguments), and ran into the same static lifetime issue as #28.

I can provide a more concrete example if you'd like, but I wanted to file this before I forgot.


---

_Comment by @kbknapp on 2015-03-26 23:00_

No need for the example, I know what you mean :) I actually was thinking about this when implementing #28 I'll see what I can figure out, and post back with some answers!


---

_Closed by @kbknapp on 2015-03-27 01:17_

---

_Comment by @kbknapp on 2015-03-27 01:18_

Let me know if that works for you. If not, I may need a more detailed diagram of what you're trying to do ;) Master in the repo or 0.4.17 on crates.io is what you're after btw.


---

_Comment by @jhelwig on 2015-03-27 01:41_

:+1: It works.  Thank you!


---

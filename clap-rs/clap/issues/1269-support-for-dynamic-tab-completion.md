---
number: 1269
title: Support for dynamic tab completion
type: issue
state: closed
author: softprops
labels: []
assignees: []
created_at: 2018-05-04T13:28:11Z
updated_at: 2018-08-02T03:30:23Z
url: https://github.com/clap-rs/clap/issues/1269
synced_at: 2026-01-10T01:26:47Z
---

# Support for dynamic tab completion

---

_Issue opened by @softprops on 2018-05-04 13:28_

Looking for something akin to the dynamic options feature of the golang cli package

https://github.com/alecthomas/kingpin/blob/master/README.md#additional-api

More specifically I would like an alternative to the existing clap API that let's you configure a static array of arg option values that would let me instead provide a fn type that yields a dynamic list.

Bonus points if this could factor into claps current support for tab completion

I would be interested in working on an implementation with some coaching on what the key areas in clap would need to change.


---

_Comment by @kbknapp on 2018-06-05 01:22_

This is covered by #1232 and #568 

It's something I very much want, I just have some other priorities to get 3.x-beta out the door first :wink: I'm going to close this in favor of the other two issues. Feel free to discuss there and we can work on implementation!

---

_Closed by @kbknapp on 2018-06-05 01:22_

---

_Comment by @softprops on 2018-06-05 02:56_

No prob. These are exactly what  I'm looking for!

---

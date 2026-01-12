```yaml
number: 602
title: subcommand arg groups
type: issue
state: closed
author: laishulu
labels: []
assignees: []
created_at: 2016-07-25T06:13:37Z
updated_at: 2018-08-02T03:29:52Z
url: https://github.com/clap-rs/clap/issues/602
synced_at: 2026-01-12T16:14:09Z
```

# subcommand arg groups

---

_@laishulu_

Are subcommand arg groups supported?
If supported, how to config in yaml file?


---

_Comment by @laishulu on 2016-07-25 11:10_

found how to do that by myself.


---

_Closed by @laishulu on 2016-07-25 11:10_

---

_Comment by @jpeddicord on 2017-10-26 04:13_

https://xkcd.com/979/

To everyone else: the answer appears to be yes, and if you want to specify them via yaml, just indent the `groups` keyword to a child of your subcommand key (same level as `args`, `about`, etc).

---

_Comment by @kbknapp on 2017-10-29 18:05_

Thanks for taking the time to actually say the answer @jpeddicord 

I'm also OK with people opening issues to ask the question again, or join the Gitter chat to ask 😄 

---

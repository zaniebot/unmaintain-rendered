---
number: 523
title: Loosen Clippy Version Restriction
type: issue
state: closed
author: indiv0
labels: []
assignees: []
created_at: 2016-06-06T17:37:48Z
updated_at: 2018-08-02T03:29:50Z
url: https://github.com/clap-rs/clap/issues/523
synced_at: 2026-01-10T01:26:30Z
---

# Loosen Clippy Version Restriction

---

_Issue opened by @indiv0 on 2016-06-06 17:37_

Would it be possible to loosen the Clippy version restriction from `=0.0.64` to `^0.0.64`?


---

_Referenced in [indiv0/ferru#7](../../indiv0/ferru/issues/7.md) on 2016-06-06 19:20_

---

_Comment by @kbknapp on 2016-06-06 23:11_

It used to be that way, but then when new clippy version would come out, or clippy fell out of date with a nightly compiler it would stop TravisCI builds. 

I'll update it to `^0.0.64` in the next round of PRs.


---

_Label `T: RFC / question` added by @kbknapp on 2016-06-06 23:12_

---

_Label `W: 2.x` added by @kbknapp on 2016-06-06 23:12_

---

_Comment by @indiv0 on 2016-06-07 01:30_

Couldn't you also set it to 0.0 to keep it up to date with the latest version?


---

_Comment by @kbknapp on 2016-06-07 01:52_

`0.0` is the same as `^0.0` IIRC, the default is the caret. I normally use the tilde `~` though on pre-1.0 crates to reduce breakage. The only thing I wouldn't want is breakage at the 0.1 mark, so I'll probably use `~` just to be safe cause it only takes a few minutes to update a clippy.


---

_Comment by @indiv0 on 2016-06-07 02:19_

Yeah, `0.0` is the same as `^0.0`. However what I meant was due to the way that semver handles pre-`0.1.0` versions, setting clippy to `^0.0` will make it automatically track the clippy patch version until `0.1`.


---

_Added to milestone `2.6.0` by @kbknapp on 2016-06-08 02:04_

---

_Closed by @homu on 2016-06-08 10:58_

---

_Comment by @indiv0 on 2016-06-08 18:52_

@kbknapp thanks!


---

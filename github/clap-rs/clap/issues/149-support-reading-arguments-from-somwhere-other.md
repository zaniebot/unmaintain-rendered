---
number: 149
title: "Support reading arguments from somwhere other than env::args()"
type: issue
state: closed
author: cristicbz
labels:
  - C-enhancement
assignees: []
created_at: 2015-07-07T21:02:51Z
updated_at: 2018-08-02T03:29:40Z
url: https://github.com/clap-rs/clap/issues/149
synced_at: 2026-01-07T13:12:19-06:00
---

# Support reading arguments from somwhere other than env::args()

---

_Issue opened by @cristicbz on 2015-07-07 21:02_

Sorry if this exists already and I couldn't find it; basically I'd like a `get_matches_from(some_str_iter)`. This would making testing easier.

Thanks!


---

_Comment by @kbknapp on 2015-07-07 21:38_

It does exist to an extent, because that's actually how subcommands work...but it's not public :P This should be an easy thing to make public, let me do some tests and if all goes well I'll post a new version (1.0).

Thanks for taking the time to file the issue!


---

_Label `enhancement` added by @kbknapp on 2015-07-07 21:38_

---

_Referenced in [clap-rs/clap#150](../../clap-rs/clap/pulls/150.md) on 2015-07-08 00:06_

---

_Comment by @kbknapp on 2015-07-08 00:08_

Once #150 passes Travis, I'll merge and publish the new 1.0 to crates.io

Thanks again for the good idea! :+1: 


---

_Comment by @kbknapp on 2015-07-08 00:23_

1.0 on crates.io or master here now includes a [App::get_matches_from()](http://kbknapp.github.io/clap-rs/clap/struct.App.html#method.get_matches_from).


---

_Closed by @kbknapp on 2015-07-08 00:23_

---

_Comment by @cristicbz on 2015-07-08 09:06_

Awesome, thanks a lot!


---

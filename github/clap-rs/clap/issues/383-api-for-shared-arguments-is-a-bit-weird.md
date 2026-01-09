---
number: 383
title: API for shared arguments is a bit weird
type: issue
state: closed
author: sgrif
labels: []
assignees: []
created_at: 2016-01-23T19:57:57Z
updated_at: 2018-08-02T03:29:47Z
url: https://github.com/clap-rs/clap/issues/383
synced_at: 2026-01-07T13:12:19-06:00
---

# API for shared arguments is a bit weird

---

_Issue opened by @sgrif on 2016-01-23 19:57_

So `diesel_cli` has a use case where basically all of our commands take the same argument. You can see our exact code here: https://github.com/sgrif/diesel/blob/331f81c353ec0c7f0b6897cb80cb405b2d9120bb/diesel_cli/src/main.rs#L13-L42

All of our commands except one take `--database-url` (and really it's not a big deal if it gets passed to that command as well). Right now, if I put that argument on the `Clap::App` instance, it requires that the command is run as `diesel --database-url=url migration run`, but I'd much rather it be `diesel migration run --database-url=url`, or really just allow it basically anywhere. (As an aside, we're having to wrap the argument in a closure because `Arg` doesn't derive `Clone`, but it probably could)

Does this seem like a reasonable/common enough use case to warrant support within clap itself?


---

_Comment by @kbknapp on 2016-01-25 09:52_

If you add that `Arg` to your top level `App` instance but set the `Arg::global(true)` it _should_ be exactly what you're looking for...i.e. it'll just propagate that particular `Arg` down through all child subcommands....if not it's a bug ;)


---

_Comment by @kbknapp on 2016-01-25 09:54_

@sgrif Also I haven't forgotten about #372 I'm really hoping to have it out really soon with the 2.x base :wink: 


---

_Label `T: RFC / question` added by @kbknapp on 2016-01-25 09:55_

---

_Comment by @kbknapp on 2016-01-25 10:18_

@sgrif Also I forgot to mention that in v2 it supports passing borrowed `Arg` structs, so you could create an `Arg` instance and just pass a borrow of that same instance to all your subcommands which doesn't require `Clone`ing like `global(true)` does in 1.x (`Arg::global` also exists in v2 ~~and removes the `Clone`ing as well, just FYI it's essentially shorthand for passing a borrow to down through each subcommand~~ Edit: `Arg::global` still clones, I was mistaken on how'd I'd implemented that...but passing a borrow does not clone so long as `global(true)` is not set).

Edit: Corrections


---

_Comment by @sgrif on 2016-01-26 19:21_

Yup, I just missed this. Thanks. 


---

_Closed by @sgrif on 2016-01-26 19:21_

---

_Referenced in [rwx-research/abq#67](../../rwx-research/abq/pulls/67.md) on 2023-07-07 13:48_

---

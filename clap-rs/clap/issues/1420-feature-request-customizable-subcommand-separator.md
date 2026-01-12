```yaml
number: 1420
title: "[feature request] customizable subcommand separator besides space"
type: issue
state: open
author: samuela
labels:
  - C-enhancement
  - A-builder
  - A-parsing
  - S-waiting-on-design
assignees: []
created_at: 2019-02-19T19:38:07Z
updated_at: 2021-12-09T19:45:50Z
url: https://github.com/clap-rs/clap/issues/1420
synced_at: 2026-01-12T16:14:10Z
```

# [feature request] customizable subcommand separator besides space

---

_@samuela_

Many CLIs use colons to separate subcommands instead of spaces, eg. `heroku auth:login`, `heroku auth:logout`, etc. It would be nice to have support for this sort of thing in clap as well. It becomes especially important when maintaining backwards compatibility with CLIs written in this format.

---

_Label `C: subcommands` added by @CreepySkeleton on 2020-02-01 15:05_

---

_Label `D: hard` added by @CreepySkeleton on 2020-02-01 15:05_

---

_Label `P4: nice to have` added by @CreepySkeleton on 2020-02-01 15:05_

---

_Label `T: enhancement` added by @CreepySkeleton on 2020-02-01 15:05_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-09 07:49_

---

_Referenced in [epage/clapng#114](../../epage/clapng/issues/114.md) on 2021-12-06 18:34_

---

_Label `C: subcommands` removed by @epage on 2021-12-08 20:08_

---

_Label `A-builder` added by @epage on 2021-12-08 20:08_

---

_Label `A-parsing` added by @epage on 2021-12-08 20:08_

---

_Comment by @epage on 2021-12-08 20:09_

So it sounds like you want to have `heroku auth:login` act like `heroku auth logic` today?  Is there a reason to do this instead of just defining an `auth:login` subcommand?  For help management?

---

_Removed from milestone `3.1` by @epage on 2021-12-08 20:09_

---

_Comment by @samuela on 2021-12-09 00:35_

Yeah, it's not a huge deal writing a CLI from scratch. Just a bit of an issue if you want to rewrite an existing CLI with clap.

---

_Comment by @epage on 2021-12-09 00:45_

I think what I'm missing is what the challenge is in porting an existing CLI to clap.  In what way would a `auth:login` subcommand not work?

---

_Comment by @samuela on 2021-12-09 00:55_

Honestly it's been a while since I was working on this, so I don't recall at this point. Things may have also changed with clap  in the meantime that would obviate the need for this feature request.

---

_Comment by @pksunkara on 2021-12-09 01:24_

I think the intention is to treat `auth` as a command and then it having it's own subcommands (maybe for better organization in the help messages?).

---

_Label `P4: nice to have` removed by @epage on 2021-12-09 19:45_

---

_Label `E-hard` removed by @epage on 2021-12-09 19:45_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-09 19:45_

---

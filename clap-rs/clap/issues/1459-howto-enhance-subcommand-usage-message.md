---
number: 1459
title: Howto enhance subcommand/USAGE message?
type: issue
state: closed
author: spooch
labels: []
assignees: []
created_at: 2019-04-27T16:34:38Z
updated_at: 2020-02-01T14:21:25Z
url: https://github.com/clap-rs/clap/issues/1459
synced_at: 2026-01-10T01:26:54Z
---

# Howto enhance subcommand/USAGE message?

---

_Issue opened by @spooch on 2019-04-27 16:34_

Hi,
Im trying to use clap for a cli-tool like git with two subcommands. The first one is the target, which I want to edit, the second is the action type (like "create" "remove" etc.).

Now on two subcommands the USAGE message always is, although both subcommands are required:
```
my_cli [SUBCOMMAND]
```

but I would like to see more meaningful like:
```
my_cli <TARGET> <ACTION>
```
Furthermore It would be great to rename the Header from "SUBCOMMANDS" to "TARGETS" too, even as in the help of the targets to "ACTION".

Is this possible at the moment?


---

_Comment by @CreepySkeleton on 2020-02-01 14:21_

Duplicate of #1597 

---

_Closed by @CreepySkeleton on 2020-02-01 14:21_

---

_Label `T: duplicate` added by @CreepySkeleton on 2020-02-01 14:21_

---

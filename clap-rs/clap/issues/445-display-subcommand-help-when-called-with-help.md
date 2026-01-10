---
number: 445
title: "Display subcommand help when called with \"help subcommand\""
type: issue
state: closed
author: Fiedzia
labels: []
assignees: []
created_at: 2016-03-10T14:28:22Z
updated_at: 2018-08-02T03:29:48Z
url: https://github.com/clap-rs/clap/issues/445
synced_at: 2026-01-10T01:26:29Z
---

# Display subcommand help when called with "help subcommand"

---

_Issue opened by @Fiedzia on 2016-03-10 14:28_

Having "list" subcommand, I'd like to display help for it when called as "help list"
(./prog help list). Currently it displays help for all available options and subcommands.
There seem to be settings NeedsSubcommandHelp for that, but its not documented, so I am not sure what it is supposed to do.


---

_Comment by @sru on 2016-03-10 16:04_

Duplicate of #416? It's not currently possible.


---

_Comment by @kbknapp on 2016-03-10 16:35_

Yep, I'm gonna close this as a duplicate, but move the comments to that thread for tracking the issue.


---

_Closed by @kbknapp on 2016-03-10 16:35_

---

_Referenced in [clap-rs/clap#416](../../clap-rs/clap/issues/416.md) on 2016-03-10 16:36_

---

_Referenced in [clap-rs/clap#1288](../../clap-rs/clap/issues/1288.md) on 2018-06-07 10:51_

---

---
number: 3478
title: "clap::Command docs recommends deprecated function."
type: issue
state: closed
author: tbethe
labels: []
assignees: []
created_at: 2022-02-16T22:24:17Z
updated_at: 2022-02-16T23:43:52Z
url: https://github.com/clap-rs/clap/issues/3478
synced_at: 2026-01-10T01:27:41Z
---

# clap::Command docs recommends deprecated function.

---

_Issue opened by @tbethe on 2022-02-16 22:24_

Clap version: 3.1.0

The docs of [clap::Command](https://docs.rs/clap/3.1.0/clap/type.Command.html) still tells you to use [CommandFactory::into_app](https://docs.rs/clap/3.1.0/clap/trait.CommandFactory.html#tymethod.into_app) when getting access to Command when deriving using Parser, but this function is deprecated in favour of [clap::CommandFactory::command](https://docs.rs/clap/3.1.0/clap/trait.CommandFactory.html#method.command). The docs of clap::Command should be updated.



---

_Referenced in [clap-rs/clap#3479](../../clap-rs/clap/pulls/3479.md) on 2022-02-16 22:27_

---

_Comment by @epage on 2022-02-16 22:28_

Thanks!

---

_Closed by @epage on 2022-02-16 23:43_

---

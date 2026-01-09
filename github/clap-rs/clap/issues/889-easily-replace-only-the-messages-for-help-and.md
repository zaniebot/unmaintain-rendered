---
number: 889
title: Easily replace only the messages for --help and --version
type: issue
state: closed
author: porglezomp
labels: []
assignees: []
created_at: 2017-03-07T02:12:42Z
updated_at: 2018-08-02T03:30:03Z
url: https://github.com/clap-rs/clap/issues/889
synced_at: 2026-01-07T13:12:19-06:00
---

# Easily replace only the messages for --help and --version

---

_Issue opened by @porglezomp on 2017-03-07 02:12_

I'm trying to help make a project's help messages consistent, and all of the messages are of the format "Start …", "Check …", "Print …", so the "Prints  version information" stands out. I was unable to find an easy way to replace only the help and version messages without re-wiring the commands themselves. Is there any easy solution I missed, or is this something that would have to be added.

---

_Comment by @kbknapp on 2017-03-09 22:38_

There isn't a way to do this currently. You can override the `--version` by supplying a flag with the long of `version`. The only downside to doing this, is you'll need to check if it was used and print the version/exit accordingly. But that's only about 3 lines of code, so in practice it's not too bad.

---

_Label `T: RFC / question` added by @kbknapp on 2017-03-09 22:38_

---

_Comment by @porglezomp on 2017-03-09 22:40_

Would you appreciate a contribution that adds this? It's obviously easily worked around, but seems like it would be nice to have for other people as well.

---

_Comment by @kbknapp on 2017-03-09 22:42_

I would be willing to consider it, sure! I don't have the bandwidth to work on it right now, but would gladly look at a PR adding it.

---

_Comment by @porglezomp on 2017-03-10 05:32_

So I'm working on this, and am wondering what you think the methods should be called. On args it's `help`, but with the help arg that gets awkward. Do you prefer `help_message` and `version_message`, `help_help` and `version_help`,  or something else?

---

_Referenced in [clap-rs/clap#894](../../clap-rs/clap/pulls/894.md) on 2017-03-10 06:10_

---

_Comment by @porglezomp on 2017-03-10 06:22_

Also, where in the yaml should these customizations end up? Based on the `help_short` and `version_short` it looks like they should top level?

---

_Closed by @homu on 2017-03-10 13:22_

---

_Referenced in [clap-rs/clap#2500](../../clap-rs/clap/issues/2500.md) on 2021-05-26 19:25_

---

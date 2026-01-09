---
number: 3418
title: Man page content differs unnecessarily from help ouput
type: issue
state: closed
author: alerque
labels:
  - A-man
assignees: []
created_at: 2022-02-08T15:14:26Z
updated_at: 2022-09-30T18:32:15Z
url: https://github.com/clap-rs/clap/issues/3418
synced_at: 2026-01-07T13:12:19-06:00
---

# Man page content differs unnecessarily from help ouput

---

_Issue opened by @alerque on 2022-02-08 15:14_

As of Clap 3.0.14, the output of clap_mangen is different on quite a few points from the `--help` output that are just confusing and don't appear to be configurable.

- [ ] The subcommand output assumes `command-subcommand` style multi-entry binaries. The help output does not, and the help output seems to be a better match for what Clap actually does right now. I don't see any docs on getting a multi-entry binary system setup to even build such apps, much less generate the related subcommand man pages.

- [ ] Subcommand args are not included. Given the inability to generate subcommand man pages this seems like a must. Issue #1334 is related, but for help pages.

- [ ] In the man page, subcommands are `command <subcommands>`, in the help pages they are `command SUBCOMMANDS`. The latter appears to be configurable, but the former doesn't follow suite. They should probably match by default and be configured together.

- [ ] Version flags are not included in the man page, they are in help.

- [ ] Options are in a different order. For example I have an app with `-h, --help` and `-d, --debug` flags. In the man page help is shown first, in the help output debug is shown first.

- [ ] Man page suggests long args use `--option=OPTION`, help suggests `--option <OPTION>`.

- [ ] Man page puts default values on same line with option, help content puts it at the end of the explanation block.


---

_Comment by @epage on 2022-02-08 15:26_

Yes, they are different.  https://github.com/clap-rs/clap/issues/2914 will allow us to reuse help generation for man pages but there was interest in getting man generation going before that.

Could you open individual Issues for each of these as handling disparate problems in one issue makes things harder to track?

---

_Comment by @epage on 2022-02-28 17:49_

While I hate to lose valuable information like this, I'm closing this out as the submitter is putting the burden of properly tracking these as individual issues on the maintainers but we have limited time for doing so.

We'd welcome these being opened as distinct issues in the future.

---

_Closed by @epage on 2022-02-28 17:49_

---

_Label `A-man` added by @epage on 2022-09-30 18:32_

---

_Referenced in [clap-rs/clap#4308](../../clap-rs/clap/issues/4308.md) on 2022-09-30 18:38_

---

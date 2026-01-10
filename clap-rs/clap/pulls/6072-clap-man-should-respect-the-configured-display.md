---
number: 6072
title: clap_man should respect the configured display order for args and subcommands
type: pull_request
state: closed
author: ericgumba
labels: []
assignees: []
base: master
head: issue_3362
created_at: 2025-07-19T07:09:58Z
updated_at: 2025-10-22T16:46:14Z
url: https://github.com/clap-rs/clap/pull/6072
synced_at: 2026-01-10T01:28:26Z
---

# clap_man should respect the configured display order for args and subcommands

---

_Pull request opened by @ericgumba on 2025-07-19 07:09_

In https://github.com/clap-rs/clap/issues/3362 we have an issue where when we configure an arg via .display_order(int),
and then generate a manpage, the synposis and options will render
the order the args were provided to the App rather than the order they were configured

e.g

Command::new(name)
arg(Arg::new("few").short('b').display_order(2))
arg(Arg::new("bar").short('a').display_order(1))

will show

...
SYNOPSIS
    <name> [-b] [-a] ...

...
OPTIONS
    -b
    -a

instead of

...
SYNOPSIS
    <name> [-a] [-b] ...

...
OPTIONS
    -a
    -b

and so on. This fix adds sorting in the synopsis and options functions
responsible for generating the corresponding synopsis and options sections
of the manpage.

---

_@pksunkara approved on 2025-07-19 18:17_

I don't see any issue here. Unless @epage wants to do this with shell completions too in this PR.

---

_Review comment by @epage on `clap_mangen/tests/testsuite/common.rs`:1 on 2025-07-21 17:30_

> clap_man should respect the configured display order for args and subcommands

The title and the issue mention subcommands but they don't seem to be included in this PR.

---

_@epage reviewed on 2025-07-21 17:31_

---

_Review comment by @epage on `clap_mangen/src/render.rs`:38 on 2025-07-21 17:31_

Should we be using the same sort keys as help?

https://github.com/clap-rs/clap/blob/4c039309b614ee4523c67a243afc38af11860de9/clap_builder/src/output/help_template.rs#L1063-L1088

---

_Review comment by @epage on `clap_mangen/tests/testsuite/common.rs`:360 on 2025-07-21 17:32_

Do we have sort order tests within `tests/builder`?  If so, seems like we should use that for easier comparison and to help catch things we might miss.

---

_@epage reviewed on 2025-07-21 17:32_

---

_@ericgumba reviewed on 2025-07-24 12:47_

---

_Review comment by @ericgumba on `clap_mangen/tests/testsuite/common.rs`:360 on 2025-07-24 12:47_

After searching/grepping through `tests/builder` the only thing I found was in `derive_order.rs`. I did add more thorough test cases. 

---

_Review comment by @ericgumba on `clap_mangen/src/render.rs`:38 on 2025-07-24 12:47_

Thanks for the tip, I reused the same sort keys for both options and subcommands found in help_template.rs.

---

_@ericgumba reviewed on 2025-07-24 12:47_

---

_@ericgumba reviewed on 2025-07-24 12:48_

---

_Review comment by @ericgumba on `clap_mangen/tests/testsuite/common.rs`:1 on 2025-07-24 12:48_

Thank you for taking the time to review @epage . Also thanks for the catch, I added sorting in subcommands and tested both default order and custom configured ordering.

---

_Review comment by @epage on `clap_mangen/src/render.rs`:38 on 2025-07-29 00:25_

There a reason that the usage isn't using the key?

---

_@epage reviewed on 2025-07-29 00:25_

---

_@epage reviewed on 2025-07-29 00:25_

---

_Review comment by @epage on `clap_mangen/src/render.rs`:38 on 2025-07-29 00:25_

Whoops, looking at the wrong commit

---

_@epage reviewed on 2025-07-29 00:31_

---

_Review comment by @epage on `clap_mangen/src/render.rs`:4 on 2025-07-29 00:31_

This should be at the bottom of the file, not the top, so people start with the entry points that matter and dig into details as needed

---

_@epage reviewed on 2025-07-29 00:31_

---

_Review comment by @epage on `clap_mangen/src/render.rs`:4 on 2025-07-29 00:31_

This doesn't look like it needs to be `pub(crate)`

---

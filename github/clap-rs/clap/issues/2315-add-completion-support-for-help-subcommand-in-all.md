---
number: 2315
title: "Add completion support for `help` subcommand in all shells"
type: issue
state: closed
author: dkg
labels:
  - C-enhancement
  - A-completion
  - E-easy
  - ":money_with_wings: $10"
assignees: []
created_at: 2021-01-28T18:38:33Z
updated_at: 2022-08-26T20:28:22Z
url: https://github.com/clap-rs/clap/issues/2315
synced_at: 2026-01-07T13:12:19-06:00
---

# Add completion support for `help` subcommand in all shells

---

_Issue opened by @dkg on 2021-01-28 18:38_

Maintainer's notes
- Remember that you can do `cmd help sub1 sub2` which should be supported

---
Sequoia's `sq` uses clap to generate bash bindings.

`sq` has a subcommand interface, including the subcommand `help`.  `sq help` behaves the way that `sq --help` does.

That is, you can do `sq help inspect` (`inspect` is one of the `sq` subcommands).

The tab completion when a bash user types `sq help <TAB><TAB>` does *not* show the list of subcommands the way that `sq --help <TAB><TAB>` does.  Rather, it only shows the options like `-h`, `-V`, `--version`, etc. 

I [reported this concern to sequoia](https://gitlab.com/sequoia-pgp/sequoia/-/issues/656) and they indicated that it is a bug in clap.

### Versions

* **Rust**: 1.48.0
* **Clap**: 2.33.3


---

_Label `T: bug` added by @dkg on 2021-01-28 18:38_

---

_Comment by @pksunkara on 2021-01-28 18:40_

Hello,

We really appreciate your enthusiasm to contribute to this open source project and to help make it better. To make use of the maintainers' time more productively, we have set up a few issue templates that should be used when creating an issue.

We see that you made an accidental mistake because the issue you created doesn't follow the template. We will be closing this until that is rectified. Please do so and message here and we will gladly re-open this.

Thanks

---

_Closed by @pksunkara on 2021-01-28 18:40_

---

_Reopened by @pksunkara on 2021-01-28 18:41_

---

_Label `C: generator` added by @pksunkara on 2021-01-28 18:41_

---

_Label `T: enhancement` added by @pksunkara on 2021-01-28 18:41_

---

_Label `T: bug` removed by @pksunkara on 2021-01-28 18:41_

---

_Label `:money_with_wings: $10` added by @pksunkara on 2021-01-28 18:42_

---

_Renamed from "bash tab completion for clap-generated subcommand CLI doesn't show subcommands after "foo help <TAB><TAB>"" to "Add completion support for `help` subcommand in all shells" by @pksunkara on 2021-01-28 18:42_

---

_Added to milestone `3.1` by @pksunkara on 2021-01-28 18:42_

---

_Comment by @dkg on 2021-01-28 18:47_

thanks, @pksunkara !

---

_Referenced in [epage/clapng#175](../../epage/clapng/issues/175.md) on 2021-12-06 20:21_

---

_Removed from milestone `3.1` by @epage on 2021-12-13 15:48_

---

_Label `E-easy` added by @epage on 2021-12-13 15:49_

---

_Referenced in [clap-rs/clap#4077](../../clap-rs/clap/pulls/4077.md) on 2022-08-15 04:37_

---

_Referenced in [clap-rs/clap#4100](../../clap-rs/clap/issues/4100.md) on 2022-08-20 23:42_

---

_Comment by @talklittle on 2022-08-23 08:49_

Update: My second attempt was to put the help completion in `clap_complete/src/generator/utils.rs` (see #4077 discussion). It went poorly. Modifying the command tree after it has been built feels wrong, especially with the current Command API that treats a Command as mostly immutable. I couldn't get it working, and even if I did, it feels like it would easily break in the future.

My next attempt will be closer to #4077, putting the subtree-copy logic back in the parser where the "help" subcommand is first created. Plus fixing the missing recursion cases and adding a parser flag to toggle this slow logic.

---

_Referenced in [clap-rs/clap#4115](../../clap-rs/clap/pulls/4115.md) on 2022-08-25 21:08_

---

_Closed by @epage on 2022-08-26 20:28_

---

_Referenced in [clap-rs/clap#5547](../../clap-rs/clap/pulls/5547.md) on 2024-06-26 22:46_

---

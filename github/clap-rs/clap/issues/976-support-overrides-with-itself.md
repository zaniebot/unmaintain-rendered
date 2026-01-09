---
number: 976
title: "Support overrides_with(\"itself\")"
type: issue
state: closed
author: ericbn
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2017-05-29T23:02:22Z
updated_at: 2018-08-02T03:30:07Z
url: https://github.com/clap-rs/clap/issues/976
synced_at: 2026-01-07T13:12:19-06:00
---

# Support overrides_with("itself")

---

_Issue opened by @ericbn on 2017-05-29 23:02_

Suppose I have defined these:

    .arg(flag("foo").overrides_with("foo"))
    .arg(flag("bar").overrides_with("bar").takes_value(true).possible_values(&["0", "1", "2"]))

I'd like to allow the options to be able to be defined multiple times, the last one overriding previous ones.

So a call with options `--foo --foo --foo --bar=0 --bar=1 --bar=2` will not fail with `error: The argument '--foo' was provided more than once, but cannot be used multiple times` (or the same for `--bar`), and both foo, and bar with value 2 would be yield (resulting in the same as calling with only `--foo --bar=2`).

Or even better, maybe a `.setting(AppSettings::POSIX)` defined once would enable the previous behavior. Why would defining the same option twice or more yield an error? I just played with some command line tools here (`git status --short --short --short`, `ls -l -l -l`, `vi -R -R -R README.md`) and none complains of options provided multiple times...


---

_Referenced in [clap-rs/clap#970](../../clap-rs/clap/issues/970.md) on 2017-05-29 23:08_

---

_Label `C: parsing` added by @kbknapp on 2017-05-30 00:23_

---

_Label `D: intermediate` added by @kbknapp on 2017-05-30 00:23_

---

_Label `P2: need to have` added by @kbknapp on 2017-05-30 00:23_

---

_Label `T: bug` added by @kbknapp on 2017-05-30 00:23_

---

_Label `W: 2.x` added by @kbknapp on 2017-05-30 00:23_

---

_Comment by @kbknapp on 2017-05-30 00:24_

Thanks! I like the `AppSettings` idea too as defining all args as overridable by themselves would be handy.

---

_Comment by @ericbn on 2017-05-30 12:06_

Cool. Actually the [POSIX specifications](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap12.html) don't state nothing about overriding arguments. But the way the [POSIX implementation](http://pubs.opengroup.org/onlinepubs/9699919799/functions/getopt.html) usually involves a loop through the arguments, same arguments just get a count increment or override the previous assigned one.

Maybe the AppSettings could be something more explicit than just `AppSettings::POSIX`...

---

_Referenced in [BurntSushi/ripgrep#499](../../BurntSushi/ripgrep/pulls/499.md) on 2017-06-01 23:38_

---

_Referenced in [BurntSushi/ripgrep#491](../../BurntSushi/ripgrep/issues/491.md) on 2017-06-02 00:00_

---

_Referenced in [BurntSushi/ripgrep#553](../../BurntSushi/ripgrep/issues/553.md) on 2017-07-13 02:22_

---

_Referenced in [ogham/exa#152](../../ogham/exa/issues/152.md) on 2017-08-04 19:15_

---

_Comment by @lilyball on 2017-08-04 19:19_

There definitely should be an `AppSetting` for this (`POSIX` isn't a good name for this, maybe `AppSetting::AllowDuplicateFlags`?). And I suppose a per-arg setting as well, in case you want to e.g. allow it in general but disallow it on certain flags that take arguments (for example, you'd probably want to allow it on flags that take arguments specifying config values, e.g. sort options, but disallow it on flags that take arguments specifying input files).

[exa](https://github.com/ogham/exa/) just ran into this issue with its argument parsing (which was getopts), and it looks like their solution is to [implement a custom argument parser](https://github.com/ogham/exa/issues/152#issuecomment-320224452).

And it looks like ripgrep also [hit this limitation](https://github.com/BurntSushi/ripgrep/issues/553).

---

_Comment by @kbknapp on 2018-02-03 03:22_

[This tweet](https://twitter.com/burntsushi5/status/959624525368487939) got me thinking about this issue some more, and I'm thinking that because of the new lazy validation I'm doing this could actually be an easy add, and easily forward portable to the v3-master branch. I'm going to take a look at this tomorrow see and if it's as easy as I think it'll be.

---

_Added to milestone `2.29.3` by @kbknapp on 2018-02-03 03:22_

---

_Referenced in [clap-rs/clap#1164](../../clap-rs/clap/pulls/1164.md) on 2018-02-05 01:07_

---

_Closed by @kbknapp on 2018-02-05 03:27_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2018-02-05 03:30_

---

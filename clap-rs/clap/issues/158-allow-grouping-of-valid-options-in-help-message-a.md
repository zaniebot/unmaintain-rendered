---
number: 158
title: Allow grouping of valid options in help message (a-la docopt or getopts style)
type: issue
state: closed
author: kbknapp
labels:
  - S-waiting-on-decision
  - A-help
assignees: []
created_at: 2015-07-15T03:52:02Z
updated_at: 2018-08-02T03:29:41Z
url: https://github.com/clap-rs/clap/issues/158
synced_at: 2026-01-10T01:26:24Z
---

# Allow grouping of valid options in help message (a-la docopt or getopts style)

---

_Issue opened by @kbknapp on 2015-07-15 03:52_

Instead of breaking up each type of argument into, `FLAGS`, `OPTIONS` and `ARGS`, add an option which combines them all into a single `OPTIONS` section a-la docopt or getopts style.

Perhaps a `App::grouped_help(bool)`? Unless someone has a better name :P


---

_Label `feature request` added by @kbknapp on 2015-07-15 03:52_

---

_Label `maybe` added by @kbknapp on 2015-07-15 03:52_

---

_Label `P: nice to have` added by @kbknapp on 2015-07-15 04:11_

---

_Label `C: help message` added by @kbknapp on 2015-07-15 04:11_

---

_Label `D: easy` added by @kbknapp on 2015-07-15 04:11_

---

_Renamed from "Allow grouping of options in help message (a-la docopt or getopts style)" to "Allow grouping of valid options in help message (a-la docopt or getopts style)" by @kbknapp on 2015-07-15 04:25_

---

_Comment by @severen on 2015-07-15 04:28_

Isn't naming the method `App::grouped_help` somewhat arbitrary? Perhaps naming it `App::unified_args` or `App::unified_options` would be better?

Or perhaps it should just be the default, only behaviour?


---

_Comment by @kbknapp on 2015-07-15 12:26_

I don't want to make the default behavior yet because that'd be too drastic a change without bumping the major version. But I _can_ include the option. I like your naming better of `unified`.

Whats your opinion in the usage strings? Currently, with a program that has optional flags, options, and args it would read `program [FLAGS] [OPTIONS] [ARGS]`. When this setting is turned on, should flags and options be collapsed into just `[OPTIONS]`, or should all three be collapsed?

As for precedents, I believe `docopt` combines what `clap` calls `flags` and `options`, but _does_ separate out `args` (in usage strings at least).


---

_Comment by @kbknapp on 2015-07-16 05:31_

@SShrike This will be implemented via #160 although it's not the default, it should still be more to your liking ;)


---

_Closed by @kbknapp on 2015-07-16 05:43_

---

```yaml
number: 569
title: Options should accept leading hyphens in arguments where applicable
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: bug/568/leading-hyphen-values
created_at: 2017-07-29T16:17:33Z
updated_at: 2017-07-30T23:41:01Z
url: https://github.com/BurntSushi/ripgrep/pull/569
synced_at: 2026-01-12T18:23:13Z
```

# Options should accept leading hyphens in arguments where applicable

---

_@okdana_

Related to #568

This makes it so that options that take arguments which might conceivably begin with a leading hyphen actually accept arguments with leading hyphens. And it adds a test to make sure that works.

IMO, `clap`'s default behaviour here is really undesirable. I guess it works the way it does because it wants to detect when a user might have given an option without giving a required argument to the previous one, but there's not really any reliable way to discern the user's intent. You're trading one assumption (the POSIX-style assumption that `-1` in `rg -C -1` is simply a bad argument to `-C`) for another one (the assumption that the user tried to use `-C` without giving it an argument at all). Neither one is more reliable than the other, but the POSIX-style assumption is more consistent and less surprising, i think.

To that end i tried simply setting [`AppSettings::AllowLeadingHyphen`](https://docs.rs/clap/2.24.0/clap/enum.AppSettings.html#variant.AllowLeadingHyphen) on the app to always override that behaviour, but it caused bizarre errors — for example, when i'd run `rg -M20 -C1` it would interpret `-C1` as the pattern operand for some reason.

So... as a compromise, i added the `ArgSettings` version to all of the options that accept an arbitrary string. That seems to work, i guess.

---

_Merged by @BurntSushi on 2017-07-30 21:55_

---

_Closed by @BurntSushi on 2017-07-30 21:55_

---

_Comment by @BurntSushi on 2017-07-30 21:56_

Yup, this seems reasonable to me!

I agree that it might be worth it for clap to rethink how it does things here, but the last time I dug into it with @kbknapp it seemed more complex than what I had initially thought. I can't remember the details now though.

---

_Comment by @kbknapp on 2017-07-30 22:43_

FWIW This is something I've been thinking about for 3.x  specifically. Granted the work on that has been going *far* slower than I'd like/anticipated just due to my paid job monopolizing my time as of late.

The hard part about this issue (for 2.x mind you; 3.x will handle this differently in a manner consistent with the OPs ideas) is knowing when to present which kind of error. 2.x uses almost no backtracking, and must decide which argument it's currently parsing. This sounds simple in most cases, but to do it in the generic case is rather difficult because by default 2.x allows multiple values to be passed in to a single option consecutively (i.e. `--option val val`). When combined with all other forms of arguments, *and* providing a solution that works for all applications and not just a single implementation it becomes tricky.

3.x is tackling this by having more sane defaults. For instance by default only one value per instance of an option, such as `--option val --option val`, and disallowing empty values by default such as `--option val | --option`. All the old behavior will become opt-in, rather than opt-out that they currently are. Likewise, values which start with a hyphen should become the default as it'll be easy to detect the complicated case if someone has opt-ed in to all these strange behaviors, making it the exception and no longer the rule.

Those are probably the biggest changes to defaults coming, as those are the most confusing for users from my optic.

---

_Comment by @BurntSushi on 2017-07-30 23:41_

@kbknapp Thanks for thinking about this! Can't wait for clap 3.x. :-)

---

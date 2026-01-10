---
number: 1013
title: The help message offset is inconsistent
type: issue
state: open
author: marmistrz
labels:
  - C-bug
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2017-07-25T10:40:06Z
updated_at: 2021-12-09T17:06:37Z
url: https://github.com/clap-rs/clap/issues/1013
synced_at: 2026-01-10T01:26:41Z
---

# The help message offset is inconsistent

---

_Issue opened by @marmistrz on 2017-07-25 10:40_

### Rust Version

1.19.0

### Affected Version of clap

2.25.1

### Issue description
Take this Rust project: https://github.com/marmistrz/randmockery/commit/3fa575f6642a9679c5fe0f7432803e21de4566dd

(I specify the commit because the app may be developed further). When launching `randmockery --help` I get
```
randmockery 0.1.0
Marcin Mielniczuk <mail>
Proof-of-concept that we can mock randomness

USAGE:
    randmockery [OPTIONS] <command>...

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

OPTIONS:
    -l <library>...        Adds a library to be injected

ARGS:
    <command>...    The command to be executed
```

I'd expect all the help messages to be aligned to each other:
```
FLAGS:
    -h, --help             Prints help information
    -V, --version          Prints version information

OPTIONS:
    -l <library>...        Adds a library to be injected

ARGS:
    <command>...           The command to be executed
```



---

_Comment by @kbknapp on 2017-10-04 02:48_

The alignment is per *section* (i.e. `ARGS`, `FLAGS`, etc.). In small programs like the example, I agree it looks better aligned as a single unit, but in large programs this actually looks worse (subjective opinion I know).

I'm going to close this as just by design. But I'm open to discussing the issue if there is an argument to be made.

---

_Closed by @kbknapp on 2017-10-04 02:48_

---

_Comment by @marmistrz on 2017-10-04 08:26_

Since it's subjective, what about making it configurable?

---

_Comment by @kbknapp on 2017-10-24 01:54_

@marmistrz apologies I didn't get the ping that a comment was made!

In general I'm ok with making things configurable, and I'm not against adding a setting does exactly this. I'll re-open the issue but can't promise when it'll be done due to my work schedule lately. I'd be happy to help someone figure this out if they'd like to work on a fairly easy to implement issue.

---

_Reopened by @kbknapp on 2017-10-24 01:54_

---

_Label `C: help message` added by @kbknapp on 2017-10-24 01:55_

---

_Label `D: easy` added by @kbknapp on 2017-10-24 01:55_

---

_Label `P4: nice to have` added by @kbknapp on 2017-10-24 01:55_

---

_Label `T: new setting` added by @kbknapp on 2017-10-24 01:55_

---

_Label `W: 2.x` added by @kbknapp on 2017-10-24 01:55_

---

_Referenced in [clap-rs/clap#1277](../../clap-rs/clap/pulls/1277.md) on 2018-05-28 13:36_

---

_Closed by @kbknapp on 2018-07-22 02:32_

---

_Reopened by @pksunkara on 2020-02-04 12:09_

---

_Label `W: 2.x` removed by @pksunkara on 2020-02-04 12:09_

---

_Added to milestone `3.1` by @pksunkara on 2020-02-04 12:09_

---

_Comment by @pksunkara on 2021-10-27 09:24_

@epage What do you think of this?

---

_Comment by @epage on 2021-10-27 18:05_

My general stance is to push back against configuration and prefer making a decision one way or the other :), depending on how hard the workarounds are, the number of people affected, etc.

For those unaware, my motivations for low configuration are:
- Keep the API more focused.  Its hard to find relevant parts of the API because the surface area is so large.
- Keep the number of supported cases down (not number of flags but their permutations) since we've seen a lot of problems crop up from unexpected interactions
- Keep the code base smaller / more nimble

We might be able to re-frame this to have it *and* keep low configuration, like with https://github.com/clap-rs/clap/issues/2913 or https://github.com/clap-rs/clap/issues/2914.   We could keep the mainline `clap` behavior less configurable but allow people in the more corner cases to drop down to lower level APIs that offer them more flexibility to do what they want  We can then keep on an eye on these (especially with crates.io's reverse dependencies) and use that as feedback for where we should take clap.

As for timelines, I wouldn't think this is a priority for 3.1.  Putting it in my buckets (Focus Area, Planned, Someday), the prior conversation makes this seem like a Planned but my suggestion above makes this sound like a Someday or that it is Planned but blocked on #2913 and would then be targeted as a change to the lower level help generator only.

---

_Comment by @pksunkara on 2021-10-27 20:32_

> My general stance is to push back against configuration and prefer making a decision one way or the other :), depending on how hard the workarounds are, the number of people affected, etc.

I guess that's where we differ. I envision clap (and clap had always been) to allow all kinds of configuration so that users can choose their own preferences.

Our job as maintainers is to grow the codebase to make sure those configuration options are not workarounds but rather modular and the number of people affected are less because they can opt-out.

In fact, I would wager that the reason clap is so popular is because of it's flexibility.

---

_Referenced in [epage/clapng#75](../../epage/clapng/issues/75.md) on 2021-12-06 16:34_

---

_Label `T: new setting` removed by @epage on 2021-12-08 20:38_

---

_Label `C-bug` added by @epage on 2021-12-08 20:38_

---

_Label `P4: nice to have` removed by @epage on 2021-12-09 17:06_

---

_Label `D: easy` removed by @epage on 2021-12-09 17:06_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-09 17:06_

---

_Removed from milestone `3.1` by @epage on 2021-12-09 17:06_

---

_Referenced in [ducaale/xh#216](../../ducaale/xh/pulls/216.md) on 2022-01-01 19:37_

---

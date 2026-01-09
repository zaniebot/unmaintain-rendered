---
number: 1023
title: "Feature Request: Override setting for groups"
type: issue
state: open
author: Phlosioneer
labels:
  - C-enhancement
  - A-builder
  - E-medium
  - A-validators
assignees: []
created_at: 2017-08-09T03:26:35Z
updated_at: 2021-12-09T17:08:31Z
url: https://github.com/clap-rs/clap/issues/1023
synced_at: 2026-01-07T13:12:19-06:00
---

# Feature Request: Override setting for groups

---

_Issue opened by @Phlosioneer on 2017-08-09 03:26_

There are many instances of command line utilities that have groups of flags, all able to override each other.

For example, as mentioned in [this issue](https://github.com/uutils/coreutils/issues/1006), the `od` utility has 10 flags that all override each other: `abcdfilosx`. The last appearing flag is the one that is used. Likewise, the `ls` utility has similar "groups" of overriding flags: [ls manual](https://man.openbsd.org/ls.1)
```
It is not an error to specify more than one of the following mutually exclusive options:
-1, -C, -g, -l, -m, -n, and -x; and -c, -f, -S, -t, and -u. Where more than one option is specified
from the same mutually exclusive group, the last option given overrides the others, except
that -l always overrides -g; and -f always overrides -c, -S, -t, and -u.
```
If this feature existed, one could input it into clap like this:
```
// Note, my syntax might not be perfect.
let foo = App::new("ls")
  .args(&["1", "C", "g", "l", "m", "n", "x"])
  .group(ArgGroup::with_name("formatting")
    .override_with_self(true);  // New feature.
  );
```
This is technically already do-able; just set every individual argument to overload every other argument. However, that's inefficient, unreasonably difficult to input, and grows faster than exponentially.

The proposed feature would be one new function: `ArgGroup::override_with_self(value: bool) -> Self`.



Implementation for just flags would imply `Arg::multiple(true)` for its args and then simply force `ArgMatches::value_of` to return the _last_ parsed value. It may be more complicated to account for arguments with values (`--foo bar`)? I'm not familiar with the internal workings of clap-rs, so I don't know how difficult it would be to add this feature.

This feature would be _very_ useful when re-writing old command-line apps, as this kind of group-override pattern shows up a lot.


---

_Comment by @kbknapp on 2017-10-04 03:18_

I like this idea! It would also help with a similar issue I had when converting `exa` to use clap. I can't say I'll implement this tomorrow, as my plate is super full, but I *do* want this feature think it'd making specifying these large groups of overrides *much* easier.

Thanks for the details and the suggestion! :+1: 

---

_Label `C: arg groups` added by @kbknapp on 2017-10-04 03:18_

---

_Label `C: flags` added by @kbknapp on 2017-10-04 03:18_

---

_Label `D: intermediate` added by @kbknapp on 2017-10-04 03:18_

---

_Label `P3: want to have` added by @kbknapp on 2017-10-04 03:18_

---

_Label `T: enhancement` added by @kbknapp on 2017-10-04 03:18_

---

_Label `W: 2.x` added by @kbknapp on 2017-10-04 03:18_

---

_Comment by @Phlosioneer on 2017-10-04 05:46_

Note: another option is to simply have a method like ArgMatches::values_of_group that just lists the command matches, in order, within the group, and their values. Then the application can worry about overrides; this would help in particular with nasty corner cases like the above "except that -l always overrides -g" etc.

this option exposes more API to worry about... but, there's very little state to keep track of, so parsing logic might be made easier. Still not familiar with the internals of clap, though.

---

_Comment by @kbknapp on 2017-10-04 14:33_

Values can already be checked by group, but since it's a relatively unused feature (or rather undocumented feature) I can't speak to any edge case bugs that exist.

`matches.value_of("group")` should work for testing purposes.

---

_Comment by @Phlosioneer on 2017-10-05 14:13_

good to know, thanks.

---

_Label `W: 3.x` added by @kbknapp on 2018-07-22 02:35_

---

_Label `W: 2.x` removed by @kbknapp on 2018-07-22 02:35_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-09 07:08_

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Referenced in [epage/clapng#77](../../epage/clapng/issues/77.md) on 2021-12-06 16:35_

---

_Label `C: arg groups` removed by @epage on 2021-12-08 19:57_

---

_Label `C: flags` removed by @epage on 2021-12-08 19:57_

---

_Label `A-builder` added by @epage on 2021-12-08 19:57_

---

_Label `C: validators` added by @epage on 2021-12-08 20:22_

---

_Label `D: medium` removed by @epage on 2021-12-09 17:08_

---

_Label `P3: want to have` removed by @epage on 2021-12-09 17:08_

---

_Label `E-medium` added by @epage on 2021-12-09 17:08_

---

_Removed from milestone `3.1` by @epage on 2021-12-09 17:08_

---

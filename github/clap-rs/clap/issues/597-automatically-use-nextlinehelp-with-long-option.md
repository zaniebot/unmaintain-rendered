---
number: 597
title: Automatically use NextLineHelp with long option names/values
type: issue
state: closed
author: joshtriplett
labels:
  - C-enhancement
  - A-help
assignees: []
created_at: 2016-07-24T01:03:31Z
updated_at: 2018-08-02T03:29:52Z
url: https://github.com/clap-rs/clap/issues/597
synced_at: 2026-01-07T13:12:19-06:00
---

# Automatically use NextLineHelp with long option names/values

---

_Issue opened by @joshtriplett on 2016-07-24 01:03_

Based on discussion in https://github.com/kbknapp/clap-rs/issues/587 .

Clap should limit the maximum width of the option name/value column, and automatically use NextLineHelp otherwise.


---

_Referenced in [clap-rs/clap#587](../../clap-rs/clap/issues/587.md) on 2016-07-24 01:03_

---

_Label `T: enhancement` added by @kbknapp on 2016-07-24 03:22_

---

_Label `C: help message` added by @kbknapp on 2016-07-24 03:22_

---

_Label `D: intermediate` added by @kbknapp on 2016-07-24 03:22_

---

_Comment by @kbknapp on 2016-08-26 15:31_

I think the base has been laid to do this, where the flags+spaces > longest word in the help string. This should be an easy add now. @joshtriplett is there another metric you'd use to determine when a flag/option is "too long" and should trigger use of the next line?


---

_Added to milestone `2.10.5` by @kbknapp on 2016-08-26 15:31_

---

_Comment by @joshtriplett on 2016-08-27 04:30_

@kbknapp In terms of heuristics, I'd suggest checking if the name of an option, its argument, and the needed whitespace would take up more than 25% of the terminal width. For an 80-character terminal, that'd be 20 characters, which seems reasonable.

Actually, somewhere around 20 characters is probably a reasonable upper limit in general, since even on a wider terminal you don't want an excessive amount of whitespace on other options caused by a single long option name.  Perhaps "longer than 25% of the terminal width or 24 characters, whichever is smaller"?


---

_Comment by @kbknapp on 2016-08-27 23:36_

@joshtriplett I did some checking just based on random Rust CLI, and on 80 column terminals quite a significant ratio of help lines would end up on the next line, even though the entirety of their help text fits. So I thought, perhaps only those who's help text is greater than 75% when the flags/options values, etc. is greater than 25%, which works, but still ends up (IMO) jumping to the next line too early.

So what I'd like to do, just as a start since we can change this later based on feedback and just aesthetics is to use 40% as the margin, and only when the help text exceeds the remaining 60%.

Perhaps even later we could make this editable :stuck_out_tongue_winking_eye: 


---

_Comment by @joshtriplett on 2016-08-28 00:23_

@kbknapp The heuristic should definitely include "some help texts" (not just the current one) would wrap.  40% seems worth trying.  And I'd rather see a sensible default heuristic than configurability. :)


---

_Comment by @kbknapp on 2016-08-28 01:11_

@joshtriplett So I just finished implementing this, and ended up going with something far closer to what you originally stated. It moves the help text to the next line only if it doesn't fit, no matter the length of the flags/options/spaces/values. The exception to this is, is when the flags/options/spaces/values is less than 25%, at which point it just wraps normally as it would have. Of the tests I've run, this seems to be the least alerting and most natural.

i.e. at 70 width, the `-c, --cafe` (etc.) is greater than 25% and the help text is long, so this wraps to the next line.

```
ctest 0.1

USAGE:
    ctest [OPTIONS]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

OPTIONS:
    -c, --cafe <FILE>
        A coffeehouse, coffee shop, or café is an establishment which
        primarily serves hot coffee, related coffee beverages
        (e.g., café latte, cappuccino, espresso), tea, and other
        hot beverages. Some coffeehouses also serve cold beverages
        such as iced coffee and iced tea. Many cafés also serve
        some type of food, such as light snacks, muffins, or pastries.
```


---

_Comment by @joshtriplett on 2016-08-28 01:44_

That sounds good to me.

I'd still love to see the paragraph aligned with the help text of the other options, though.  But that's a separate issue (#587).


---

_Comment by @kbknapp on 2016-08-28 02:00_

Yeah, I'll have to play with it some more since those two paragraphs are aligned separately...so we'll see as I get more time to tinker! :+1: 


---

_Comment by @joshtriplett on 2016-08-28 02:04_

@kbknapp Because one is under FLAGS and the other is under OPTIONS?  For a first pass, if it's easier, it'd be fine if all the FLAGS had a single indent level for help and all the OPTIONS had a single indent level for help, even if they didn't have the same one.  I use `UnifiedHelpMessage` anyway. :)


---

_Comment by @kbknapp on 2016-08-28 02:31_

Yeah they're determined independently...unless you use `UnifiedHelpMessage` :wink: But  I think there's probably an ergonomic way to share that info and implement #587 easily. I'll just have to put some thought into it.


---

_Closed by @kbknapp on 2016-08-28 03:42_

---

_Referenced in [clap-rs/clap#3300](../../clap-rs/clap/issues/3300.md) on 2022-11-13 04:26_

---

_Referenced in [clap-rs/clap#4550](../../clap-rs/clap/issues/4550.md) on 2022-12-13 06:33_

---

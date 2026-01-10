---
number: 1469
title: optionally wrap help lines at less than terminal width
type: issue
state: closed
author: wookietreiber
labels:
  - C-enhancement
  - A-help
  - S-waiting-on-author
assignees: []
created_at: 2019-05-13T16:03:53Z
updated_at: 2021-12-10T18:04:03Z
url: https://github.com/clap-rs/clap/issues/1469
synced_at: 2026-01-10T01:26:54Z
---

# optionally wrap help lines at less than terminal width

---

_Issue opened by @wookietreiber on 2019-05-13 16:03_

Maintainer's notes:

Workaround
```rust
let max_width = std::env::var("FOO_MAX_WIDTH")
    .and_then(|s| s.parse::<usize>().ok())
    .unwrap_or(100);
app
    .max_term_width(max_width)
```
---
<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

* rustc 1.34.1 (fc50f328b 2019-04-24)

### Affected Version of clap

* 2.33.0

### Bug or Feature Request Summary

I have just browsed through quite some `man` pages (mostly `git help cmd`) and clap-generated long help outputs (`bat`, `fd`, some of my own tools). While doing that, I have noticed that `man` wraps **paragraphs** not at terminal width but less than that. This can be considered a follow-up to #428 and #451.

I think, I have noticed this because I have been looking at `man` pages for quite some time and, while the clap-generated **long** help output (`--help`) looks very similar to a `man` page (thank you for that, it's awesome!), it sometimes looked a bit weird (to me). I finally noticed what was bugging me and it is the wrapping.

In my opinion (yes, subjectively, I'm getting to that), it is more readable to wrap at less than terminal width. Maybe you feel the same way. Even if you do not feel the same way, you might (objectively) consider for clap-generated **long** help output to emulate the behavior of `man` pages in this regard.

It is *subjective* what looks best, to wrap at term width or to wrap at term width minus some value. Thus, *objectively*, this is not something that the CLI app **developer** should decide, but the user herself/himself. For this *objective* reason, I am suggesting to let the app **user** have the final say in this, not the **developer** as is done now. I suggest to add an environment variable that clap looks for, e.g. like in this `~/.bash_profile` snippet:

```bash
# this if executes only if shell is interactive
if [[ $- == *i* ]]
then
  # if set, will override max_term_width
  # man for man page style
  # max for max term width
  # u64 for whatever unsigned integer you want
  export CLAP_WRAP_STYLE="[man|max|u64]"
fi
```

---

**Note:** I tested how `man` wraps with different terminal sizes. On standard terminal of 80 x 24 the wrapping is at column 78, so `columns - 2` and with my usual terminal of 147 columns it's at `columns - 4`. I guess, they decide somehow. I haven't looked at their code yet.

---

_Label `C: help message` added by @CreepySkeleton on 2020-02-01 13:52_

---

_Label `T: new feature` added by @CreepySkeleton on 2020-02-01 13:52_

---

_Label `W: maybe` added by @CreepySkeleton on 2020-02-01 13:54_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-09 07:52_

---

_Referenced in [epage/clapng#118](../../epage/clapng/issues/118.md) on 2021-12-06 18:35_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:16_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:16_

---

_Removed from milestone `3.1` by @epage on 2021-12-09 20:00_

---

_Comment by @epage on 2021-12-09 20:01_

How is this different than `max_term_width` requested in https://github.com/clap-rs/clap/issues/653 and added in #654?  Am I missing some context for how the two differ?

---

_Label `S-waiting-on-decision` removed by @epage on 2021-12-09 20:01_

---

_Label `S-waiting-on-author` added by @epage on 2021-12-09 20:01_

---

_Comment by @pksunkara on 2021-12-09 20:30_

From what I understood when the issue was created, they are asking some kind of runtime max width specifying capability.

---

_Comment by @epage on 2021-12-09 20:36_

You are right, I missed this part:

> Thus, objectively, this is not something that the CLI app developer should decide, but the user herself/himself. For this objective reason, I am suggesting to let the app user have the final say in this, not the developer as is done now

If there is a standard env variable across apps, I can see reading it, like https://no-color.org/, otherwise, I think this is best left to the app developer to define and read the variable, setting `max_term_width`, like
```rust
let max_width = std::env::var("FOO_MAX_WIDTH")
    .and_then(|s| s.parse::<usize>().ok())
    .unwrap_or(100);
app
    .max_term_width(max_width)
```

The developer can read this and then apply it to other areas of their app as well.

Any reason for us to own this behavior in clap?

---

_Comment by @pksunkara on 2021-12-10 02:29_

> Any reason for us to own this behavior in clap?

 The only scenario I can think of is allow the user to specify the width via an arg. But this might be niche?

---

_Comment by @epage on 2021-12-10 18:04_

I've noted the way for users to directly do this in the Issue description.  I think for now we should consider that the recommended solution.  If there are concerns with this, feel free to speak up!  It will help us know whether to re-evaluate.

---

_Closed by @epage on 2021-12-10 18:04_

---

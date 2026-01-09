---
number: 2806
title: User confusion over help messages not being colored, despite color being enabled
type: issue
state: closed
author: epage
labels:
  - C-enhancement
  - M-breaking-change
assignees: []
created_at: 2021-10-04T18:37:47Z
updated_at: 2021-10-11T15:23:30Z
url: https://github.com/clap-rs/clap/issues/2806
synced_at: 2026-01-07T13:12:19-06:00
---

# User confusion over help messages not being colored, despite color being enabled

---

_Issue opened by @epage on 2021-10-04 18:37_

### Rust Version
1.55.0

### Affected Version of clap

v3.0.0-beta.4

### Expected Behavior Summary

When the color feature is enabled, all coloring is done

### Actual Behavior Summary

Users have to know that the `ColoredHelp` setting exists and set it

### Context

This came up when discussing clap's defaults. In addition being [a very popular setting among clap CLIs](https://github.com/clap-rs/clap/discussions/2627#discussion-3478195), a [user reported](https://github.com/clap-rs/clap/discussions/2627#discussioncomment-1193448)

> I recently started using clap, and it took a bit to comprehend help messages were not coloured despite the `color` feature being the default. Lots of head scratching followed by "really?".
> 
> I understand the potential argument against having _any_ colourisation by default (ie not a default feature), but regardless of the feature being default or not, colourisation should be the default when the feature is enabled.

Depending on what the motivation for this flag existing, we should either
- Remove it completely and always color help when color is on
- Change the default (and possibly rename to reflect that changed behavior)

---

_Label `T: bug` added by @epage on 2021-10-04 18:37_

---

_Label `E: breaking change` added by @epage on 2021-10-04 18:37_

---

_Label `C: colorizing` added by @epage on 2021-10-04 18:37_

---

_Added to milestone `4.0` by @epage on 2021-10-04 18:37_

---

_Comment by @joshtriplett on 2021-10-04 21:40_

I completely agree: if you have the color feature enabled, you should get color automatically by default unless you disable it.

People can always disable `ColoredHelp` if they don't want it, *or* disable the color feature entirely to not get color anywhere.

---

_Comment by @pksunkara on 2021-10-09 00:12_

@epage Do you have a design for this if we want to get this into v3?

---

_Label `T: bug` removed by @pksunkara on 2021-10-09 00:13_

---

_Label `T: enhancement` added by @pksunkara on 2021-10-09 00:13_

---

_Comment by @epage on 2021-10-09 10:23_

While I want to fix our defaults, I don't think this meets the minimum bar for the 3.0 milestone.  Whatever the fix will be, will be small, so its easy to slip in, but I'm concerned about the decision making slowing us down and distracting us from other work.

---

_Comment by @kbknapp on 2021-10-10 18:06_

This one seems much more simple to me to decide on than some of the other defaults questions. I essentially agree with @joshtriplett 100%. If our default features includes color, we color the terminal by default. However, we *must* maintain a way for users to turn that off both at runtime and by just not compiling the color feature.

---

_Comment by @joshtriplett on 2021-10-11 09:21_

I agree; this one seems straightforward compared to other potential changes to defaults.

---

_Comment by @epage on 2021-10-11 13:51_

> If our default features includes color, we color the terminal by default. However, we must maintain a way for users to turn that off both at runtime and by just not compiling the color feature.

We still have the Always, Auto, Never settings.

If there is no real dispute on this (which is never really predictable, see bike shedding), I'll go ahead and remove it.

---

_Removed from milestone `4.0` by @epage on 2021-10-11 13:51_

---

_Added to milestone `3.0` by @epage on 2021-10-11 13:51_

---

_Referenced in [clap-rs/clap#2845](../../clap-rs/clap/pulls/2845.md) on 2021-10-11 14:03_

---

_Closed by @bors[bot] on 2021-10-11 15:23_

---

_Referenced in [clap-rs/clap#2956](../../clap-rs/clap/pulls/2956.md) on 2021-11-03 06:53_

---

_Referenced in [epage/clapng#59](../../epage/clapng/pulls/59.md) on 2021-12-04 18:02_

---

_Referenced in [solana-labs/solana-program-library#4511](../../solana-labs/solana-program-library/issues/4511.md) on 2023-06-08 23:26_

---

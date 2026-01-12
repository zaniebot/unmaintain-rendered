```yaml
number: 952
title: Implement and Derive common traits
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
  - M-breaking-change
  - E-medium
  - E-easy
assignees: []
created_at: 2017-05-09T22:06:32Z
updated_at: 2020-12-26T17:02:29Z
url: https://github.com/clap-rs/clap/issues/952
synced_at: 2026-01-12T16:14:10Z
```

# Implement and Derive common traits

---

_@kbknapp_

Eagerly derive/implement common traits for all public types. I haven't yet looked at which ones would actually be appropriate to implement. The list below is just a quick list.

I'm not 100% sure, but I think adding these would be a breaking change?

 - [ ] App
   - [ ] Clone
   - [ ] Eq
   - [ ] PartialEq
   - [ ] Ord
   - [ ] PartialOrd
   - [ ] Hash
   - [ ] Debug,
   - [ ] Display
   - [ ] Default
 - [ ] Arg
   - [ ] Clone
   - [ ] Eq
   - [ ] PartialEq
   - [ ] Ord
   - [ ] PartialOrd
   - [ ] Hash
   - [ ] Debug,
   - [ ] Display
   - [ ] Default
 - [ ] ArgMatches
   - [ ] Clone
   - [ ] Eq
   - [ ] PartialEq
   - [ ] Ord
   - [ ] PartialOrd
   - [ ] Hash
   - [ ] Debug,
   - [ ] Display
   - [ ] Default
 - [ ] Values
   - [ ] Copy
   - [ ] Clone
   - [ ] Eq
   - [ ] PartialEq
   - [ ] Ord
   - [ ] PartialOrd
   - [ ] Hash
   - [ ] Debug,
   - [ ] Display
   - [ ] Default
 - [ ] ValuesOs
   - [ ] Copy
   - [ ] Clone
   - [ ] Eq
   - [ ] PartialEq
   - [ ] Ord
   - [ ] PartialOrd
   - [ ] Hash
   - [ ] Debug,
   - [ ] Display
   - [ ] Default

---

_Label `D: easy` added by @kbknapp on 2017-05-09 22:06_

---

_Label `E: breaking change` added by @kbknapp on 2017-05-09 22:06_

---

_Label `M: mentored` added by @kbknapp on 2017-05-09 22:06_

---

_Label `P3: want to have` added by @kbknapp on 2017-05-09 22:06_

---

_Label `T: enhancement` added by @kbknapp on 2017-05-09 22:06_

---

_Label `W: 3.x` added by @kbknapp on 2017-05-09 22:06_

---

_Added to milestone `Lib Blitz Overhaul` by @kbknapp on 2017-05-09 22:06_

---

_Referenced in [clap-rs/clap#950](../../clap-rs/clap/issues/950.md) on 2017-05-09 22:07_

---

_Comment by @Vinatorul on 2017-06-18 17:46_

I can work on this. I am not sure about breaking change right now, it depends from associated changes.

---

_Comment by @kbknapp on 2017-06-18 18:01_

Yeah I'm concerned with the breaking changes implications. I've got a local 3x branch that I've been working on the past few days. Once I get it to a state where it actually builds I'll push the branch and you can run through the types making sure they're all deriving the traits they should. :+1:

I've got one more work trip coming up soon, but I'm hoping to have this branch pushed sometime this coming week.

---

_Comment by @Vinatorul on 2017-06-18 18:18_

Nice, is there any issues I can work on while you will be away? Are you still using `gitter`?

---

_Comment by @kbknapp on 2017-06-20 03:05_

@Vinatorul The gitter is still up, and I get pinged on mobile (usually) when new messages are posted. Really any of the current feature requests *could* get knocked out, but 3.x is being heavily refactored, so it'll probably just be easier to wait until I get it building.

I now have a 3x-dev branch up, but it doesn't compile yet as it's a messy state of partially refactored. I'm hoping to get a large chunk of it complete tomorrow while on an airplane :smile: Once the refactoring is done, adding the new features will be easy, and you can jump on in at that point. I'll publish a list of priorities as well.

Once it's good to go, I'll create a 3x-master branch and start merging feature PRs onto that branch.

---

_Removed from milestone `Lib Blitz Overhaul` by @kbknapp on 2018-02-02 02:03_

---

_Added to milestone `v3-alpha1` by @kbknapp on 2018-02-02 02:03_

---

_Label `good first issue` added by @kbknapp on 2018-07-22 02:24_

---

_Removed from milestone `v3-alpha.2` by @pksunkara on 2020-02-01 07:45_

---

_Added to milestone `v3.0` by @pksunkara on 2020-02-01 07:45_

---

_Comment by @sassman on 2020-05-06 08:30_

Can this issue be tackled now? If that is the case, I'd might have a look at it.

---

_Comment by @pksunkara on 2020-05-06 08:31_

Yes, please. Which is why I added it to the rust weekly.

---

_Comment by @sassman on 2020-05-06 08:33_

There I saw it and it might be an easy candidate.

---

_Comment by @CreepySkeleton on 2020-05-06 15:55_

Regarding certain traits:

* Default is meaningless for `Arg`, `App`, `ArgGroup`. 
* Implementing `PartialOrd + Ord` is _probably_ next to impossible because they aren't implemented  for `IndexMap`. Same with `Hash`

---

_Comment by @kbknapp on 2020-05-06 16:10_

With #1365 we should look at how much this increases the binary size and probably feature gate this (it's ok if it's in the default features) if it negatively effects the size. I *think* LLVM is pretty good about dead code elimination and removing any trait impls that we impl (or derive) but aren't actually used in the resulting binary...but I'm not 100% certain in all cases. I know there are specific cases where LLVM won't remove dead code, but I believe that is tied to generics and monomorphic trait impls IIRC.

---

_Comment by @pickfire on 2020-05-13 02:32_

@sassman are you working on all of it or do you need some help?

---

_Comment by @sassman on 2020-05-13 08:46_

@pickfire I started by `PartialEq` on `ArgGroup` and `App` and wrote some little tests for it. But I'm not sure if tests would be necessary for those standard traits.

---

_Comment by @pksunkara on 2020-05-13 08:48_

Tests are not needed. Just write `derive(PartialEq)` and if it compiles, then it's okay. That is why this is a beginner issue 😄 

---

_Comment by @pksunkara on 2020-05-13 08:49_

Tests are only needed if there is a custom implementation somewhere.

---

_Comment by @pickfire on 2020-05-13 09:22_

@pksunkara It would be fun if beginners are able to claim parts of this issue like https://github.com/rust-dc/fish-manpage-completions/issues/2

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2020-05-14 14:15_

---

_Referenced in [clap-rs/clap#2266](../../clap-rs/clap/pulls/2266.md) on 2020-12-23 23:47_

---

_Closed by @pksunkara on 2020-12-26 17:02_

---

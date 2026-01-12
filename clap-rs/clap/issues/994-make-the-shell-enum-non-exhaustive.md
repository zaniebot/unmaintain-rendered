```yaml
number: 994
title: Make the Shell enum non-exhaustive
type: issue
state: closed
author: PlasmaPower
labels:
  - E-medium
  - A-completion
  - E-easy
assignees: []
created_at: 2017-07-04T18:49:19Z
updated_at: 2018-08-02T03:30:08Z
url: https://github.com/clap-rs/clap/issues/994
synced_at: 2026-01-12T16:14:10Z
```

# Make the Shell enum non-exhaustive

---

_@PlasmaPower_

(sorry for not using the template, but I don't think it's applicable here)

Currently, the `clap::Shell` enum (as defined [here](https://github.com/kbknapp/clap-rs/blob/e27a6ffbde87a35a367cd3c5b7369ef8173d0e7a/src/completions/shell.rs#L8)) can be matched against, which means that supporting a new shell is a breaking change. I'd like to propose making the enum nonexhaustive. Until rust-lang/rfcs#2008 or an equivalent gets passed and stabilized, this would be done through a doc hidden `__nonexhaustive_dont_match` variant (or similar). Using that variant would be considered unsupported, and the existing enum functions would ignore it (or panic if passed in). This would be a breaking change, but it would prevent future breaking changes whenever a shell is added.

It's unfortunate this didn't happen with clap v2. I'm not sure if this would be a major enough change to warrant waiting for clap v3, that'll be up to the maintainers of course.

---

_Comment by @kbknapp on 2017-07-29 21:10_

Since I'm actively working on v3 (see v3-master branch) I can just incorporate this into the new version. Actually I'm trying to pull out all the completion code into `clap_completions` so that changes such as this can be breaking and bump the major without changing `clap` proper.

Sorry for the long wait on a response :\

---

_Comment by @kbknapp on 2017-07-29 21:12_

Oh and no worries with the template. If it doesn't apply, no need to use it :wink:

---

_Referenced in [clap-rs/clap#1017](../../clap-rs/clap/pulls/1017.md) on 2017-07-29 21:37_

---

_Label `C: completion gen` added by @kbknapp on 2017-10-04 02:43_

---

_Label `W: 3.x` added by @kbknapp on 2017-10-04 02:43_

---

_Added to milestone `v3-alpha1` by @kbknapp on 2018-02-02 01:59_

---

_Label `P2: need to have` added by @kbknapp on 2018-07-22 02:31_

---

_Label `D: easy` added by @kbknapp on 2018-07-22 02:31_

---

_Label `M: mentored` added by @kbknapp on 2018-07-22 02:31_

---

_Label `good first issue` added by @kbknapp on 2018-07-22 02:31_

---

_Comment by @kbknapp on 2018-07-23 18:38_

Implemented on v3-master

---

_Closed by @kbknapp on 2018-07-23 18:38_

---

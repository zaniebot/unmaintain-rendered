```yaml
number: 2835
title: Deprecate Macro API
type: issue
state: closed
author: epage
labels: []
assignees: []
created_at: 2021-10-09T13:43:27Z
updated_at: 2023-02-11T03:03:19Z
url: https://github.com/clap-rs/clap/issues/2835
synced_at: 2026-01-12T16:14:13Z
```

# Deprecate Macro API

---

_@epage_

The Macro API was meant to be a performant, ergonomic version of the builder API.  However, there is a level of technical load in keeping it around and sufficiently documented and seems to mostly go unused.

Let's deprecate it for 3.0 and either spin it off into a standalone crate or remote it completely in a followup version.

---

_Label `C: builder macros` added by @epage on 2021-10-09 13:43_

---

_Added to milestone `3.0` by @epage on 2021-10-09 13:43_

---

_Referenced in [clap-rs/clap#2840](../../clap-rs/clap/issues/2840.md) on 2021-10-09 19:25_

---

_Referenced in [clap-rs/clap#2848](../../clap-rs/clap/pulls/2848.md) on 2021-10-11 19:42_

---

_Closed by @bors[bot] on 2021-10-12 08:52_

---

_Comment by @epage on 2021-12-08 17:06_

If users decide to fork clap_app and continue its development, some relevant commits
- 0d1f34b9

---

_Comment by @UltiRequiem on 2022-04-05 16:07_

> keeping it around and sufficiently documented and seems to mostly go unused.

I really like it and I was using it in most of my projects 😭 

---

_Comment by @epage on 2022-04-05 16:17_

I did say "mostly" :)

You are welcome to split it out into a separate crate!

---

_Comment by @UltiRequiem on 2022-04-05 16:20_

> I did say "mostly" :)
> 
> You are welcome to split it out into a separate crate!

I'm updating them to use the Derivate API, which is cool too.

Thanks for the hard work!

---

_Referenced in [kcl-lang/kcl#333](../../kcl-lang/kcl/issues/333.md) on 2022-12-12 03:27_

---

_Comment by @bergkvist on 2023-02-11 03:03_

I miss the `clap_app!` macro as well

---

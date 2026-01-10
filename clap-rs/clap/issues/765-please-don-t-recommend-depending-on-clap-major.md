---
number: 765
title: "Please don't recommend depending on clap=\"~major.minor.patch\""
type: issue
state: closed
author: joshtriplett
labels:
  - C-enhancement
  - A-docs
assignees: []
created_at: 2016-12-05T04:41:05Z
updated_at: 2018-08-02T03:29:57Z
url: https://github.com/clap-rs/clap/issues/765
synced_at: 2026-01-10T01:26:35Z
---

# Please don't recommend depending on clap="~major.minor.patch"

---

_Issue opened by @joshtriplett on 2016-12-05 04:41_

Clap's README.md recommends depending on `clap="~major.minor.patch"`, "to keep from being suprised of breaking changes".  However, any actual breakage that would prevent compiling code written against an old version of clap with a new version of clap would involve a *major* version bump, which a "compatible version" dependency like `clap="major.minor.patch"` would also prevent.  The only thing a "compatible version" dependency would allow would be a requirement on a new version of Rust or Cargo, which not every application needs to worry about.

Meanwhile, dependencies like "~major.minor.patch" mean that Linux distributions would have to package many different major.minor versions of clap to support various application dependencies on them.  Linux distributions will already avoid packaging a version of clap that requires a version of Rust or Cargo newer than the one they ship.

Based on this, please consider changing the recommendation in README.md to suggest using a "compatible version" dependency *unless* your application specifically needs to maintain support for versions of Rust older than Clap's compatibility policy (two versions older than stable).  This would significantly reduce the likelihood of having to package numerous versions of clap for Linux distributions.

---

_Comment by @kbknapp on 2016-12-05 04:59_

I'll look at re-wording the readme. The primary point I want to get across is if someone is depending on on a pinned version of Rust, and clap's support moves past that pinned version. This will only happen in a minor version bump of clap, *at a minimum*. 

Perhaps I could say use, `~major.minor.patch` *only* if one is concerned about the minimum version of Rust sticking at a point lower than the compatibility policiy states (i.e. less than 2 stable releases back).

---

_Label `C: docs` added by @kbknapp on 2016-12-05 05:00_

---

_Label `D: easy` added by @kbknapp on 2016-12-05 05:00_

---

_Label `T: enhancement` added by @kbknapp on 2016-12-05 05:00_

---

_Comment by @joshtriplett on 2016-12-05 07:03_

@kbknapp
> Perhaps I could say use, `~major.minor.patch` *only* if one is concerned about the minimum version of Rust sticking at a point lower than the compatibility policiy states (i.e. less than 2 stable releases back).

That's exactly what I'm requesting; thank you.

---

_Referenced in [BurntSushi/ripgrep#271](../../BurntSushi/ripgrep/issues/271.md) on 2016-12-06 18:21_

---

_Referenced in [clap-rs/clap#767](../../clap-rs/clap/pulls/767.md) on 2016-12-07 00:53_

---

_Closed by @homu on 2016-12-08 21:02_

---

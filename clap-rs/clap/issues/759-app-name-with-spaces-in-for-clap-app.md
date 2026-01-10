---
number: 759
title: "App name with spaces in for clap_app!"
type: issue
state: closed
author: alexreg
labels: []
assignees: []
created_at: 2016-11-28T02:42:35Z
updated_at: 2018-08-02T03:29:57Z
url: https://github.com/clap-rs/clap/issues/759
synced_at: 2026-01-10T01:26:35Z
---

# App name with spaces in for clap_app!

---

_Issue opened by @alexreg on 2016-11-28 02:42_

Currently the `clap_app!` macro doesn't accept an app name with spaces in it. It might be better to take a string expression before the `=>` token.

Also, could we get `crate_name!` and `crate_description!` macros?

---

_Comment by @kbknapp on 2016-11-28 16:36_

It's been quite a while since I've dug around in the `clap_app!` macro 😜 Let me see if this can be done in a non-breaking fashion and I'll post back here.

---

_Label `C: macros` added by @kbknapp on 2016-11-28 16:37_

---

_Label `T: RFC / question` added by @kbknapp on 2016-11-28 16:37_

---

_Comment by @alexreg on 2016-11-28 17:45_

Okay! :)

Sent from my iPhone

> On 28 Nov 2016, at 16:36, Kevin K. <notifications@github.com> wrote:
> 
> It's been quite a while since I've dug around in the clap_app! macro 😜 Let me see if this can be done in a non-breaking fashion and I'll post back here.
> 
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub, or mute the thread.
> 


---

_Referenced in [clap-rs/clap#731](../../clap-rs/clap/pulls/731.md) on 2016-12-02 02:12_

---

_Added to milestone `2.20.0` by @kbknapp on 2016-12-02 03:35_

---

_Closed by @homu on 2016-12-18 21:25_

---

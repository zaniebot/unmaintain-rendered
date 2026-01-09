---
number: 5422
title: Write-up of opt-in deprecation warnings?
type: issue
state: closed
author: svix-jplatte
labels: []
assignees: []
created_at: 2024-03-25T11:06:13Z
updated_at: 2024-03-25T13:34:13Z
url: https://github.com/clap-rs/clap/issues/5422
synced_at: 2026-01-07T13:12:20-06:00
---

# Write-up of opt-in deprecation warnings?

---

_Issue opened by @svix-jplatte on 2024-03-25 11:06_

Hi, I just went through https://github.com/clap-rs/clap/issues/3822 and other related GitHub / reddit threads to find a summary of the opt-in deprecation scheme that clap ended up with. Is my memory serving me wrong that such a write-up exists?

---

_Comment by @epage on 2024-03-25 11:18_

Are you looking for justification for it, an explanation of the mechanics / pattern, or user facing "how to opt-in"?

---

_Comment by @svix-jplatte on 2024-03-25 12:15_

Mostly an explanation of the pattern (I think there's not much to it, simple cfg-gated deprecation attributes?), probably with the motivation as an introduction. I thought I saw something like that before 😅

Context: wanted to link to it as prior art when mentioning the pattern for another project. 

---

_Comment by @epage on 2024-03-25 13:29_

#3830 is the main place I can think of which is linked to in the above.  That included updates to our [compatibility documentation](https://github.com/clap-rs/clap/blob/master/CONTRIBUTING.md#compatibility-expectations) and shows how we did this.  One of the key things that requires extra work is when proc-macros need to call deprecated functionality.

---

_Comment by @svix-jplatte on 2024-03-25 13:33_

Okay, so I guess I was imagining that there was a separate document / article about this pattern. Thanks :smile: 

---

_Closed by @svix-jplatte on 2024-03-25 13:33_

---

```yaml
number: 252
title: Possible enhancement to builder macro
type: issue
state: closed
author: WildCryptoFox
labels:
  - C-enhancement
  - M-breaking-change
  - S-waiting-on-decision
  - A-parsing
assignees: []
created_at: 2015-09-11T18:05:35Z
updated_at: 2017-01-30T05:14:20Z
url: https://github.com/clap-rs/clap/issues/252
synced_at: 2026-01-12T16:14:08Z
```

# Possible enhancement to builder macro

---

_@WildCryptoFox_

Hello again. I've been playing with macros for building again!

I decided to experiment with another format, just wondering what you guys think?

https://gist.github.com/james-darkfox/d61cd11be6587ed1f688


---

_Comment by @WildCryptoFox on 2015-09-12 04:00_

r? @kbknapp 

---

@yo-bot dead?


---

_Assigned to @kbknapp by @WildCryptoFox on 2015-09-12 04:02_

---

_Label `T: enhancement` added by @WildCryptoFox on 2015-09-12 04:03_

---

_Label `T: RFC / question` added by @WildCryptoFox on 2015-09-12 04:03_

---

_Label `E: breaking change` added by @WildCryptoFox on 2015-09-12 04:03_

---

_Label `W: maybe` added by @WildCryptoFox on 2015-09-12 04:03_

---

_Label `D: intermediate` added by @WildCryptoFox on 2015-09-12 04:03_

---

_Label `C: parsing` added by @WildCryptoFox on 2015-09-12 04:03_

---

_Label `R: needs review` added by @WildCryptoFox on 2015-09-12 04:03_

---

_Comment by @kbknapp on 2015-09-13 22:51_

That's an interesting way! I'd probably have a see a more `clap` specific version though to see what the true readability vs ergonomics of that would be.

**EDIT**: Also, apologies for taking a while to respond :wink:


---

_Comment by @WildCryptoFox on 2015-09-18 10:55_

@kbknapp I've been working on my other builder macro today. Overall much cleaner and easier to read, extend, or otherwise modify. https://github.com/trustless-fox/icy-crates/blob/master/src/macros.rs

I will be porting over ideas to clap shortly.


---

_Comment by @kbknapp on 2015-09-20 23:09_

Nice, I'm looking forward to seeing it :+1: 


---

_Comment by @WildCryptoFox on 2015-09-22 07:30_

@yo-bot r? @james-darkfox


---

_Unassigned @kbknapp by @WildCryptoFox on 2015-09-22 07:31_

---

_Assigned to @WildCryptoFox by @WildCryptoFox on 2015-09-22 07:31_

---

_Referenced in [clap-rs/clap#402](../../clap-rs/clap/issues/402.md) on 2016-01-30 11:05_

---

_Closed by @kbknapp on 2017-01-30 05:14_

---

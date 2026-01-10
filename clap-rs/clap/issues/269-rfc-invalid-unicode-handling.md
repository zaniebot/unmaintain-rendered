---
number: 269
title: "RFC: Invalid Unicode Handling"
type: issue
state: closed
author: kbknapp
labels:
  - A-parsing
assignees: []
created_at: 2015-09-22T17:49:36Z
updated_at: 2018-08-02T03:29:45Z
url: https://github.com/clap-rs/clap/issues/269
synced_at: 2026-01-10T01:26:26Z
---

# RFC: Invalid Unicode Handling

---

_Issue opened by @kbknapp on 2015-09-22 17:49_

This issue is to discuss how invalid unicode should be handled in 2.x **and** 1.x
### Currently
- Consumers have the option to `panic!` on invalid unicode (the default)
- Consumers can get an `Err` on invalid unicode using the `*_safe()` methods
- Consumers can get a lossy value where invalid unicode is replaced with `U+FFFD` using `*_lossy()` methods
#### Problems
- Users **cannot** get values with invalid unicode (which is allowed on Unix systems for file names, paths, etc.)
- All the `lossy`, `safe`, and `regular` version of `get_matches` is somewhat messy. Granted in practice it's not an issue because you only use one version of that method...but for API space it looks a mess.
### Future

One idea is to store the values internally in the `ArgMatches` struct as an `OsString` and by default give a `&str`, but allows the users the option to get an `OsStr` instead. This is the **opt-in** to invalid unicode. I am a firm believer the default should be strict unicode, but we **should** allows users to handle invalid unicode if they so choose.
#### Questions

How does this affect Windows? Or does it even affect Windows? If yes, should we use a `#[cfg(not(windows))]`, or similar?

---

Question, comments, suggestions?


---

_Label `T: enhancement` added by @kbknapp on 2015-09-22 17:50_

---

_Label `D: intermediate` added by @kbknapp on 2015-09-22 17:50_

---

_Label `P3: want to have` added by @kbknapp on 2015-09-22 17:50_

---

_Label `C: matches` added by @kbknapp on 2015-09-22 17:50_

---

_Label `C: parsing` added by @kbknapp on 2015-09-22 17:50_

---

_Label `W: 2.x` added by @kbknapp on 2015-09-22 17:50_

---

_Label `T: RFC / question` added by @kbknapp on 2015-09-22 17:50_

---

_Label `W: 1.x` added by @kbknapp on 2015-09-22 17:50_

---

_Label `T: enhancement` removed by @kbknapp on 2015-09-22 17:50_

---

_Renamed from "Tracking Issue: Invalid Unicode Handling" to "RFC: Invalid Unicode Handling" by @kbknapp on 2015-09-22 17:51_

---

_Referenced in [clap-rs/clap#262](../../clap-rs/clap/issues/262.md) on 2015-09-22 17:52_

---

_Comment by @sru on 2015-09-22 18:15_

:+1:


---

_Added to milestone `1.5` by @kbknapp on 2015-10-28 14:00_

---

_Added to milestone `1.6.0` by @kbknapp on 2015-12-18 08:55_

---

_Removed from milestone `1.5` by @kbknapp on 2015-12-18 08:55_

---

_Comment by @kbknapp on 2016-01-27 20:32_

Closed with 2x


---

_Closed by @kbknapp on 2016-01-27 20:32_

---

_Comment by @remram44 on 2016-02-08 17:20_

This is awesome! The ability to accept any filename is great, and the API is cool, and I love you all.


---

_Referenced in [clap-rs/clap#2623](../../clap-rs/clap/pulls/2623.md) on 2021-08-02 08:54_

---

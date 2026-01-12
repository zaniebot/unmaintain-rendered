```yaml
number: 231
title: Allow defining hidden args and subcommands
type: issue
state: closed
author: kbknapp
labels:
  - A-help
assignees: []
created_at: 2015-09-06T18:29:30Z
updated_at: 2018-08-02T03:29:44Z
url: https://github.com/clap-rs/clap/issues/231
synced_at: 2026-01-12T16:14:08Z
```

# Allow defining hidden args and subcommands

---

_@kbknapp_

Based off an idea in #217 

Hidden args and subcommands would not appear in the help messages or suggestions on errror.

They _would_ still appear in usage strings on error.

I'm unsure about allowing them to be seen in groups or not. I'm leaning towards not displaying them in groups....but open to opinions.


---

_Label `T: new feature` added by @kbknapp on 2015-09-06 18:29_

---

_Label `P4: nice to have` added by @kbknapp on 2015-09-06 18:29_

---

_Label `C: args` added by @kbknapp on 2015-09-06 18:29_

---

_Label `C: help message` added by @kbknapp on 2015-09-06 18:29_

---

_Label `C: subcommands` added by @kbknapp on 2015-09-06 18:29_

---

_Label `D: easy` added by @kbknapp on 2015-09-06 18:29_

---

_Comment by @WildCryptoFox on 2015-09-07 04:32_

If a verbose flag is present; expand the help to all.


---

_Comment by @kbknapp on 2015-09-07 20:04_

I think that's a good addition. We could even make that an `AppSettings` variant (`UnhideIfVerbose`). I think the initial implementation will just have hidden args/subcommands. Then we can add in the unhiding with verbose, because there's also a few oddities about that one which will make it more difficult to implement, but doing so is non-breaking ;)


---

_Comment by @WildCryptoFox on 2015-09-08 10:05_

I'm not too sure I know why we have the enum AppSettings? Was this created for parsing purposes only? Don't get me wrong, it makes my macro cleaner because CamelCase is better. :-)


---

_Comment by @kbknapp on 2015-09-08 22:42_

> I'm not too sure I know why we have the enum AppSettings? Was this created for parsing purposes only? Don't get me wrong, it makes my macro cleaner because CamelCase is better. :-)

The alternative is what we started with, tons of random methods which take a `bool` value. This somewhat pollutes the public API space, and makes finding "settings" more difficult. Using an enum creates a single place to look for "settings" which apply to an entire application, or subcommand. It also keeps the public API a little more clean (or at least will in 2.x once all the old methods are removed).

Also, I can't find the link but there is a Rust style guide which favors `setting(SettingsEnum)` over `some_setting(bool)`.


---

_Added to milestone `1.4` by @kbknapp on 2015-09-08 22:48_

---

_Comment by @kbknapp on 2015-09-08 22:50_

The base functionality (i.e. without `--verbose` unhiding) should be a quick add, therefore I'm throwing this into the 1.4 milestone blocker


---

_Closed by @kbknapp on 2015-09-09 02:57_

---

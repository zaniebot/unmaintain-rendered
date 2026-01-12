```yaml
number: 585
title: Please support combining flags and options into one list
type: issue
state: closed
author: joshtriplett
labels: []
assignees: []
created_at: 2016-07-18T22:09:15Z
updated_at: 2018-08-02T03:29:51Z
url: https://github.com/clap-rs/clap/issues/585
synced_at: 2026-01-12T16:14:09Z
```

# Please support combining flags and options into one list

---

_@joshtriplett_

I don't want to make the distinction between "flags" (that don't take a value) and "options" (that do take a value); I'd like to just list all of them in one sorted list.  Would you consider providing a setting to do so?  (Would you consider making it the default?)

This should merge the "[FLAGS] [OPTIONS]" in the usage string into just "[OPTIONS]", and similarly merge the two sections in the help string.


---

_Comment by @kbknapp on 2016-07-23 19:35_

@joshtriplett This already exists with [`AppSettings::UnifiedHelpMessage`](http://kbknapp.github.io/clap-rs/clap/enum.AppSettings.html#variant.UnifiedHelpMessage)

If this isn't want you're talking about let me know :wink:


---

_Label `T: RFC / question` added by @kbknapp on 2016-07-23 19:36_

---

_Comment by @joshtriplett on 2016-07-23 22:04_

Thanks, that did exactly what I wanted.  I needed to read https://kbknapp.github.io/clap-rs/clap/enum.AppSettings.html more carefully.


---

_Closed by @joshtriplett on 2016-07-23 22:04_

---

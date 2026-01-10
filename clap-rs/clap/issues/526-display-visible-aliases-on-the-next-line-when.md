---
number: 526
title: "Display visible aliases on the next line when they'd significantly widen display"
type: issue
state: closed
author: joshtriplett
labels:
  - A-help
assignees: []
created_at: 2016-06-10T16:21:57Z
updated_at: 2018-08-02T03:29:50Z
url: https://github.com/clap-rs/clap/issues/526
synced_at: 2026-01-10T01:26:30Z
---

# Display visible aliases on the next line when they'd significantly widen display

---

_Issue opened by @joshtriplett on 2016-06-10 16:21_

Visible aliases for subcommands (from #522) can significantly widen the help display, and move subcommand descriptions far away from the subcommand name, making it harder to pair them up.  For example:

```
    log                          Show the history of the patch series
    pull-request|request-pull    Generate a mail requesting a pull of the patch series
```

Please consider an alternate format that doesn't widen the display like this.  For instance, indent a bit and put the aliases underneath the main subcommand name:

```
    log             Show the history of the patch series
    pull-request    Generate a mail requesting a pull of the patch series
      Alias: request-pull
```

Or with more than one alias:

```
    log             Show the history of the patch series
    pull-request    Generate a mail requesting a pull of the patch series
      Aliases: req, request-pull
```


---

_Comment by @kbknapp on 2016-06-10 22:59_

Very true. What about formatting them as such:

```
    log             Show the history of the patch series
    pull-request    Generate a mail requesting a pull of the patch series [aliases: req, request-pull]
```

To my eye, the newline and extra indent looks off...but that's also subjective :stuck_out_tongue_winking_eye: 

The other option I thought about was:

```
    log             Show the history of the patch series
    pull-request    Generate a mail requesting a pull of the patch series
    req             Alias for 'pull-request'
    request-pull    Alias for 'pull-request'
```


---

_Label `T: RFC / question` added by @kbknapp on 2016-06-10 22:59_

---

_Label `C: help message` added by @kbknapp on 2016-06-10 22:59_

---

_Label `C: subcommands` added by @kbknapp on 2016-06-10 22:59_

---

_Label `D: intermediate` added by @kbknapp on 2016-06-10 22:59_

---

_Label `W: 2.x` added by @kbknapp on 2016-06-10 22:59_

---

_Comment by @joshtriplett on 2016-06-10 23:06_

@kbknapp
Either of those looks good to me.


---

_Referenced in [clap-rs/clap#529](../../clap-rs/clap/issues/529.md) on 2016-06-13 01:29_

---

_Comment by @kbknapp on 2016-06-13 02:12_

This is fixed with #530 

Edit: I went with the `[aliases: one, two, three]` method as it was the least refactoring and still looks good to my eye.


---

_Added to milestone `2.6.0` by @kbknapp on 2016-06-13 02:13_

---

_Closed by @homu on 2016-06-13 04:39_

---

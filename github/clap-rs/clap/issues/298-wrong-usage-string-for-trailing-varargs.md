---
number: 298
title: Wrong usage string for trailing varargs
type: issue
state: closed
author: matklad
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2015-10-01T16:02:10Z
updated_at: 2018-08-02T03:29:45Z
url: https://github.com/clap-rs/clap/issues/298
synced_at: 2026-01-07T13:12:19-06:00
---

# Wrong usage string for trailing varargs

---

_Issue opened by @matklad on 2015-10-01 16:02_

For the following code

``` rust
        .args_from_usage(
            "[daemon] -d   'Daemonizes the container'
             --cpu [PREC]  'set CPU usage limit'
             <IMAGE_PATH>  'path to image of container file system'
             <CMD>         'command to run inside the container'
             [ARGS]...     'arguments to pass to the CMD'")
```

I get this usage string: `aucont_start [FLAGS] [OPTIONS] [ARGS] <IMAGE_PATH> <CMD>`.
`[ARGS]` should definitely come after IMAGE_PATH and CMD in the usage string. The parsing itself works correctly. 


---

_Comment by @kbknapp on 2015-10-01 16:16_

Thanks for taking the time to file this! I can confirm, on 1.4.3 this is the case. I believe this may be a bug in the usage generation portion because in cases like the above it should be:

```
USAGE:
    aucont_start [FLAGS] [OPTIONS] <IMAGE_PATH> <CMD> [--] [ARGS]
```

Note, you _can_ also override the usage string to make it anything you like, but doing so manes you no longer get context aware usage strings generated on error messages. This doesn't appear to be  a good case for that, but just FYI.

We'll report back here once this bug is fixed :+1: 


---

_Label `T: bug` added by @kbknapp on 2015-10-01 16:16_

---

_Label `P2: need to have` added by @kbknapp on 2015-10-01 16:16_

---

_Label `C: help message` added by @kbknapp on 2015-10-01 16:16_

---

_Label `D: easy` added by @kbknapp on 2015-10-01 16:16_

---

_Label `W: 1.x` added by @kbknapp on 2015-10-01 16:16_

---

_Added to milestone `1.4.4` by @kbknapp on 2015-10-01 16:16_

---

_Comment by @kbknapp on 2015-10-01 16:28_

Also, this is somewhat muddled by the use of `ARGS` for the argument name, and `clap`s use of `[ARGS]` in regular usage strings just to denote "optional positional arguments." It's not a big deal, but just something to be aware of.


---

_Referenced in [clap-rs/clap#299](../../clap-rs/clap/pulls/299.md) on 2015-10-01 16:36_

---

_Comment by @matklad on 2015-10-01 16:45_

Thanks for the swift reply! I'd like to add that implementing #278 would be ideal for my use case :)


---

_Comment by @kbknapp on 2015-10-01 16:49_

That's actually what I'm working on now, and should be implemented shortly :)


---

_Closed by @homu on 2015-10-01 17:37_

---

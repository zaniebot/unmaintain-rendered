```yaml
number: 428
title: Wrap help lines at term width
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
  - A-help
assignees: []
created_at: 2016-02-17T11:39:11Z
updated_at: 2018-08-02T03:29:48Z
url: https://github.com/clap-rs/clap/issues/428
synced_at: 2026-01-12T16:14:09Z
```

# Wrap help lines at term width

---

_@kbknapp_

_No description provided._

---

_Label `T: enhancement` added by @kbknapp on 2016-02-17 11:39_

---

_Label `P4: nice to have` added by @kbknapp on 2016-02-17 11:39_

---

_Label `C: help message` added by @kbknapp on 2016-02-17 11:39_

---

_Label `D: intermediate` added by @kbknapp on 2016-02-17 11:39_

---

_Label `W: 2.x` added by @kbknapp on 2016-02-17 11:39_

---

_Comment by @mitsuhiko on 2016-02-23 12:30_

I'm very interested in this.  This is currently basically impossible to emulate manually and it makes long help texts very hard to read because when the term wraps, it does not align with the start of the message.

As an example from an app i have:

```
        --use-dsymutil         Invoke dsymutil on encountered macho binaries to extract
the symbols before uploading.  This requires the dsymutil binary to be available.
```


---

_Comment by @kbknapp on 2016-02-24 12:27_

I have a working local stash of this feature. The downside is that's a naive implementation and doesn't wrap by word, i.e. some words get cut in the middle. I'm going to continue playing with this as I have some ideas to improve it and at least get it to wrap where it's aesthetically pleasing.

@mitsuhiko your example above will end up outputting:

```
        --use-dsymutil         Invoke dsymutil on encountered macho binaries to extract
                               the symbols before uploading.  This requires the dsymutil 
                               binary to be available.
```


---

_Comment by @kbknapp on 2016-03-08 12:50_

Just an update, my free time has been almost non-existent, but I'm hoping to have this complete over the next few days. More updates to follow...


---

_Comment by @kbknapp on 2016-03-13 22:38_

Finally, after some back to the drawing board moments I have this working in a whitespace respecting way (i.e. it doesn't cut words in the middle). I'm updating tests now, and should have a PR in tonight. Once merged I'll go ahead and put out 2.2.0.


---

_Comment by @kbknapp on 2016-03-14 01:56_

PR is in, here's a screenshot of what the term wrapping looks like using some arbitrary test options and flags. Term width is 66:

![screenshot](http://i.imgur.com/PAJzJJG.png)

Edit:
There's also the option to place the text on the line following the option, flag, or argument indented once (a la, some getopts/docopt style formatting for long help strings). This could also be applied [per argument](http://kbknapp.github.io/clap-rs/clap/struct.Arg.html#method.next_line_help), or [application/command wide](http://kbknapp.github.io/clap-rs/clap/enum.AppSettings.html#examples-15).


---

_Comment by @sru on 2016-03-14 03:11_

An aside, what font is that?


---

_Comment by @kbknapp on 2016-03-14 11:31_

Inconsolata, I love it.


---

_Closed by @homu on 2016-03-15 12:11_

---

_Referenced in [clap-rs/clap#1469](../../clap-rs/clap/issues/1469.md) on 2019-05-13 16:03_

---

```yaml
number: 189
title: "Consider showing less output for `rg -h`"
type: issue
state: closed
author: djc
labels:
  - question
assignees: []
created_at: 2016-10-19T11:34:12Z
updated_at: 2020-05-04T00:24:38Z
url: https://github.com/BurntSushi/ripgrep/issues/189
synced_at: 2026-01-12T16:13:21Z
```

# Consider showing less output for `rg -h`

---

_@djc_

This might be a bit subjective, but for me the result of `rg -h` was somewhat suboptimal in giving me much more output than expected, thus quickly scrolling the most useful stuff off the screen.

I think I would like it better if `rg -h` did not show the `Less common options` and `File management options` by default, instead showing a line at the bottom that refers to some extra switch to make `-h/--help` more verbose.


---

_Comment by @BurntSushi on 2016-10-19 12:30_

FWIW, `rg --help | less` should help, and it's what I normally do when the output of `--help` is large.

I'm not opposed to doing this, but I'm not 100% sure we should because I'm not a fan of making flags harder to discover.


---

_Label `question` added by @BurntSushi on 2016-10-19 12:30_

---

_Comment by @lilyball on 2016-10-24 22:29_

`ag --help` shows a one-line (or sometimes two-line) description of all flags. Maybe `rg --help` should do the same, and then something like `rg --help-options` to show the more verbose output? That would make it easier to skim the options to find what you want.


---

_Comment by @djc on 2016-10-25 07:05_

Yeah, that's another option that would maybe help discovery without making flags harder to discover.


---

_Comment by @BurntSushi on 2016-10-25 18:45_

I do like that idea better.

On Oct 24, 2016 18:29, "Kevin Ballard" notifications@github.com wrote:

> ag --help shows a one-line (or sometimes two-line) description of all
> flags. Maybe rg --help should do the same, and then something like rg
> --help-options to show the more verbose output? That would make it easier
> to skim the options to find what you want.
> 
> —
> You are receiving this because you commented.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/189#issuecomment-255884632,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34sXeJ6zOhiqgotAM55IPJy_rcWJiks5q3TFZgaJpZM4Ka3MS
> .


---

_Closed by @BurntSushi on 2016-11-18 01:22_

---

_Comment by @devsnek on 2020-05-03 22:07_

I'm not sure if something has happened since this, but the help output is once again enormous, and I usually end up doing `rg --help | rg "thing i'm looking for"`.

---

_Comment by @BurntSushi on 2020-05-03 22:28_

`rg --help` has been basically "the man page" for years. Nothing has changed recently. If you want a denser view, then use `rg -h`. 

---

_Comment by @devsnek on 2020-05-03 22:31_

Oh interesting. I'm not used to `-h` and `--help` doing different things. Perhaps they should be merged and manpages can be generated separately?

---

_Comment by @BurntSushi on 2020-05-03 22:36_

No. man pages aren't available everywhere. The behavior is staying as is.

---

_Comment by @lilyball on 2020-05-04 00:18_

`rg -h` still has a large header block that looks identical to the one from `rg --help`. Perhaps we could slim down the `rg -h` header block for just a couple of lines? It seems odd that even the USAGE section isn't immediately visible in `rg -h | less` on a 80x24 terminal.

---

_Comment by @BurntSushi on 2020-05-04 00:24_

Indeed, I'd be open to that. The initial header block has indeed slowly grown larger over the years.

---

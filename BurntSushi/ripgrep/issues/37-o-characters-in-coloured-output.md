```yaml
number: 37
title: ^O characters in coloured output
type: issue
state: closed
author: uazu
labels:
  - bug
assignees: []
created_at: 2016-09-23T22:43:43Z
updated_at: 2016-11-22T09:36:10Z
url: https://github.com/BurntSushi/ripgrep/issues/37
synced_at: 2026-01-12T18:23:11Z
```

# ^O characters in coloured output

---

_@uazu_

Viewing with `--color always` piped to `less -R`, colours appear correctly but there are ^O characters (\x0F) inserted regularly, which appear in less as inverted `^O`. When removed with `sed 's/\x0f//g'` output appears like screenshot.

Tested release binary version 0.1.16 on Debian Linux, with TERM=screen.linux.


---

_Label `bug` added by @BurntSushi on 2016-09-24 02:04_

---

_Comment by @BurntSushi on 2016-09-24 02:04_

Yup, I can reproduce it. Setting `TERM=screen.linux` was key.


---

_Comment by @BurntSushi on 2016-09-27 01:08_

N.B. When I'm in a screen session, if I do `TERM=xterm rg -p foo | less -R` then things appear to work. When `TERM=screen`, I see the same `^0` characters as the OP. I wonder if it's a bug in the `term` library I'm using.


---

_Comment by @balta2ar on 2016-09-27 09:02_

Same here (ArchLinux, tmux 2.2, rg 0.2.1). In tmux my $TERM is `screen-256color`, but running `TERM=xterm rg temp | less` helps.


---

_Comment by @uazu on 2016-09-27 15:00_

Yes, I can work around it. ^O is shift-in, i.e. switch to non-graphic character set, so may be part of a generic "reset" sequence. As I need to add `--color always` to use with less, I need to put a wrapper script around it anyway so I'll add TERM=whatever there. Perhaps it only needs documenting. However if there was a way to just reset colours without also resetting character set that might avoid confusion.


---

_Comment by @BurntSushi on 2016-09-27 15:32_

@uazu Ah thanks for the explanation. I think other tools seem to work fine, so we should try to fix it. Documenting it as a known bug in the README is also a good idea. By the way, you may also be interested in the `-p/--pretty` flag. :-)


---

_Comment by @Screwtapello on 2016-10-08 03:56_

According to `less(1)`, the `-R` option tells `less` to assume all sequences of the form `ESC [ ... m` are zero-width, and pass them through verbatim. `^O` is not of that form, so less makes it printable. But where does the `^O` come from?

Looking at the terminfo database entry for `screen`, we see:

```
$ infocmp -1 screen | grep sgr0
    sgr0=\E[m\017,
```

...so whenever ripgrep calls `.reset()`, the term crate looks up the sequence associated with `sgr0` ("set graphical rendition to 0" i.e. all fancy things turned off), the terminfo database for `screen` defines `sgr0` to be "ESC [ m ^O" and so that's what gets written to the screen.

If there's a bug here, I'd say it's in `less` for half-assing its escape-sequence passthrough.


---

_Comment by @uazu on 2016-10-09 11:45_

Well, I now have a wrapper around ripgrep which sets TERM=xterm and pipes to `less` and everything is fine (and I use ripgrep all the time now -- Thanks!). However, maybe using the 'reset' (sgr0) sequence in this application is overkill, since you don't need to reset everything, but rather just the colours. If there is some other terminfo string that can be used for that then the problem is avoided. Otherwise the TERM=xterm workaround needs to be documented, since I'm sure some other people will want to use `ripgrep` with `less` on `screen`.


---

_Comment by @Screwtapello on 2016-10-09 12:49_

`sgr0` is definitely the canonical way to do it. And it's not just colours - ripgrep also makes some text bold.


---

_Closed by @BurntSushi on 2016-11-20 20:01_

---

_Comment by @kaushalmodi on 2016-11-22 09:36_

Thanks. I confirm the fix. 

---

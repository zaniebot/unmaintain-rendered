```yaml
number: 255
title: ripgrep since 0.3.0 produces terrible output in non-color xterms
type: issue
state: closed
author: siebenmann
labels:
  - question
  - wontfix
assignees: []
created_at: 2016-11-26T02:46:37Z
updated_at: 2016-11-27T00:22:31Z
url: https://github.com/BurntSushi/ripgrep/issues/255
synced_at: 2026-01-12T18:23:11Z
```

# ripgrep since 0.3.0 produces terrible output in non-color xterms

---

_@siebenmann_

If you used old versions of ripgrep (0.3.0) in an xterm that had color disabled, they simply bolded some portions of the output (it looks like file names, line numbers, and matching text). Versions from 0.3.0 onwards do truly unreadable things in this environment. Everything that used to be bolded now *blinks*, and matching text has a ~~strike~~ through it. Given the blinking, the result is not in the least bit usable.

Can the old behavior be restored on non-color xterms? Better yet, can we also get an option that always does this (even when color is available)? Even in environments with colors I would often prefer not to use them and would rather have things just come out as bold.

(In theory color can make things more readable. In practice automatically chosen highlight colors don't always go with certain choices of foreground and background colors. I use black text on white background, which clashes with most color schemes; they often seem designed for white on black.)

---

_Comment by @BurntSushi on 2016-11-26 02:59_

Does `--color never` not work for you?

I don't really get why you are seeing blinking or strike through text. The intended behavior is to use ANSI escape sequences to color certain aspects of the output. If this is doing something else, then I'd like to fix it, but as it stands i don't understand your environment enough to reproduce the issue. Could you elaborate more?

I would like to further avoid discussion about default colors in this issue. If you want to open a separate issue then that's okay. The short story is that you should be able to change colors/styles using the `--colors` flag. See `rg --help` for details.

---

_Comment by @BurntSushi on 2016-11-26 03:00_

By the way, I use black on white and I use the default color scheme.

---

_Comment by @siebenmann on 2016-11-26 03:07_

`-color=never` works, but it suppresses the bolding as well; everything comes out plain, and I like(d) the bolding. I believe the equivalent of my xterm X resources settings can be gotten through the command line with `xterm -cm` (and I would be happy to send you my full set of xterm resource settings).

---

_Comment by @BurntSushi on 2016-11-26 03:09_

Have you looked at `--colors`?

---

_Comment by @siebenmann on 2016-11-26 03:24_

`--colors` works, in that sufficient application of it to 0.3.1 reverts things to the 0.2.9 behavior. What I appears to need is:

```
$ rg --colors path:none --colors line:none --colors match:none --colors path:style:bold --colors match:style:bold --colors line:style:bold <search term>
```

(If I don't clear the existing color settings first and just try to set everything bold, text still blinks and so on.)

---

_Comment by @BurntSushi on 2016-11-26 21:31_

I was able to reproduce this problem using `xterm -cm`. (The `-cm` option is necessary to reproduce the problem.)

I wasn't familiar with `xterm -cm`, but I just had a chance to look it up:

```
       -cm     This option disables recognition of ANSI color-change escape sequences.  It sets the
               colorMode resource to “false”.
```

I'm not sure what you're actually expecting to happen here. ripgrep doesn't know that you've disabled ANSI color escape sequences, and I don't know why `xterm` interprets them as strike-through/blinking text. Unless you can provide more insight here, this seems like a bug in `xterm`.

Given that you can actually fix this with sufficient application of the `--colors` flag (which could go in an alias or a `~/bin` script), I'm inclined to mark this as `wontfix` unless there's some further insight than can be provided here. For example, if `xterm` signaled somehow that colors should be disabled, then ripgrep could pick up on and disable them since the default setting of `--color` is `auto`. However, style settings like bold are lumped in with this decision procedure, and it turns out that you do actually want text to be bolded. Therefore, this auto-detection (if it existed) wouldn't actually work for your use case.

---

_Comment by @siebenmann on 2016-11-26 22:23_

I did some tests and things are interesting here. Rather than speculate, I will tell you what I've determined:

* Xterm will say whether colors are enabled or disabled as part of its response to an escape sequence that asks about device attributes; this is 'Send Device Attributes' request in [the control sequences documentation](http://invisible-island.net/xterm/ctlseqs/ctlseqs.html). However I don't think ripgrep 0.2.9 was (or is) using this.
* regardless of whether xterm is in color mode or not, ripgrep 0.2.9 sends a set of control sequences that render color on xterms with color enabled and bold (for all colors) on my xterms without it. I've tested this by using `script` to capture the raw output from ripgrep and using `cat` to display it in either type of xterm.
* again regardless of xterm type, ripgrep 0.3.1 sends a different set of control sequences that render as either color in color xterms or the blinking ~~strikethrough~~ in non-color ones.

If I'm reading the raw output correctly, the 0.2.9 control sequence for eg file names are CSI 32 m CSI 1 m. In the same situation, 0.3.1 sends CSI m CSI 3 8 ; 5 ; 1 0 m. The xterm page doesn't document the latter, but it appears to be [ANSI color codes](https://en.wikipedia.org/wiki/ANSI_escape_code#Colors) used to directly pick the high-intensity green. Ripgrep 0.3.1 appears to use high-intensity colors for all of its highlighting, and presumably a non-color xterm interprets these as 'they said high intensity so I'll make the text blink'.

(Using `rg --color=ansi` vs `rg --color=always` or `rg --color=auto` doesn't appear to make a difference in xterm as far as what ripgrep 0.3.1 generates as escape sequences; they always seem to come out as the ANSI sequences instead of 0.2.9's.)

---

_Comment by @BurntSushi on 2016-11-26 22:44_

> Using rg --color=ansi vs rg --color=always or rg --color=auto doesn't appear to make a difference 

I wouldn't expect it to. If `--color=auto` outputs ANSI escape sequences, then so will `--color=ansi` and `--color=always`.

> Ripgrep 0.3.1 appears to use high-intensity colors for all of its highlighting, and presumably a non-color xterm interprets these as 'they said high intensity so I'll make the text blink'.

Hmm, I'm not necessarily seeing the same thing. The intensity doesn't appear related to the blinking. For example, if you use `rg --colors 'match:style:nobold'`, then it will use a non-intense color, but the matched text still blinks. Interestingly, it becomes bold but not strike-through. If I change the color, e.g., `rg --colors 'match:style:nobold' --colors 'match:fg:cyan'`, then the bold goes away but the text still blinks.

It looks like `-cm` isn't "don't recognize ANSI escapes" but instead is "attempt to translate ANSI escapes."

> If I'm reading the raw output correctly, the 0.2.9 control sequence for eg file names are CSI 32 m CSI 1 m. In the same situation, 0.3.1 sends CSI m CSI 3 8 ; 5 ; 1 0 m.

A big part of ripgrep 0.3.0 was a [complete overhaul of how colors are handled](https://github.com/BurntSushi/ripgrep/pull/240). In particular, we switched away from an [error prone process of reading a `TERMINFO` database](https://github.com/BurntSushi/ripgrep/issues/37) to a simpler process that uses ANSI escape sequences only. Pretty much everything has some sane support for this. Apparently `xterm -cm` is an exception.

---

_Comment by @siebenmann on 2016-11-26 23:07_

> It looks like `-cm` isn't "don't recognize ANSI escapes" but instead is "attempt to translate ANSI escapes."

It does seem that way. I've tried to look at the xterm code in order to figure out how it behaves, but I can't follow the code well enough to do that; there is a lot of levels of magic involved in the handling of the `-cm` switch. So I can't tell if this is an xterm feature or a bug (and if it is a bug, it will be years before an updated version makes it everywhere).

---

_Comment by @BurntSushi on 2016-11-27 00:22_

All right, I think I'm going to close this for a couple reasons:

1. It seems like ripgrep <0.3 did the "right thing" with respect to this use case, but it's possible that doing the right thing requires reading the `TERMINFO` database for `xterm` specifically, and I really want to try and avoid bringing that complexity back.
2. There is a work-around for your use case using the `--colors` flag. It's pretty verbose, but it works, and the verboseness can be worked around by using an alias or a simple wrapper script in `$HOME/bin`.
3. I feel like your use case is very niche: "I use xterm and I disabled color ANSI escapes but still want to use bold ANSI escapes." Niche use cases aren't necessarily unimportant, but if niche use cases can be resolved with less than ideal paths (see 2 above), then I think I'm OK with that.

In any case, thanks so much for the report and the follow up info. If the underlying issue spreads to non-niche use cases, then perhaps we will be forced to fix it for reals. :-)

---

_Closed by @BurntSushi on 2016-11-27 00:22_

---

_Label `question` added by @BurntSushi on 2016-11-27 00:22_

---

_Label `wontfix` added by @BurntSushi on 2016-11-27 00:22_

---

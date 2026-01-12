```yaml
number: 240
title: Completely re-work colored output and tty handling.
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: color
created_at: 2016-11-20T06:45:23Z
updated_at: 2016-11-22T23:26:49Z
url: https://github.com/BurntSushi/ripgrep/pull/240
synced_at: 2026-01-12T18:23:12Z
```

# Completely re-work colored output and tty handling.

---

_@BurntSushi_

This commit completely guts all of the color handling code and replaces
most of it with two new crates: wincolor and termcolor. wincolor
provides a simple API to coloring using the Windows console and
termcolor provides a platform independent coloring API tuned for
multithreaded command line programs. This required a lot more
flexibility than what the `term` crate provided, so it was dropped.
We instead switch to writing ANSI escape sequences directly and ignore
the TERMINFO database.

In addition to fixing several bugs, this commit also permits end users
to customize colors to a certain extent. For example, this command will
set the match color to magenta and the line number background to yellow:

    rg --colors 'match:fg:magenta' --colors 'line:bg:yellow' foo

For tty handling, we've adopted a hack from `git` to do tty detection in
MSYS/mintty terminals. As a result, ripgrep should get both color
detection and piping correct on Windows regardless of which terminal you
use.

Finally, switch to line buffering. Performance doesn't seem to be
impacted and it's an otherwise more user friendly option.

Fixes #37, Fixes #51, Fixes #94, Fixes #117, Fixes #182, Fixes #231

---

_Merged by @BurntSushi on 2016-11-20 20:01_

---

_Closed by @BurntSushi on 2016-11-20 20:01_

---

_Branch deleted on 2016-11-20 20:01_

---

_Comment by @retep998 on 2016-11-22 15:47_

cc @Stebalien you may be very interested in the techniques used here to detect what sort of TTY is available on Windows.

---

_Comment by @BurntSushi on 2016-11-22 16:40_

#94 is where the goods are, including [some code](https://github.com/BurntSushi/ripgrep/issues/94#issuecomment-261761687). Arguably, that might belong in the `atty` crate. cc @softprops

---

_Comment by @BurntSushi on 2016-11-22 23:26_

I filed an issue against `atty`: https://github.com/softprops/atty/issues/10

How awesome would it be if the entire Rust ecosystem could automatically do correct tty detection on Windows? Oh boy!

---

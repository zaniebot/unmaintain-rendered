```yaml
number: 3140
title: rg man page miscodes interior hyphens in long option names as unicode hyphen
type: issue
state: closed
author: hmelman
labels:
  - bug
  - rollup
assignees: []
created_at: 2025-09-06T16:06:20Z
updated_at: 2025-09-20T01:08:47Z
url: https://github.com/BurntSushi/ripgrep/issues/3140
synced_at: 2026-01-12T16:13:25Z
```

# rg man page miscodes interior hyphens in long option names as unicode hyphen

---

_@hmelman_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.1

### How did you install ripgrep?

homebrew

### What operating system are you using ripgrep on?

macOS 15.6.1

### Describe your bug.

I believe rg's man page miscodes hyphens within option names.  Using Gnu man on mac the rg.1 man page shows switches as `--case‐sensitive` and `--files‐without‐match` where the leading hyphens are properly Ascii HYPHEN-MINUS #x2d but the interior ones are Unicode HYPHEN #x2010 which makes it difficult to search for the option name. (I think/hope this came through properly on GitHub).

### What are the steps to reproduce the behavior?

$ gman rg

### What is the actual behavior?

```
       --pre‐glob=GLOB
           This  flag works in conjunction with the --pre flag. Namely, when one or more --pre‐glob flags are given, then only files that match the given set of globs will be handed to the command specified
           by the --pre flag. Any non‐matching files will be searched without using the preprocessor command.
```

### What is the expected behavior?

hyphen in switch names should be ascii hyphen-minus as you would type them so they can be cut and pasted to a command line.

---

_Comment by @BurntSushi on 2025-09-06 17:10_

ripgrep does not do this. Something else is. It renders correctly as an ASCII hyphen in my `man` version `2.13.1`. My `man` comes from the `man-db` package on Archlinux, whose homepage is: https://gitlab.com/man-db/man-db

It also renders correctly using `man` on macOS. I don't have `gman` even though I have `coreutils` installed. I don't know how to install GNU man. All of the obvious searches don't show anything for me.

And finally, to prove that ripgrep is not emitting some kind of weird hyphen, you can ask it to generate the man page:

```
$ rg --generate man | rg pre-glob
flag. One possible mitigation to this is to use the \fB\-\-pre-glob\fP flag to
\fB\-\-pre-glob\fP=\fIGLOB\fP
more \fB\-\-pre-glob\fP flags are given, then only files that match the given set
then it is possible to use \fB\-\-pre\fP \fIpre-pdftotext\fP \fB--pre-glob
Multiple \fB\-\-pre-glob\fP flags may be used. Globbing rules match
```

---

_Closed by @BurntSushi on 2025-09-06 17:10_

---

_Label `invalid` added by @BurntSushi on 2025-09-06 17:10_

---

_Comment by @blyxxyz on 2025-09-06 18:39_

In `\-\-pre-glob` the leading hyphens are escaped but the internal hyphen isn't.

Seems like some roff implementations in some environments need all the hyphens to be escaped in order to stay ASCII: https://lists.gnu.org/archive/html/groff/2021-01/msg00074.html

---

_Comment by @BurntSushi on 2025-09-06 18:43_

Wow, okay. Your link doesn't work, but it's an easy change on my end regardless.

---

_Reopened by @BurntSushi on 2025-09-06 18:43_

---

_Label `invalid` removed by @BurntSushi on 2025-09-06 18:52_

---

_Label `bug` added by @BurntSushi on 2025-09-06 18:52_

---

_Label `rollup` added by @BurntSushi on 2025-09-06 18:52_

---

_Comment by @hmelman on 2025-09-06 21:39_

Thanks for looking into this.  Here are two references from debian:

https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1052675

https://lists.debian.org/debian-devel/2023/10/msg00085.html

Yes it was brew install man-db and either I or it offered an option to install as gman.


---

_Closed by @BurntSushi on 2025-09-20 01:08_

---

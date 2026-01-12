```yaml
number: 596
title: "--with-filename doesn't show filenames on each match in a tty"
type: issue
state: closed
author: dfabulich
labels:
  - doc
assignees: []
created_at: 2017-09-06T09:13:01Z
updated_at: 2018-02-20T12:43:25Z
url: https://github.com/BurntSushi/ripgrep/issues/596
synced_at: 2026-01-12T16:13:22Z
```

# --with-filename doesn't show filenames on each match in a tty

---

_@dfabulich_

I like having `rg` print a filename on each matched line. I use iTerm2 for macOS, where I can command-click on a line like `foo.c:123:blah` and have Sublime automatically open `foo.c` at line 123.

By default, `rg` enables `--heading` in a tty:
> This shows the file name above clusters of matches from each file instead of showing the file name for every match. This is the default mode at a tty.

That breaks my use case, so I tried using `-H` or `--with-filename`:
> Prefix each match with the file name that contains it. This is the default when more than one file is searched.

I was quite surprised to find that this did nothing. Of course it says "This is the default when more than one file is searched," but I was searching more than one file and it was still just showing the heading and not showing the file name with "each match" on each line.

In fact, to get the output I want, I have to use `--no-heading`:
> Don't group matches by each file. If -H/--with-filename is enabled, then file names will be shown for every line matched. This is the default mode when not at a tty.

I think the current behavior of `--with-filename` would be better described like this:
> Display the file name for matches. This is the default when more than one file is searched. If --heading is enabled, the file name will be shown above clusters of matches from each file; otherwise, the file name will be shown on each match.

---

_Comment by @BurntSushi on 2017-09-06 09:23_

LGTM!

On Sep 6, 2017 05:13, "dfabulich" <notifications@github.com> wrote:

> I like having rg print a filename on each matched line. I use iTerm2 for
> macOS, where I can command-click on a line like foo.c:123:blah and have
> Sublime automatically open foo.c at line 123.
>
> By default, rg enables --heading:
>
> This shows the file name above clusters of matches from each file instead
> of showing the file name for every match. This is the default mode at a tty.
>
> That breaks my use case, so I tried using -H or --with-filename:
>
> Prefix each match with the file name that contains it. This is the default
> when more than one file is searched.
>
> I was quite surprised to find that this did nothing. Of course it says
> "This is the default when more than one file is searched," but I was
> searching more than one file and it was still just showing the heading and
> not showing the file name with "each match" on each line.
>
> In fact, to get the output I want, I have to use --no-heading:
>
> Don't group matches by each file. If -H/--with-filename is enabled, then
> file names will be shown for every line matched. This is the default mode
> when not at a tty.
>
> I think the current behavior of --with-filename would be better described
> like this:
>
> Display the file name for matches. This is the default when more than one
> file is searched. If --heading is enabled, the file name will be shown
> above clusters of matches from each file; otherwise, the file name will be
> shown on each match.
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/596>, or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34m5rwVuk6Zy-Ux0FbL_VNMBYQR93ks5sfmIegaJpZM4POEgZ>
> .
>


---

_Comment by @dfabulich on 2017-09-06 09:25_

I'm not sure if my use case is particularly unusual, but I'd prefer `--no-heading` by default in a tty. Am I right in understanding that since there's no `.rgrc` #196 I'd have to `alias` `rg` to get that behavior?

---

_Comment by @BurntSushi on 2017-09-06 09:27_

Yes. Defaults aren't changing. Sorry.

On Sep 6, 2017 05:25, "dfabulich" <notifications@github.com> wrote:

> I'm not sure if my use case is particularly unusual, but I'd prefer
> --no-heading by default in a tty. Am I right in understanding that since
> there's no .rgrc #196 <https://github.com/BurntSushi/ripgrep/issues/196>
> I'd have to alias rg to get that behavior?
>
> —
> You are receiving this because you commented.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/596#issuecomment-327427790>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34u0srvv4L0kcSfJbRd40LeehUL05ks5sfmULgaJpZM4POEgZ>
> .
>


---

_Closed by @BurntSushi on 2017-09-06 12:25_

---

_Comment by @KindDragon on 2018-02-19 16:44_

Can you add short option for `--no-heading`?

---

_Comment by @BurntSushi on 2018-02-19 16:45_

@KindDragon Please don't post unrelated requests in closed issues. Please open a new issue and fill out the provided template.

---

_Label `doc` added by @BurntSushi on 2018-02-20 12:43_

---

```yaml
number: 119
title: "rg doesn't reset terminal color when interrupted"
type: issue
state: closed
author: lambda
labels:
  - bug
assignees: []
created_at: 2016-09-27T23:27:26Z
updated_at: 2016-10-30T01:54:24Z
url: https://github.com/BurntSushi/ripgrep/issues/119
synced_at: 2026-01-12T18:23:11Z
```

# rg doesn't reset terminal color when interrupted

---

_@lambda_

I was grepping a large log file, and accidentally wrote a regex that matched a very large number of lines. After waiting for a minute of it just printing out lines to stdout, I killed `rg` with <kbd>Ctrl</kbd>-<kbd>C</kbd>. However, that left the `time` output and my shell prompt blue since apparently I'd interrupted it in the process of printing out a line number.

`rg` should probably catch SIGINT and reset the terminal color (or any other terminal changes) before exiting.


---

_Label `bug` added by @BurntSushi on 2016-09-27 23:31_

---

_Comment by @BurntSushi on 2016-09-27 23:34_

Agreed.

Looks like we might be able to get away with the [`ctrlc`](https://github.com/Detegr/rust-ctrlc) crate.


---

_Comment by @ticki on 2016-10-14 17:17_

Or simply go into raw mode and handle `C-c` manually. [termion](http://github.com/ticki/termion) allows this:
1. Enter raw mode.
2. Read from the TTY (`termion::get_tty`).
3. Handle `termion::event::Key::Ctrl('c')` manually.


---

_Comment by @BurntSushi on 2016-10-14 17:38_

`termion` doesn't work on Windows, right? (Which I suppose isn't strictly a problem per se, I don't even know if this issue exists on Windows.)

In any case, it seems strictly better to handle `SIGTERM` instead of `C-c` manually, since `SIGTERM` can come from anywhere.


---

_Comment by @andyleejordan on 2016-10-18 18:47_

@BurntSushi it's starting to exist on Windows with the additional support of ASCII color codes in new versions of ConHost shipped in Windows 10. (Oh god why do I know that?)


---

_Comment by @BurntSushi on 2016-10-18 18:51_

Yeah I know, but that's too new to rely on IMO.

On Oct 18, 2016 14:47, "Andrew Schwartzmeyer" notifications@github.com
wrote:

@BurntSushi https://github.com/BurntSushi it's starting to exist on
Windows with the additional support of ASCII color codes in new versions of
ConHost shipped in Windows 10. (Oh god why do I know that?)

—
You are receiving this because you were mentioned.

Reply to this email directly, view it on GitHub
https://github.com/BurntSushi/ripgrep/issues/119#issuecomment-254602206,
or mute the thread
https://github.com/notifications/unsubscribe-auth/AAb34v_mASLh8OVGEitsaghaWFhfDGv-ks5q1RQ9gaJpZM4KIQ4y
.


---

_Comment by @BurntSushi on 2016-10-18 18:51_

Yeah I know, but that's too new to rely on IMO.

On Oct 18, 2016 14:47, "Andrew Schwartzmeyer" notifications@github.com
wrote:

> @BurntSushi https://github.com/BurntSushi it's starting to exist on
> Windows with the additional support of ASCII color codes in new versions of
> ConHost shipped in Windows 10. (Oh god why do I know that?)
> 
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/119#issuecomment-254602206,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34v_mASLh8OVGEitsaghaWFhfDGv-ks5q1RQ9gaJpZM4KIQ4y
> .


---

_Closed by @BurntSushi on 2016-10-30 01:54_

---

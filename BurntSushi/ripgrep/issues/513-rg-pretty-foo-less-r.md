```yaml
number: 513
title: "rg --pretty 'foo' | less -r"
type: issue
state: closed
author: sumonto
labels:
  - question
assignees: []
created_at: 2017-06-14T00:57:44Z
updated_at: 2017-10-22T01:30:22Z
url: https://github.com/BurntSushi/ripgrep/issues/513
synced_at: 2026-01-12T16:13:22Z
```

# rg --pretty 'foo' | less -r

---

_@sumonto_

Using Mac

Search any code base with rg for a common term 'foo' in .cpp files. 
Make sure the result is at least 1.5 pages.

Move to the end and top multiple times you shall see the output is inconsistent with which file has which occurrences of 'foo'

Might be a hard to repro issue.

---My settings---
rg ()
{
    /usr/local/bin/rg --pretty $@ | less -r
}


---

_Comment by @BurntSushi on 2017-06-14 01:20_

 > Move to the end and top multiple times you shall the o/p is inconsistent with which file has which occurrences of 'foo'

Sorry, but I don't understand. What is `o/p`? Does this happen with other utilities that emit colors?

---

_Comment by @sumonto on 2017-06-14 17:54_

Fixed my comments + settings.

---

_Comment by @BurntSushi on 2017-06-14 17:59_

Sorry, but I still don't understand. I'll repeat my second question: Does this happen with other utilities that emit colors?

Frankly, it sounds like some weird rendering bug in your terminal or something. But I don't really understand the issue.

---

_Label `question` added by @BurntSushi on 2017-06-14 17:59_

---

_Comment by @sumonto on 2017-06-14 18:17_

Sorry, some how part of my message got clipped.
Make sure there is enough matches to page at least 1-2 times
I don't see this issue with other utilities like grep

$ rg -g '!test' foobar

Good output
-------------

file1/file/path/file1.cpp
206: xxxxxxfoobarxxx

file2/file/path/file2.cpp
306: xxxxxxfoobarxxx

file3/file/path/file3.cpp
222: xxxxfoobar

file4/file/path/file4.cpp
312: xxxxxxfoobarxxx

After a few scrolls up/down (using 'gg' or 'shift-g')
Bad output
-----------
file1/file/path/file1.cpp
206: xxxxxxfoobarxxx

file2/file/path/file2.cpp

file3/file/path/file3.cpp

file4/file/path/file4.cpp
312: xxxxxxfoobarxxx



---

_Comment by @BurntSushi on 2017-06-14 20:02_

I don't know how to reproduce this. My best guess is that this isn't a problem with ripgrep. But I can't explain it. I'll leave this open for now in case anyone else has any ideas on what this problem is. It would help if you could provide more information about your environment:

1. What version of ripgrep?
2. Which shell are you using?
3. What operating system?
4. What version of `less` are you using?
5. What terminal emulator are you using?
6. Are you using tmux or screen?

---

_Comment by @sumonto on 2017-06-15 19:11_

Thanks, I know it is a difficult repro. 

What version of ripgrep? 0.5.2
Which shell are you using? bash
What operating system? Mac OS latest
What version of less are you using? default less with OS
What terminal emulator are you using? iTerm2
Are you using tmux or screen? tmux 2.3

---

_Comment by @BurntSushi on 2017-06-15 19:21_

Does the issue happen outside tmux or in other terminal emulators?

On Jun 15, 2017 3:11 PM, "sumonto" <notifications@github.com> wrote:

Thanks, I know it is a difficult repro.

What version of ripgrep? 0.5.2
Which shell are you using? bash
What operating system? Mac OS latest
What version of less are you using? default less with OS
What terminal emulator are you using? iTerm2
Are you using tmux or screen? tmux 2.3

—
You are receiving this because you commented.

Reply to this email directly, view it on GitHub
<https://github.com/BurntSushi/ripgrep/issues/513#issuecomment-308839186>,
or mute the thread
<https://github.com/notifications/unsubscribe-auth/AAb34tl4Kuzbihigf8P8mwe73pH1qBOhks5sEYH0gaJpZM4N5Qig>
.


---

_Comment by @polarathene on 2017-07-25 06:39_

Be sure to test without any shell frameworks/tools like zsh oh-my-zsh or anything else that modifies the rendering.

I remember the way a zsh theme rendered would have draw issues if you resized the window(even if you set it back to the original size prior).

For reproducing, if you're able to consistently reproduce it on your machine, save the output to an external file, then instead of ripgrep, confirm with running `cat` on that file and piping to `less` instead. Supposedly, you'd run into the same issue. Once you're viewing the output in a tool like less I don't see how ripgrep could be responsible for changing what's being rendered?

---

_Comment by @BurntSushi on 2017-10-22 01:30_

I was never able to reproduce this and I don't have any guesses as to what the problem is, so I'm just going to close it.

---

_Closed by @BurntSushi on 2017-10-22 01:30_

---

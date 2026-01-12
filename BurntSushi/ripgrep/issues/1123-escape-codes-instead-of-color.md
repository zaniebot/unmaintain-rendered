```yaml
number: 1123
title: escape codes instead of color
type: issue
state: closed
author: thorstenkampe
labels:
  - question
assignees: []
created_at: 2018-11-27T17:55:30Z
updated_at: 2019-01-24T14:37:49Z
url: https://github.com/BurntSushi/ripgrep/issues/1123
synced_at: 2026-01-12T16:13:23Z
```

# escape codes instead of color

---

_@thorstenkampe_

#### What version of ripgrep are you using?

```
ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Not installed just downloaded

#### What operating system are you using ripgrep on?

Windows 10

#### Describe your question, feature request, or bug.

ripgrep outputs escape sequences that are not interpreted as color codes by the terminal.

#### If this is a bug, what are the steps to reproduce the behavior?

1. Use any winpty based terminal (Visual Studio Code, PyCharm or ConEmu with winpty)
2. Start PowerShell
3. pipe the output to ripgrep

#### If this is a bug, what is the actual behavior?

```
pwsh> ps | rg --debug cron
DEBUG|grep_regex::literal|grep-regex\src\literal.rs:58: literal prefixes detected: Literals { lits: [Complete(cron)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset\src\lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
     1784       1    1784       1784  ?         197609   Nov 24 /usr/sbin/?[0m?[1m?[31mcron?[0m
```

#### If this is a bug, what is the expected behavior?

Output color codes that the terminal understands


---

_Comment by @thorstenkampe on 2018-11-27 18:02_

Showing the issue in VS Code: ag and pt work fine rg (and sift) don't show colors.
(Using `.exe` to show the commands are not aliased)
![grafik](https://user-images.githubusercontent.com/475462/49101651-06910f00-f277-11e8-9586-6fc5286f4bc7.png)



---

_Comment by @BurntSushi on 2018-11-27 18:12_

This seems like a bug in the console you're using, not ripgrep. Can you reproduce this in a standard Windows console?

---

_Comment by @okdana on 2018-11-27 18:16_

Something to do with this?

https://github.com/rprichard/winpty/issues/140

---

_Comment by @thorstenkampe on 2018-11-27 18:17_

Yes, the issue is only when using the winpty wrapper

---

_Comment by @thorstenkampe on 2018-11-27 18:26_

On the other hand, these "interactions" work (not sure how the winpty wrapper would not interfere here but in the above case):
![grafik](https://user-images.githubusercontent.com/475462/49102849-2c6be300-f27a-11e8-9191-96f316e62930.png)


---

_Label `question` added by @BurntSushi on 2018-11-28 00:50_

---

_Comment by @BurntSushi on 2018-11-28 00:54_

This does indeed look like a bug in winpty in that it simply cannot handle ANSI escape sequences. ripgrep uses them in this context because, on Windows, it will attempt to enable VT emulation, which is what makes ANSI escapes work in Windows 10. If enabling VT emulation is successful, then ripgrep will use ANSI escape sequences instead of using the normal Windows console APIs for colors.

One possible work-around for this would be to expose an option of some kind to forcefully disable the use of VT emulation. Unfortunately, the plumbing to do this is probably non-trivial. Therefore, whether we add the work-around or not really depends on the severity of this bug. It's hard for me to estimate how severe this is.

---

_Comment by @thorstenkampe on 2018-11-28 08:02_

I'm not really able to qualify the severity. As you can see from above screenshots, piping a Windows applications output (`es.exe`) to rg works fine. Same with cat from a file. I can't tell what's special about `ps`.

Maybe it would make sense to see why pt and ag don't exhibit the problem.

---

_Comment by @BurntSushi on 2018-11-28 10:21_

> Maybe it would make sense to see why pt and ag don't exhibit the problem.

I hinted at this with my work around above. My guess is that neither of those programs know how to use ANSI escape sequences on Windows and instead always interact with the Windows console APIs.

I would suggest lobbying the winpty developers to get their thoughts on the nature of this bug.

---

_Comment by @BurntSushi on 2019-01-24 14:37_

I'm going to close this under the assumption that this is a bug in winpty.

---

_Closed by @BurntSushi on 2019-01-24 14:37_

---

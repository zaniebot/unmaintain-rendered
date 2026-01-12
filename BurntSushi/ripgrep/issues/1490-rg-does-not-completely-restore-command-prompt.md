```yaml
number: 1490
title: rg does not completely restore Command Prompt colors on Windows 10 Pro 1903
type: issue
state: closed
author: jason-s
labels:
  - question
  - windows
assignees: []
created_at: 2020-02-18T17:37:50Z
updated_at: 2020-02-21T19:05:10Z
url: https://github.com/BurntSushi/ripgrep/issues/1490
synced_at: 2026-01-12T16:13:23Z
```

# rg does not completely restore Command Prompt colors on Windows 10 Pro 1903

---

_@jason-s_

#### What version of ripgrep are you using?

ripgrep 11.0.2 (rev 3de31f7527)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

from the latest Github binary release

#### What operating system are you using ripgrep on?

Windows 10 Pro 1903
(something changed from the previous Windows 10 Pro version I was using; this issue did not start occurring until immediately after the 1903 install a few days ago)

#### Describe your question, feature request, or bug.

Using ripgrep and letting it run to completion (as opposed to Ctrl+c interruption as described in [PR#187](https://github.com/BurntSushi/ripgrep/pull/187)) appears to restore terminal colors, but it does not do so completely. 

If I run `less` after ripgrep, it displays as white-on-white.

If I run `color` to restore colors to window defaults, then `less` works correctly.

#### If this is a bug, what are the steps to reproduce the behavior?

1. Run `less` to verify colors are correct
2. Run ripgrep (on anything) to completion
3. Run `less` again, observe white-on-white behavior
4. Run `color` to restore command prompt colors
5. Run `less` again, observe correct color behavior

I am using `less`  from [GoW 0.8.0](https://github.com/bmatzelle/gow/releases)

I tried this with various console colors (white on black, black on white, yellow on black, red on blue-green) and got the same result.

#### If this is a bug, what is the actual behavior?

(see steps above)

#### If this is a bug, what is the expected behavior?

(see steps above)


---

_Comment by @BurntSushi on 2020-02-18 17:44_

What terminal are you using? What shell are you using?

I'm having a hard time understanding the steps to reproduce the issue. Could you please list out the precise commands you are running?

> (something changed from the previous Windows 10 Pro version I was using; this issue did not start occurring until immediately after the 1903 install a few days ago)

This might not be an issue with ripgrep then? Can you test other programs that output color?

---

_Label `question` added by @BurntSushi on 2020-02-18 17:44_

---

_Comment by @jason-s on 2020-02-18 17:52_

Terminal and shell: cmd.exe (the Command Prompt that ships with Windows)

Interestingly enough there is another symptom that doesn't involve `less` at all.

I've now created a directory `c:\tmp\ripgrep-1490` with one file in it, test.txt, containing the text "hi".

Precise commands:

```
> color 73
> rg hi
```

and the colors are... strange after rg runs:

![image](https://user-images.githubusercontent.com/738893/74763301-cb447c80-523c-11ea-904c-782cf8ab0282.png)


---

_Comment by @jason-s on 2020-02-18 17:53_

>This might not be an issue with ripgrep then? Can you test other programs that output color?

Hmm, I'll check; do you have any suggestions? (the only one I can think of is a peculiar utility called `waf`)

I just tried `waf` (which uses terminal escapes for changing colors) and it seems to do the right thing, restoring all color state. It doesn't affect the output of `less` and works fine with `color 73`.

---

_Comment by @jason-s on 2020-02-18 17:57_

(grrr, nothing like a Microsoft command... `color` appears to have no way to *view* the current colors or the default colors of the window, it only lets you set them.)

---

_Comment by @BurntSushi on 2020-02-18 18:04_

I'm not a Windows user, so I don't know. But I'm pretty sure `grep` itself has colors that work on Windows.

The key here is finding a program that works, looking at its source and figuring out what it's doing differently than ripgrep.

>  (which uses terminal escapes for changing colors)

ripgrep should too. ripgrep only uses the old console APIs when ANSI colors aren't available.

---

_Comment by @jason-s on 2020-02-18 18:30_

OK it looks like there are at least *four* color values relating to the Windows Command prompt.

1. the "default" color value from the Registry (`reg query "HKEY_CURRENT_USER\Software\Microsoft\Command Processor" /v DefaultColor` -- I have mine set right now to `02` = green on white for debugging this issue... think I'll keep it this way)
2. the "natural" (??? my terminology) color of the Command Prompt which was set at the time that `cmd.exe` started running, and which you can set with the `/T:xy` option where `xy` is the color code
3. the current color of the command prompt.
4. the last color set manually via the UI (upper left corner of command prompt -> Properties -> Colors) -- I have no idea where this color is stored but it's stored somewhere

![image](https://user-images.githubusercontent.com/738893/74765929-a6063d00-5241-11ea-881b-4d9f95b22d4c.png)


With my Registry entry at `02` (green on white), and colors of a Command Prompt window set to that horrid blue-on-red, I can type:

- `start cmd` -- opens command window W1 with green-on-white
- `cd \tmp\ripgrep-1490` in W1
- `start cmd /T:7e` from W1 -- opens command window W2 with yellow-on-black
- `rg hi` in W1 -- this sets the colors to blue-on-red

W1:

![image](https://user-images.githubusercontent.com/738893/74766095-f5e50400-5241-11ea-8fb0-63a99f3f9d3b.png)

W2:

![image](https://user-images.githubusercontent.com/738893/74766113-fed5d580-5241-11ea-9843-de97eb887db0.png)




---

_Comment by @jason-s on 2020-02-18 18:31_

I've got to get back to work for now, but if there are any other well-known programs I can run that change color, I'm happy to give them a try.

What mechanism does `rg` use to determine the color it should restore after it runs?

---

_Comment by @BurntSushi on 2020-02-18 18:41_

> What mechanism does rg use to determine the color it should restore after it runs?

It doesn't. It just emits a "reset" ANSI escape code. The terminal emulator is then responsible for interpreting that and resetting the colors appropriately. This is why I suspect this might not be a ripgrep problem. (If there were something wrong with the ANSI reset escape, then it would have an immediate and wide impact on almost all users.)

In any case, it could be a while before I open up my Windows machine to test this. If I can't reproduce it, then someone else will have to investigate.

---

_Comment by @jason-s on 2020-02-18 18:52_

No problem. It's not a huge issue since typing `color` at a command prompt fixes things, so I have a workaround, but it's rather annoying.

>It doesn't. It just emits a "reset" ANSI escape code.

https://en.wikipedia.org/wiki/ANSI_escape_code#3/4_bit

>To reset colors to their defaults, use ESC[39;49m (not supported on some terminals), or reset all attributes with ESC[0m.

Hmm. Apparently, Windows terminal windows don't handle ANSI escape codes natively; if I try in Python on my Windows machine, I get:

```
C:\>python
Python 2.7.14 |Anaconda, Inc.| (default, Oct 15 2017, 03:34:40) [MSC v.1500 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> print(u'\u001b[31mHello')
[31mHello
>>> print u"\u001b[31mHelloWorld\u001b[0m"
[31mHelloWorld[0m
```

From https://pypi.org/project/colorama/:

>Makes ANSI escape character sequences (for producing colored terminal text and cursor positioning) work under MS Windows.
>
>ANSI escape character sequences have long been used to produce colored terminal text and cursor positioning on Unix and Macs. Colorama makes this work on Windows, too, by wrapping stdout, stripping ANSI sequences it finds (which would appear as gobbledygook in the output), and converting them into the appropriate win32 calls to modify the state of the terminal. On other platforms, Colorama does nothing.

So which library does ripgrep use on Windows to intercept ANSI escape sequences and get them to work properly?

---

_Comment by @BurntSushi on 2020-02-18 19:02_

No. ripgrep writes ANSI escapes directly. This only works once VT emulation is enabled, which ripgrep does. But your Python example does not, which is why it doesn't work.

---

_Comment by @BurntSushi on 2020-02-18 19:04_

(VT emulation is a new feature in Windows as of Windows 10, which is probably why colorama doesn't use it.)

---

_Comment by @jason-s on 2020-02-18 20:43_

>No. ripgrep writes ANSI escapes directly. This only works once VT emulation is enabled, which ripgrep does. But your Python example does not, which is why it doesn't work.

Ah -- ok. How does this get enabled? If I search google for "Windows 10" "VT emulation" nothing seems to come up.

Never mind, found it:

- https://docs.microsoft.com/en-us/windows/console/setconsolemode
- https://stackoverflow.com/questions/36760127/how-to-use-the-new-support-for-ansi-escape-sequences-in-the-windows-10-console

This works:

```
import ctypes

kernel32 = ctypes.windll.kernel32
kernel32.SetConsoleMode(kernel32.GetStdHandle(-11), 7)

print(u'\u001b[31mHello')
```

---

_Comment by @jason-s on 2020-02-18 20:50_

OK, I can reproduce the same issue in a Python program `vt.py`:

```
import ctypes

kernel32 = ctypes.windll.kernel32
kernel32.SetConsoleMode(kernel32.GetStdHandle(-11), 7)

for fg in xrange(30,37):
    print(u'\u001b[%dmHello (fg=%d)' % (fg,fg))
print "Now to reset"
print u'\u001b[39;49m'
```

![image](https://user-images.githubusercontent.com/738893/74776854-9264d180-5255-11ea-9638-259ee7150192.png)

Same behavior happens with `ESC[0m`.

---

_Label `windows` added by @BurntSushi on 2020-02-18 22:50_

---

_Comment by @jason-s on 2020-02-21 19:05_

To clarify -- it's not a ripgrep issue, it's something that is inherently wrong with this Windows 10 update. (Or *my* instance of Windows 10, and it got corrupted during the Win10 update.)

---

_Closed by @jason-s on 2020-02-21 19:05_

---

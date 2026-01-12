```yaml
number: 2161
title: "\"Colour wrapping\""
type: issue
state: closed
author: gvanem
labels:
  - wontfix
assignees: []
created_at: 2022-03-12T10:39:42Z
updated_at: 2023-11-24T20:04:09Z
url: https://github.com/BurntSushi/ripgrep/issues/2161
synced_at: 2026-01-12T16:13:24Z
```

# "Colour wrapping"

---

_@gvanem_

#### What version of ripgrep are you using?

ripgrep 13.0.0, -SIMD -AVX (compiled)

#### How did you install ripgrep?

Built it myself by `cargo build  --all --release --features pcre2`

#### What operating system are you using ripgrep on?

Win-10, 21H1, ver. 19043.1586.

#### Describe your bug.

As part of a `pygrep.bat` file I use RipGrep like:
```
setlocal
set RG_COLORS=--colors "match:fg:white" --colors "match:bg:red" ^
              --colors "path:fg:green"  --colors "path:style:intense" ^
              --colors "line:bg:blue"   --colors "line:style:intense"   --colors "line:fg:white" ^
              --colors "column:bg:blue" --colors "column:style:intense" --colors "column:fg:green"

::
:: handle option '-c' to set RG_OPTIONS=-i' etc. etc.
::
rg.exe --after-context=1 --mmap --fixed-strings f:\ProgramFiler\Python27\test %RG_OPTIONS% %RG_COLORS%  %*
```

With the Windows console width set to 120,  a command like `pygrep.bat IP2Location`, shows this:
![pygrep-rg-colours](https://user-images.githubusercontent.com/945271/158013562-c8e1a670-3278-4df1-ace1-7c85f47dbc0d.png)

the red colour bleeds over to the next line marking a non-match. 

#### What is the expected behaviour?

With the WinConsole width set to e.g. 130, there is no problem. A `cls & pygrep.bat IP2Location`, shows this:
![pygrep-rg-colours-130](https://user-images.githubusercontent.com/945271/158013673-1280bf8d-43bf-4264-b068-68c22444f944.png)

In lack of a better description, I named this "*Colour wrapping*". I've no idea what's the real issue here.
**But** this depends on which Y-position the .bat-file was invoked on. It's as if a 1 line scroll when `"match:bg:red"` is in 
effect causes this *bleeding*. Since if I increase the console **height** to avoid a scroll, then there's no *bleeding*.

*PS*. I wrote that `netstat.py` script using [psutil](https://github.com/giampaolo/psutil).


---

_Comment by @BurntSushi on 2022-03-12 13:35_

This feels more like a rendering bug than a problem with ripgrep. Regardless, I'm not a Windows user. Someone else will have to debug this.

---

_Comment by @gskt17 on 2022-03-18 01:00_

I'm on windows, so I can look into this. That said, I'm also not an expert on Windows. My _guess_ would be this has something to do with the way the Windows console writes wrapped lines; it looks to me like what happened was that after writing the `i` in `IP2Location`, the cursor moved down and then back to the beginning of the line, setting the background and foreground colors at each location along the way. I suspect that this is documented behavior somewhere? I think the fix will probably go in `termcolor` and not `rg`, if a fix is even possible.

---

_Comment by @gskt17 on 2022-03-18 20:51_

[It seems like this issue is known](https://github.com/JanDeDobbeleer/oh-my-posh/issues/65)

It may be related to these resizing bugs:
- https://github.com/microsoft/terminal/issues/32
- https://stackoverflow.com/questions/66123718/write-host-with-background-colour-fills-the-entire-line-with-background-colour-w

Clearing the remainder of the line when writing a newline (per https://github.com/JanDeDobbeleer/oh-my-posh/issues/65) will apparently produce the correct behavior.

@BurntSushi: [Apparently, Windows already has VT100 (ANSI) escape sequence support.](https://docs.microsoft.com/en-us/windows/console/console-virtual-terminal-sequences) They still require accessing the Windows console API to enable, but if I'm reading the docs correctly, they should work the same way any other OS does. I think they probably exist because of the Linux subsystem in Windows 10.

---

_Comment by @gvanem on 2022-03-19 05:58_

@gskt17 Your links mentions PowerShell for the most part. I forgot to say I'm using [4NT](http://www.jpsoft.com) as my shell.
So the bug is in probably in the underlaying conhost process.

---

_Comment by @gskt17 on 2022-03-29 19:09_

@gvanem Sorry for late reply, but the bug seems to be present in any shell on Windows, so I think you're correct.

@BurntSushi What would you say the best way to insert a line clear just before a newline is written in the output functions? I think I've found a place but I can't say for sure.

I've hit another snag, too: Windows API doesn't offer the equivalent to "clear to remainder of line," only VT mode does, so I would need to manually change the remainder of the line on wrapping. I'm going to see about forking `termcolor` to add some of that functionality. Also, I should note that the bug only takes place on write and resizing the console immediately fixes it.

---

_Comment by @BurntSushi on 2022-03-29 22:58_

To be honest, I just don't really have the bandwidth right now to dig into this. The Windows console API is indeed quite limited. On the other hand, it's been quite some time (years now) since Windows supported ANSI color escapes. So if you fix it for ANSI escapes, that might be enough.

The right place for this to go is indeed probably in `termcolor`, likely all the way down in the lowest levels of the `write` routine. It should only happen on Windows, and it might be a good idea to provide a way to disable it in the crate API.

Another option here, honestly, is to not fix it. This really seems like a bug in how Windows consoles handle this stuff to me. But if the fix is to do a search for `\n` on every `write` call, then I guess it's just yet another penalty that Windows users pay. (They already pay a fair amount. ripgrep is generally much quicker on Linux/macOS in my experience.)

---

_Comment by @ninmonkey on 2022-04-03 19:08_

> Another option here, honestly, is to not fix it... So if you fix it for ANSI escapes

## The Short Version is that Microsoft says:

> ["For all new and ongoing development on Windows, virtual terminal sequences are recommended as the way of interacting with the terminal."](https://docs.microsoft.com/en-us/windows/console/classic-vs-vt#recommendations)

- It says: Don't use the console API
- Do use `VT Sequences`
- On `Win10+` conhost is `xterm` compatible
- In 2022 the `windowsterminal` will replace the existing one as the system's default. It is in `win11` 
- internally the conhost translates to VT Sequences, but it's not perfect. OP's issue seems to 
- There is [A limited subset of Windows Console APIs is still necessary to establish the initial environment](https://docs.microsoft.com/en-us/windows/console/classic-vs-vt#exceptions-for-using-windows-console-apis)

I didn't see this cleary summarized, so I added extra information so that they may link to it. place.


## Windows `xterm` compatibility 


| Name                   | xterm compatible?                                                                     |
| ---------------------- | ------------------------------------------------------------------------------------- |
| `Windows Console Host` | **Yes** for `Windows 10` and higher                                                   |
| `Windows Terminal`     | **Yes**</br>Supports **24bit color** and `utf8` support<br> Is the default in `win11` |


## Quotes from the Documentation

- > ["Our recommendation is to replace the classic Windows Console API with virtual terminal sequences. This article will outline the difference between the two and discuss the reasons for our recommendation."](https://docs.microsoft.com/en-us/windows/console/classic-vs-vt)

- > ["A command-line application desiring full compatibility across platforms and full support of all new features and scenarios both on Windows and elsewhere is therefore recommended to move to virtual terminal sequences and adjust the architecture of command-line applications to align with all platforms"](https://docs.microsoft.com/en-us/windows/console/classic-vs-vt#exceptions-for-using-windows-console-apis).    

- > [Over the course of 2022, we are planning to make Windows Terminal the default experience on Windows 11 devices](https://devblogs.microsoft.com/commandline/windows-terminal-as-your-default-command-line-experience/), Windows terminal is the new default console in windows11
- > Applications that use the [GetConsoleScreenBufferInfo family of APIs](https://docs.microsoft.com/en-us/windows/console/getconsolescreenbufferinfoex) to retrieve the active console colors in [Win32 format and then attempt to transform them into cross-platform VT sequences (for example, by transforming `BACKGROUND_RED` to `\x1b[41m`) may interfere with Terminal's ability to detect what background color the application is attempting to use.](https://docs.microsoft.com/en-us/windows/terminal/troubleshooting#technical-notes)
- > Application developers are encouraged to choose either Windows API functions or VT sequences for adjusting colors and not attempt to mix them.

That red one sounds like @gvanem OP's bug, and fits with the StackOverflow threads about bugs when resizing the window.

- > Future planning and `pseudoconsole`: There are **no plans to remove the Windows console APIs** from the platform.

- > On the contrary, [the` Windows Console host` has provided the `pseudoconsole` technology to translate existing Windows command-line application](https://docs.microsoft.com/en-us/windows/console/classic-vs-vt#future-planning-and-pseudoconsole) calls into `virtual terminal sequences` and forward them to another hosting environment remotely or across platforms

----

> @gskt17 should work like any other platform

The last link had a little bit on the origin

## issues

- [Long lines that wrap in terminal cause a background "highlight" on the next line](https://github.com/JanDeDobbeleer/oh-my-posh/issues/65)
- 

@BurntSushi 

> Windows users pay. (They already pay a fair amount. ripgrep is generally much quicker on Linux/macOS in my experience.)

Fairly recently -- maybe the last 5?, 10? years -- Microsoft has turned around. Instead of competing, they started integrating open source projects.  Powershell being both open source and cross platform is pretty neat. ripgrep works great on windows. 

---

_Comment by @BurntSushi on 2022-04-03 19:31_

I agree it works great. I'm speaking from experience benchmarking equivalent corpora with similar CPUs. Windows tends to be quite a bit slower.

It has been only a couple years since I've performed said benchmarks. And I do not claim to know why there is a difference, although I have a variety of guesses. Windows requires additional work in some cases with interacting with its system APIs (not console APIs).

---

_Comment by @BurntSushi on 2023-11-24 20:04_

This issue does not appear actionable on my end.

---

_Closed by @BurntSushi on 2023-11-24 20:04_

---

_Label `wontfix` added by @BurntSushi on 2023-11-24 20:04_

---

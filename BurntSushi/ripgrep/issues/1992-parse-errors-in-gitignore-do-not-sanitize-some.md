```yaml
number: 1992
title: Parse errors in .gitignore do not sanitize some terminal escape sequences before printing to stderr
type: issue
state: closed
author: Kimundi
labels:
  - wontfix
assignees: []
created_at: 2021-09-16T16:38:52Z
updated_at: 2021-10-22T19:04:30Z
url: https://github.com/BurntSushi/ripgrep/issues/1992
synced_at: 2026-01-12T16:13:24Z
```

# Parse errors in .gitignore do not sanitize some terminal escape sequences before printing to stderr

---

_@Kimundi_

#### What version of ripgrep are you using?

```
ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`apt install ripgrep` in a Ubuntu 20.04 WSL2 install.

#### What operating system are you using ripgrep on?

Ubuntu 20.04 in a WSL2

#### Describe your bug.

The line printed from a .gitignore in case of an parse errors might contain terminal escape sequences that mess up the terminal state.

#### What are the steps to reproduce the behavior?

I'm not sure which kind of terminal escape sequences these are, so in case this is terminal emulator specific:

- Be on windows
- Use the new "Windows Terminal"
- Be logged into a WSL2 instance with it (bash or fish)
- Have a `.gitignore` containing the following bytes: `\x5b\x5d\xc2\x96\xc2\x97\xc2\x98\xc2\x99\xc2\x9a\x0a`
- run `rg foo` or any other command that causes it to parse `.gitignore` files.
- You should now be able to observe that some terminal control sequences got send to the terminal

#### What is the actual behavior?

```
> hexdump -C .gitignore
00000000  5b 5d c2 96 c2 97 c2 98  c2 99 c2 9a 0a           |[]...........|
0000000d
> rg foo
./.gitignore: line 1: error parsing glob '[]': unclosed character class; missing ']'
^[[?1;0c
> [?1;0c
```

![grafik](https://user-images.githubusercontent.com/2903206/133650874-39677ecb-4a7f-43cc-81e8-9910e3e90aca.png)

#### What is the expected behavior?

Any terminal escape sequences should get sanitized out from the line, at least if printed to an interactive terminal.


---

_Comment by @BurntSushi on 2021-09-20 16:51_

I can't seem to reproduce this:

![no-color-mangling](https://user-images.githubusercontent.com/456674/134041804-0a6ef063-a8d5-44b6-8722-79120668ba4c.png)

Either way, I appreciate the bug report, but I think I'm going to mark this as `wontfix`. ripgrep passes through content it reads, as-is, pretty much everywhere. The same would happen if you were searching a file and ripgrep printed a match with ANSI escape codes, for example. Similarly for file names and probably other stuff. I'm not really sure it's ripgrep's place to get super paranoid about this and start cleansing everything it prints. It certainly doesn't seem like other tools do it.

---

_Closed by @BurntSushi on 2021-09-20 16:51_

---

_Label `wontfix` added by @BurntSushi on 2021-09-20 16:51_

---

_Comment by @PennRobotics on 2021-10-19 16:54_

I'm having a similar issue but not when printing to stderr. An extra color code is being inserted at the next prompt after rg exits. This is on WSL 2 using zsh (zsh-autosuggestions and zsh-syntax-highlighting) and Ubuntu 20.04 via Windows Terminal Preview 1.11.2731.0:

```
 dave@davepc > ~ >  rg -j 1 ok
...
install-from-source/vim/README_VIM9.md
51:That looks very promising!  It's just one example, but it shows how much
156:form of classes.  If you look at JavaScript for example, it has gone
219:This looks like JavaScript/TypeScript, thus many users will understand the

install-from-source/vim/CONTRIBUTING.md
18:pass after the fix.  Look through recent patches for examples and find help
58:Look in the header of the file for the name and email address.
 dave@davepc > ~ >  1;0c[CURSOR HERE]
```

I'm not sure where this extra escape code originates. This could be completely unrelated, but I needed to change vim settings because of ambiguous UTF-8 characters that made vim start in replace mode. In other words, I think the way characters are printed to the screen are somehow mishandled in wt.exe

**This does not happen for me in the normal Ubuntu console, only Ubuntu in Windows Terminal Preview!**

---

_Comment by @PennRobotics on 2021-10-22 17:29_

To extend the previous idea that this is UTF-8-related:
Using `--color=always` and `head`/`tail`, the exact line that causes an escape code is output as shown:

`install-from-source/vim/src/testdir/test_regexp_utf8.vim:153:    let identchars_ok = '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ_abcdefghijklmnopqrstuvwxyz`

In vim, this is the line that causes trouble:

`    let identchars_ok = '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ_abcdefghijklmnopqrstuvwxyz<80><81><82><83><84><85><86><87><88>    <89><8a><8b><8c><8d><8e><8f><90><91><92><93><94><95><96><97><98><99><9a><9b><9c><9d><9e><9f> ¡¢£¤¥¦§µÀÁÂÃÄÅÆÇÈÉÊËÌÍÎÏÐÑÒÓÔÕÖ    ØÙÚÛÜÝÞßàáâãäåæçèéêëìíîïðñòóôõöøùúûüýþÿ'  `

The `cat` output of *this* file also causes the escape code fragment, and the kicker is ... `cat` doesn't output color---at least not on my machine!

This behavior is NOT related to the font selection (e.g. Powerline or ligatures), since the problem occurs even with Courier as a font. Another detail that doesn't work? Someone has a blog post, *Using UTF-8 in the Windows Terminal*, that recommends enabling a checkbox in the administrative language settings.

This behavior is also **not related to ripgrep**, so this issue should stay closed. I'm adding information in case others find this issue first (as I did).

`iconv -f UTF-8 -t ASCII -c` to convert encoding as part of a pipe will strip away offending characters, but I don't know if that counts as a fix. The other option is to use the old terminal (conhost), but I think MS wants to get rid of this in the future.

---

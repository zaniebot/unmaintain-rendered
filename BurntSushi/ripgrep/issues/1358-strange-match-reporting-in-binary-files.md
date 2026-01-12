```yaml
number: 1358
title: Strange match reporting in binary files
type: issue
state: closed
author: vadz
labels: []
assignees: []
created_at: 2019-08-28T17:51:07Z
updated_at: 2019-08-28T19:58:35Z
url: https://github.com/BurntSushi/ripgrep/issues/1358
synced_at: 2026-01-12T16:13:23Z
```

# Strange match reporting in binary files

---

_@vadz_

#### What version of ripgrep are you using?

ripgrep 11.0.2 (rev 3de31f7527)
-SIMD -AVX (compiled)

#### How did you install ripgrep?

From [ripgrep-11.0.2-i686-pc-windows-gnu.zip](https://github.com/BurntSushi/ripgrep/releases/download/11.0.2/ripgrep-11.0.2-i686-pc-windows-gnu.zip)

#### What operating system are you using ripgrep on?

Windows 7.

#### If this is a bug, what are the steps to reproduce the behavior?

I wanted to use ripgrep to search for a fixed string on the entire disk: `rg -uuu -F "BAD SECTOR" c:\` (for context, this is after using `ddrescue` to make a copy of a bad disk and filling the unreadable sectors with this string).

#### If this is a bug, what is the actual behavior?

I get the following strange output:
```
Binary file c:\Windows\winsxs\amd64_microsoft-windows-s..restartup-repairbde_31bf3856ad364e35_6.1.7600.16385_none_2de932ff29b64a2c\r
epair-bde.exe matches (found "\u{0}" byte around offset 3)
```

The puzzling thing is the `\u{0}` part.

#### If this is a bug, what is the expected behavior?

Not sure, to be honest. Outputting the offset of the start of the match seems like a good idea, but I honestly have no idea what is the byte that ripgrep is trying to show to me.



---

_Comment by @BurntSushi on 2019-08-28 18:54_

It is telling you that it found a NUL byte at that offset, and is therefore treating the file as a binary file.

Please consider reading the guide's section about binary files. If you want to search binary files, then use the `-a/--text` flag.

---

_Closed by @BurntSushi on 2019-08-28 18:54_

---

_Comment by @vadz on 2019-08-28 19:48_

Sorry, I got confused and thought that `-uuu` implied `-a`. Thanks for the explanation.

---

_Comment by @BurntSushi on 2019-08-28 19:58_

@vadz That's okay. It used to until the ripgrep 11 release, where [binary filtering was overhauled](https://github.com/BurntSushi/ripgrep/blob/master/CHANGELOG.md#1100-2019-04-15).

---

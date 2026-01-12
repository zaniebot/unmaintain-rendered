```yaml
number: 1011
title: output string have garbled if fileencoding is not utf-8
type: issue
state: closed
author: fcying
labels:
  - invalid
assignees: []
created_at: 2018-08-12T07:00:11Z
updated_at: 2018-08-16T09:37:28Z
url: https://github.com/BurntSushi/ripgrep/issues/1011
synced_at: 2026-01-12T16:13:22Z
```

# output string have garbled if fileencoding is not utf-8

---

_@fcying_

#### What version of ripgrep are you using?
ripgrep 0.9.0 (rev 6afdf15d85)

#### How did you install ripgrep?
cargo build

#### What operating system are you using ripgrep on?

Debian

#### Describe your question, feature request, or bug.
I have a txt file, fileencoding is `CP936`, I search some string in this file, the output string have garbled, 
If I modify the fileencoding to `UTF-8`, it work fine.

#### If this is a bug, what are the steps to reproduce the behavior?

readme_cp936.txt, have garbled
fileencoding `cp936` in vim
```
file readme_cp936.txt 
readme_cp936.txt: ISO-8859 text, with CRLF line terminators

rg CONFIG_START readme_cp936.txt
1:	2¡¢ў¸Üproject.mk£ºʨ׃ǰʼµٖ·CONFIG_STARTºʹ𐠃ONFIG_SIZE¡£
2:	3iproject.mk£ºCONFIG_START
```

readme_utf8.txt, work fine.
fileencoding `utf-8` in vim,  work fine.
```
file readme_utf8.txt 
readme_utf8.txt: UTF-8 Unicode text, with CRLF line terminators

rg CONFIG_START readme_utf8.txt 
1:	2、修改\project.mk：设置起始地址CONFIG_START和大小CONFIG_SIZE。
2:	3iproject.mk：CONFIG_START
```
[readme_utf8.txt](https://github.com/BurntSushi/ripgrep/files/2280781/readme_utf8.txt)
[readme_cp936.txt](https://github.com/BurntSushi/ripgrep/files/2280782/readme_cp936.txt)



---

_Renamed from "can't display correct text if fileencoding is not utf-8" to "output string have garbled if fileencoding is not utf-8" by @fcying on 2018-08-12 07:02_

---

_Comment by @okdana on 2018-08-12 07:10_

There's a bit in the [guide](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#file-encoding) about this

The [WHATWG encoding standard](https://encoding.spec.whatwg.org/#concept-encoding-get) doesn't mention CP936 specifically, but i think it's covered under 'GBK' — that seems to work for me, anyway:

```
rg -Egbk CONFIG_START readme_utf8.txt
1:	2、修改\project.mk：设置起始地址CONFIG_START和大小CONFIG_SIZE。
2:	3iproject.mk：CONFIG_START
```

---

_Comment by @fcying on 2018-08-12 07:19_

@okdana  you are right, use `-Egpk`, can work fine.
`CP936` is what I saw in vim, if use `file`, it is `readme_cp936.txt: ISO-8859 text, with CRLF line terminators`
```
rg -Egbk CONFIG_START readme_cp936.txt
1:	2、修改\project.mk：设置起始地址CONFIG_START和大小CONFIG_SIZE。
2:	3iproject.mk：CONFIG_START
```

---

_Comment by @BurntSushi on 2018-08-14 13:15_

Yeah, I mean, I don't really understand what it is you want ripgrep to do here. How the text is shown to you is dependent on your terminal emulator. If you have your terminal emulator set to UTF-8 and you're searching non-UTF-8 contents without transcoding it, then you're going to have a bad time. There's nothing ripgrep can do about that.

---

_Closed by @BurntSushi on 2018-08-14 13:15_

---

_Label `invalid` added by @BurntSushi on 2018-08-14 13:15_

---

_Comment by @fcying on 2018-08-15 05:17_

@BurntSushi  
Can it recognize the file encoding and then automatically set encoding(ex, auto set `-Egbk`)?  Or it can set some file encoding  in ripgreprc for check?

---

_Comment by @BurntSushi on 2018-08-15 10:49_

> Can it recognize the file encoding and then automatically set encoding(ex, auto set -Egbk)?

No.

> Or it can set some file encoding in ripgreprc for check?

You can set any flag, including `-E/--encoding`, in your config file. But it will impact all searches.

---

_Comment by @fcying on 2018-08-16 02:31_

Now `-E` can only set one type of encoding, I try `-Egbk -Eutf8`, it only effect by last `-Eutf8`. 
This will still be garbled when searching many files with multi encoding. 
Can it support for set multi encoding? (ex `-E=utf-8,gbk,euc-jp`)

---

_Comment by @BurntSushi on 2018-08-16 09:37_

No.

---

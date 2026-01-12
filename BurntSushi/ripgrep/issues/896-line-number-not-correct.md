```yaml
number: 896
title: Line number not correct
type: issue
state: closed
author: kzhui125
labels:
  - question
assignees: []
created_at: 2018-04-25T06:51:15Z
updated_at: 2018-04-25T11:07:38Z
url: https://github.com/BurntSushi/ripgrep/issues/896
synced_at: 2026-01-12T16:13:22Z
```

# Line number not correct

---

_@kzhui125_

#### What version of ripgrep are you using?

https://github.com/BurntSushi/ripgrep/releases/download/0.8.1/ripgrep-0.8.1-x86_64-pc-windows-msvc.zip

ripgrep 0.8.1 (rev c8e9f25b85)
-SIMD -AVX

#### What operating system are you using ripgrep on?

Win10

#### Bug

1. download https://github.com/kzhui125/test/blob/master/clarknet_access_log_Sep4.gz and uncompress
2.  rg.exe --context 2 -E UTF8 --color never --heading --line-number -F "20:00:00" "C:\test"
3. line number not correct, should be 170824

```
170821-civileng2-kfps-dynamic-185.Stanford.EDU - - [04/Sep/1995:19:59:59 -0400] "GET /pub/job/vk/kathy.jpg HTTP/1.0" 200 7450
```

---

_Comment by @okdana on 2018-04-25 07:02_

```
170821-civileng2-kfps-dynamic-185.Stanford.EDU - - [04/Sep/1995:19:59:59 -0400] "GET /pub/job/vk/kathy.jpg HTTP/1.0" 200 7450
170822-d19-ts0.amug.org - - [04/Sep/1995:19:59:59 -0400] "GET /pub/alweiner/wavy.gif HTTP/1.0" 304 -
170823:d19-ts0.amug.org - - [04/Sep/1995:20:00:00 -0400] "GET /pub/alweiner/note.gif HTTP/1.0" 304 -
170824:dd11-047.compuserve.com - - [04/Sep/1995:20:00:00 -0400] "GET /pub/atomicbk/catalog/home.gif HTTP/1.0" 200 813
170825:d19-ts0.amug.org - - [04/Sep/1995:20:00:00 -0400] "GET /pub/alweiner/work/_0303V.gif HTTP/1.0" 200 490
170826-128.220.23.32 - - [04/Sep/1995:20:00:01 -0400] "GET /pub/atomicbk/images/nudistbk.gif HTTP/1.0" 404 207
170827-www-c4.proxy.aol.com - - [04/Sep/1995:20:00:01 -0400] "GET /pub/atomicbk/catalog/mini.gif HTTP/1.0" 304 -
```

`170821` is a context line, as indicated by the following `-`

`170823` through `170825` are match lines, as indicated by the following `:`

Seems like it's doing what it's supposed to, unless you mean that that line you pasted is literally the only output you get

---

_Comment by @kzhui125 on 2018-04-25 07:40_

@okdana  this line is 170824's line in the file, not 170821
```
civileng2-kfps-dynamic-185.Stanford.EDU - - [04/Sep/1995:19:59:59 -0400] "GET /pub/job/vk/kathy.jpg HTTP/1.0" 200 7450
```

---

_Comment by @okdana on 2018-04-25 07:53_

That's not what i see:

```
% cat -n clarknet_access_log_Sep4 | sed -n '170821,170827p'
170821  civileng2-kfps-dynamic-185.Stanford.EDU - - [04/Sep/1995:19:59:59 -0400] "GET /pub/job/vk/kathy.jpg HTTP/1.0" 200 7450
170822  d19-ts0.amug.org - - [04/Sep/1995:19:59:59 -0400] "GET /pub/alweiner/wavy.gif HTTP/1.0" 304 -
170823  d19-ts0.amug.org - - [04/Sep/1995:20:00:00 -0400] "GET /pub/alweiner/note.gif HTTP/1.0" 304 -
170824  dd11-047.compuserve.com - - [04/Sep/1995:20:00:00 -0400] "GET /pub/atomicbk/catalog/home.gif HTTP/1.0" 200 813
170825  d19-ts0.amug.org - - [04/Sep/1995:20:00:00 -0400] "GET /pub/alweiner/work/_0303V.gif HTTP/1.0" 200 490
170826  128.220.23.32 - - [04/Sep/1995:20:00:01 -0400] "GET /pub/atomicbk/images/nudistbk.gif HTTP/1.0" 404 207
170827  www-c4.proxy.aol.com - - [04/Sep/1995:20:00:01 -0400] "GET /pub/atomicbk/catalog/mini.gif HTTP/1.0" 304 -
```

It might be that whatever tool you're using to get those line numbers is confused by the fact that this file contains some carriage returns (`\r`); `rg` doesn't treat those specially

---

_Comment by @kzhui125 on 2018-04-25 08:03_

@okdana thanks! makes sense

---

_Closed by @kzhui125 on 2018-04-25 08:03_

---

_Label `question` added by @BurntSushi on 2018-04-25 11:07_

---

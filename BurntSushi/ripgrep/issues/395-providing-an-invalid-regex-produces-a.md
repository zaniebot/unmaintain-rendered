```yaml
number: 395
title: Providing an invalid regex produces a misformatted error message
type: issue
state: closed
author: bdesham
labels:
  - bug
  - icebox
assignees: []
created_at: 2017-03-07T15:38:59Z
updated_at: 2018-03-14T02:55:40Z
url: https://github.com/BurntSushi/ripgrep/issues/395
synced_at: 2026-01-12T16:13:21Z
```

# Providing an invalid regex produces a misformatted error message

---

_@bdesham_

I tried to search for the regex `\s*{`, which is invalid—the `{` needed to be backslash-escaped. The error that ripgrep produced was kind of bizarre though:

<img width="1030" alt="Screenshot of my terminal, showing the weird error message from ripgrep" src="https://cloud.githubusercontent.com/assets/354230/23663706/5f83e890-0321-11e7-9b8b-11a63b1e9a35.png">

I’m using ripgrep 0.4.0 with the built-in macOS Terminal application. The text encoding is set to UTF-8.

Here is a hex dump of ripgrep’s error message:

```
$ rg "\s*{" 2>&1 | hexdump -C
00000000  45 72 72 6f 72 20 70 61  72 73 69 6e 67 20 72 65  |Error parsing re|
00000010  67 65 78 20 6e 65 61 72  20 27 5c 73 2a 7b 27 20  |gex near '\s*{' |
00000020  61 74 20 63 68 61 72 61  63 74 65 72 20 6f 66 66  |at character off|
00000030  73 65 74 20 33 3a 20 49  6e 76 61 6c 69 64 20 61  |set 3: Invalid a|
00000040  70 70 6c 69 63 61 74 69  6f 6e 20 6f 66 20 72 65  |pplication of re|
00000050  70 65 74 69 74 69 6f 6e  20 6f 70 65 72 61 74 6f  |petition operato|
00000060  72 20 74 6f 3a 20 27 28  3f 75 3a 5b 09 2d 0d 20  |r to: '(?u:[.-. |
00000070  2d 20 c2 85 2d c2 85 c2  a0 2d c2 a0 e1 9a 80 2d  |- ..-....-.....-|
00000080  e1 9a 80 e2 80 80 2d e2  80 8a e2 80 a8 2d e2 80  |......-......-..|
00000090  a9 e2 80 af 2d e2 80 af  e2 81 9f 2d e2 81 9f e3  |....-......-....|
000000a0  80 80 2d e3 80 80 5d 29  2a 27 2e 0a              |..-...])*'..|
```

Other queries that end with the bad `{` produce fine-looking error messages:

```
$ rg "\s{"                   
Error parsing regex near '\s{' at character offset 3: Missing maximum in counted
repetition operator.
$ rg "a*{"
Error parsing regex near 'a*{' at character offset 2: Invalid application of
repetition operator to: '(?u:a)*'.
```

---

_Comment by @BurntSushi on 2017-03-07 15:46_

Oh that's just wonderful. The error message is pretty printing the AST, and since the AST isn't a faithful AST, it ends up emitting an unreadable character class. (The AST stores the expanded form of `\s` rather than `\s` itself.)

There are various things that can fix this. One is building our own error message. Another is rewriting the regex parser to itself give better error messages (which is in progress).

---

_Label `bug` added by @BurntSushi on 2017-03-07 15:46_

---

_Label `icebox` added by @BurntSushi on 2017-04-09 13:11_

---

_Closed by @BurntSushi on 2018-03-14 02:55_

---

```yaml
number: 613
title: Garbled output on some incorrect regexes
type: issue
state: closed
author: lheckemann
labels:
  - duplicate
assignees: []
created_at: 2017-09-24T11:26:49Z
updated_at: 2017-09-24T12:41:37Z
url: https://github.com/BurntSushi/ripgrep/issues/613
synced_at: 2026-01-12T16:13:22Z
```

# Garbled output on some incorrect regexes

---

_@lheckemann_

`rg '[^\t ]+\s*{$'` (bad regex: I meant `\{` instead of `{`) outputs…

```
 - or parsing regex near ']+\s*{$' at character offset 10: Invalid application of repetition operator to: '(?u:[        -
-
 -  -  -  -  -  - 　-　])*'.
```
I'm very confused!

```
$ rg '[^\t ]+\s*{$' 2>&1 | hexdump -C
00000000  45 72 72 6f 72 20 70 61  72 73 69 6e 67 20 72 65  |Error parsing re|
00000010  67 65 78 20 6e 65 61 72  20 27 5d 2b 5c 73 2a 7b  |gex near ']+\s*{|
00000020  24 27 20 61 74 20 63 68  61 72 61 63 74 65 72 20  |$' at character |
00000030  6f 66 66 73 65 74 20 31  30 3a 20 49 6e 76 61 6c  |offset 10: Inval|
00000040  69 64 20 61 70 70 6c 69  63 61 74 69 6f 6e 20 6f  |id application o|
00000050  66 20 72 65 70 65 74 69  74 69 6f 6e 20 6f 70 65  |f repetition ope|
00000060  72 61 74 6f 72 20 74 6f  3a 20 27 28 3f 75 3a 5b  |rator to: '(?u:[|
00000070  09 2d 0d 20 2d 20 c2 85  2d c2 85 c2 a0 2d c2 a0  |.-. - ..-....-..|
00000080  e1 9a 80 2d e1 9a 80 e2  80 80 2d e2 80 8a e2 80  |...-......-.....|
00000090  a8 2d e2 80 a9 e2 80 af  2d e2 80 af e2 81 9f 2d  |.-......-......-|
000000a0  e2 81 9f e3 80 80 2d e3  80 80 5d 29 2a 27 2e 0a  |......-...])*'..|
000000b0
```

this looks like the kind of thing you'd get with memory corruption, but this is Rust so I really don't know what's going on here… Can anyone enlighten me?

```
$ rg --version
ripgrep 0.6.0
-AVX -SIMD
```

---

_Comment by @BurntSushi on 2017-09-24 12:41_

> this looks like the kind of thing you'd get with memory corruption

Nah, it's just a misformatted error message. This is actually a dupe of #395. Thanks for the report though!

---

_Closed by @BurntSushi on 2017-09-24 12:41_

---

_Label `duplicate` added by @BurntSushi on 2017-09-24 12:41_

---

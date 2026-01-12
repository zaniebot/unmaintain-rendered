```yaml
number: 1215
title: search tokens are ignored, if file contains blob dumps
type: issue
state: closed
author: vbauerster
labels:
  - duplicate
assignees: []
created_at: 2019-03-09T17:53:26Z
updated_at: 2019-03-10T12:05:09Z
url: https://github.com/BurntSushi/ripgrep/issues/1215
synced_at: 2026-01-12T16:13:23Z
```

# search tokens are ignored, if file contains blob dumps

---

_@vbauerster_

#### What version of ripgrep are you using?
```
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?
homebrew

#### What operating system are you using ripgrep on?
macOS Mojave

#### Describe your question, feature request, or bug.

I noticed that `rg` keeps skipping search token, if a [file](https://github.com/vbauerster/tmp/raw/master/d.txt.gz) contains some blob dumps.
Consider following command:
```
$ cat d.txt | rg -F 'p#05'
[p#05] [03] 2019/03/09 11:13:15 total written: 0
[p#05] [03] 2019/03/09 11:13:15 (*getparty.Part)(0xc00025e0e0)({
 name: (string) (len=4) "p#05",
  prefix: (string) (len=12) "[p#05] [03] ",
   00000000  5b 70 23 30 35 5d 20 5b  30 33 5d 20 32 30 31 39  |[p#05] [03] 2019|
      msg: (string) (len=5) "p#05:",
```
There are more `p#05` entries at the end, but `rg` skipped everything after dump lines.

In contrast, `ag` handles same file as expected:
```
$ cat d.txt | ag -F 'p#05'
[p#05] [03] 2019/03/09 11:13:15 total written: 0
[p#05] [03] 2019/03/09 11:13:15 (*getparty.Part)(0xc00025e0e0)({
 name: (string) (len=4) "p#05",
  prefix: (string) (len=12) "[p#05] [03] ",
   00000000  5b 70 23 30 35 5d 20 5b  30 33 5d 20 32 30 31 39  |[p#05] [03] 2019|
      msg: (string) (len=5) "p#05:",
[p#05] [03] 2019/03/09 11:13:15 ctx timeout: 25s
[p#05] [03] 2019/03/09 11:13:15 quit: download: p#05: unexpected status: 416 Requested Range Not Satisfiable
download: p#05
```


---

_Comment by @okdana on 2019-03-09 23:05_

The file has `NUL` bytes in it that cause ripgrep to stop searching on standard input. (If you searched it by argument, as in `rg -F 'p#05' d.txt`, it wouldn't print any results at all.) You can use `-a` to tell it to search binary files.

See also #1212, #1188, #1129, #1117, ... and especially #306

---

_Label `duplicate` added by @BurntSushi on 2019-03-10 12:05_

---

_Closed by @BurntSushi on 2019-03-10 12:05_

---

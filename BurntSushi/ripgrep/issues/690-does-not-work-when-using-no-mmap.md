```yaml
number: 690
title: ^$ does not work when using --no-mmap
type: issue
state: closed
author: ericbn
labels:
  - bug
assignees: []
created_at: 2017-11-23T00:51:03Z
updated_at: 2018-08-20T11:10:21Z
url: https://github.com/BurntSushi/ripgrep/issues/690
synced_at: 2026-01-12T16:13:22Z
```

# ^$ does not work when using --no-mmap

---

_@ericbn_

I found this while running the examples described at #441 

```
$ cat scratch
a
b

c


d
$ rg 'a' scratch
1:a
$ rg 'b' scratch
2:b
$ rg 'c' scratch
4:c
$ rg 'd' scratch
7:d
$ rg --mmap '^$' scratch
3:
5:
6:
8:
$ rg --no-mmap '^$' scratch
2:
4:
6:
7:
9:
$
```

By "does not work" I mean using `--no-mmap` gives an unexpected number of results and unexpected line numbers. I get these same unexpected results by running `rg '^$' scratch`.

I'm using ripgrep 0.7.1 under macOS.

---

_Comment by @BurntSushi on 2017-11-23 01:01_

I suspect this can be rolled up into the same bug as #441.

---

_Label `bug` added by @BurntSushi on 2018-02-02 13:58_

---

_Comment by @dennis-benzinger-hybris on 2018-05-17 14:33_

It's similar to #441 but the interesting point here is that phantom empty lines are also found elsewhere in the file. For example between line 1 and 2 (then 3). Try with `-C1`.

---

_Added to milestone `libripgrep` by @BurntSushi on 2018-06-03 21:03_

---

_Comment by @BurntSushi on 2018-06-03 21:08_

I believe this bug should be fixed in libripgrep.

---

_Closed by @BurntSushi on 2018-08-20 11:10_

---

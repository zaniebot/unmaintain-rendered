```yaml
number: 1614
title: "--count with zero matches does not print \"0\""
type: issue
state: closed
author: kpp
labels:
  - duplicate
assignees: []
created_at: 2020-06-10T19:42:33Z
updated_at: 2020-06-10T22:14:07Z
url: https://github.com/BurntSushi/ripgrep/issues/1614
synced_at: 2026-01-12T16:13:23Z
```

# --count with zero matches does not print "0"

---

_@kpp_

#### What version of ripgrep are you using?

```
rg --version
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

via cargo

#### What operating system are you using ripgrep on?

Ubuntu 18.04.4 LTS

#### Describe your bug.

```
$ rg 42 /dev/null --count
$ #empty output
```

vs

```
$ grep 42 /dev/null --count
0
$
```


---

_Comment by @BurntSushi on 2020-06-10 20:01_

Duplicate of #1370. 

---

_Closed by @BurntSushi on 2020-06-10 20:01_

---

_Label `duplicate` added by @BurntSushi on 2020-06-10 20:01_

---

_Comment by @kpp on 2020-06-10 21:40_

I suggest you reopen the issue because the bug still exists since half a year the bugfix was merged.

---

_Comment by @BurntSushi on 2020-06-10 22:14_

It's not a bug. See the message of the commit that closed it: https://github.com/BurntSushi/ripgrep/commit/a070722ff27c7d57d47b1766de9d253fbfe5bef9

---

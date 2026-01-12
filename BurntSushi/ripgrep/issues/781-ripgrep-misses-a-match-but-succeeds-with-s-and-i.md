```yaml
number: 781
title: "ripgrep misses a match but succeeds with `-S` and `-i`"
type: issue
state: closed
author: OliverUv
labels:
  - bug
assignees: []
created_at: 2018-02-08T06:40:43Z
updated_at: 2018-02-08T23:26:51Z
url: https://github.com/BurntSushi/ripgrep/issues/781
synced_at: 2026-01-12T16:13:22Z
```

# ripgrep misses a match but succeeds with `-S` and `-i`

---

_@OliverUv_

#### What version of ripgrep are you using?

ripgrep 0.7.1
-AVX -SIMD

#### What operating system are you using ripgrep on?
```
Distributor ID: Ubuntu
Description:    Ubuntu 17.10
Release:        17.10
Codename:       artful
```

```
> uname -srvpoi
Linux 4.13.0-31-generic #34-Ubuntu SMP Fri Jan 19 16:34:46 UTC 2018 x86_64 x86_64 GNU/Linux
```
#### If this is a bug, what are the steps to reproduce the behavior?

Download [rgbug.tar.gz](https://github.com/BurntSushi/ripgrep/files/1705949/rgbug.tar.gz), unpack, enter the contained directory, then search for `clone_created`.

#### If this is a bug, what is the actual behavior?

https://gist.github.com/e1af638724793b341eee4f4f9e902afd

#### If this is a bug, what is the expected behavior?

Following the reproduction instructions will show a result from file `one.ts`, it should also show a result from `two.ts`. If used with the `-i` or `-S` flags, both results are shown as expected.


---

_Comment by @lilydjwg on 2018-02-08 10:10_

I get a similar issue and git bisect reports that 7dd1194a975721c49121f07644ebd0414cddbff5 is the first bad commit.

---

_Comment by @BurntSushi on 2018-02-08 12:34_

@lilydjwg Thanks for the bisect! That is indeed the cause of the issue. That commit updated the regex library, which had a new Boyer-Moore optimization. Apparently, the implementation of Boyer-Moore is buggy.

This should be fixed in the next release.

(The reason why `-i/--ignore-case` "fixes" this is because it causes a different optimization to be used instead of Boyer-Moore.)

Thanks @OliverUv for the great bug report!

---

_Label `bug` added by @BurntSushi on 2018-02-08 12:36_

---

_Closed by @BurntSushi on 2018-02-08 23:26_

---

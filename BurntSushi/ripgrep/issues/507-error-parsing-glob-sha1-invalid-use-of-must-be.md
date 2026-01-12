```yaml
number: 507
title: "error parsing glob '!**.sha1': invalid use of **; must be one path component"
type: issue
state: closed
author: Yggdroot
labels: []
assignees: []
created_at: 2017-06-07T02:54:00Z
updated_at: 2018-08-02T16:54:22Z
url: https://github.com/BurntSushi/ripgrep/issues/507
synced_at: 2026-01-12T16:13:22Z
```

# error parsing glob '!**.sha1': invalid use of **; must be one path component

---

_@Yggdroot_

There is a `.gitignore` in the source of chromium:
$ cat .gitignore
```
*/*
!**.sha1
```
If run rg as this: `rg --files`, the error `./.gitignore: line 2: error parsing glob '!**.sha1': invalid use of **; must be one path component` is printed.

---

_Comment by @BurntSushi on 2017-06-07 03:07_

What is the expected behavior? The error message seems pretty clear. In any case, this is a duplicate of #373.

---

_Comment by @Yggdroot on 2017-06-07 05:06_

Oh, sorry, did not notice it.

---

_Closed by @Yggdroot on 2017-06-07 05:06_

---

_Comment by @ChildishGiant on 2018-08-02 16:44_

![image](https://user-images.githubusercontent.com/13716824/43597746-e244b3ac-967a-11e8-8c9b-9e5f37f00ea1.png)
I'm getting this even with no uses of `**` in my gitignore. Even with no gitignore at all.

---

_Comment by @BurntSushi on 2018-08-02 16:46_

@ChildishGiant Sorry, but your comment **is not actionable**. Please open a new issue and follow the instructions listed in the issue template. Please provide a **reproducible example** and **include output using the `--debug` flag**.

---

_Comment by @ChildishGiant on 2018-08-02 16:54_

Turns out that an extension had generated some invalid files.exclude workspace settings. Sorry for the hassle. 

---

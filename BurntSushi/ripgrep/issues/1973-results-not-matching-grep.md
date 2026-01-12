```yaml
number: 1973
title: Results not matching grep
type: issue
state: closed
author: Vagelis-Prokopiou
labels:
  - duplicate
assignees: []
created_at: 2021-08-17T10:20:00Z
updated_at: 2021-08-17T11:22:30Z
url: https://github.com/BurntSushi/ripgrep/issues/1973
synced_at: 2026-01-12T16:13:24Z
```

# Results not matching grep

---

_@Vagelis-Prokopiou_

#### What version of ripgrep are you using?

ripgrep 13.0.0 (rev c9e3ffc419)

#### How did you install ripgrep?

cargo install ripgrep

#### What operating system are you using ripgrep on?

Git Bash (version 4.4.23(1)-release (x86_64-pc-msys)) in Windows 10.

#### Describe your bug.

Results not matching grep when the search pattern contains slashes.

#### What are the steps to reproduce the behavior?

rg '/form/save' yourfile

![image](https://user-images.githubusercontent.com/9408947/129708204-45047df1-8115-47ef-9251-e8e7611c1191.png)

#### What is the actual behavior?

Show the command you ran and the actual output. Include the `--debug` flag in
your invocation of ripgrep.

https://gist.github.com/Vagelis-Prokopiou/81825543448057468f2fcfed99fdb7f2

#### What is the expected behavior?

It should have found the pattern, just like grep does.
```
$ grep -r '/form/save' packages/local
packages/local/dfe-view-formcontainer/src/view/FormContainerController.js:            '/form/save',
```


---

_Comment by @BurntSushi on 2021-08-17 10:57_

The first line in the debug output gives you a hint as to what's going on:

```
DEBUG|grep_regex::literal|C:\Users\vangelisp\.cargo\registry\src\github.com-1ecc6299db9ec823\grep-regex-0.1.9\src\literal.rs:58: literal prefixes detected: Literals { lits: [Complete(C:/Users/vangelisp/Downloads/Programs/PortableGit/form/save)], limit_size: 250, limit_class: 10 }
```

That leading slash is getting replaced by something else, presumably in your shell. There is nothing ripgrep can do about this AFAIK.

In any case, this is a duplicate of #1667. And it looks like it contains a work-around. Try `rg --path-separator //`?

I don't know how grep is avoiding this problem, but it's plausible that it has been compiled in a way that prevents these issues. It's unlikely that a similar solution will work for ripgrep.

---

_Closed by @BurntSushi on 2021-08-17 10:57_

---

_Label `duplicate` added by @BurntSushi on 2021-08-17 10:57_

---

_Comment by @Vagelis-Prokopiou on 2021-08-17 11:22_

Aha.

Thanx a lot for the feedback dear Andrew, and sorry for missing the duplicate issue :-)


---

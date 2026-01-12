```yaml
number: 1197
title: "File ignored by `rg` due to `.gitignore` while `git` does not ignore it"
type: issue
state: closed
author: futpib
labels:
  - duplicate
assignees: []
created_at: 2019-02-11T10:54:04Z
updated_at: 2019-02-11T12:12:16Z
url: https://github.com/BurntSushi/ripgrep/issues/1197
synced_at: 2026-01-12T16:13:23Z
```

# File ignored by `rg` due to `.gitignore` while `git` does not ignore it

---

_@futpib_

#### What version of ripgrep are you using?

```
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

From Arch Linux repos.

#### What operating system are you using ripgrep on?

`Linux futpib-laptop 4.19.19-1-lts #1 SMP Thu Jan 31 17:56:49 CET 2019 x86_64 GNU/Linux`

#### If this is a bug, what are the steps to reproduce the behavior?

Clone this repo: https://github.com/futpib/ripgrep-issue-1197.git
Run `rg findme` there.

The repo contains a `.gitignore`:
```
_*.*
```

and a `./src/_foo/bar.js` file:
```
findme
```

#### If this is a bug, what is the actual behavior?

The file is not found (ignored with a glob from `.gitignore`).

Output of `rg --debug findme`:
https://gist.github.com/futpib/179fa5a027637652c12d4704b88c3b99#file-gistfile1-txt

#### If this is a bug, what is the expected behavior?

`rg` should have found the file, as it is not ignored by `git`. `git grep findme` finds it.


---

_Comment by @BurntSushi on 2019-02-11 12:12_

This is fixed on master, probably by #1093. 

---

_Closed by @BurntSushi on 2019-02-11 12:12_

---

_Label `duplicate` added by @BurntSushi on 2019-02-11 12:12_

---

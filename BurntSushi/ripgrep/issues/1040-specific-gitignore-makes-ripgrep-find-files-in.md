```yaml
number: 1040
title: Specific .gitignore makes ripgrep find files in .git directory
type: issue
state: closed
author: antoyo
labels:
  - bug
  - question
  - wontfix
  - gitignore
assignees: []
created_at: 2018-09-05T14:42:48Z
updated_at: 2020-05-08T11:40:45Z
url: https://github.com/BurntSushi/ripgrep/issues/1040
synced_at: 2026-01-12T16:13:22Z
```

# Specific .gitignore makes ripgrep find files in .git directory

---

_@antoyo_

#### What version of ripgrep are you using?

```
ripgrep 0.9.0
-SIMD -AVX
```

#### How did you install ripgrep?

```
pacman -S ripgrep
```

#### What operating system are you using ripgrep on?

Archlinux

```
Linux pc080 4.18.5-arch1-1-ARCH #1 SMP PREEMPT Fri Aug 24 12:48:58 UTC 2018 x86_64 GNU/Linux
```

#### Describe your question, feature request, or bug.

When I have this `.gitignore` file:
```
*
!*.*
```
the command `ripgrep --files` will show files in the `.git` directory, which does not make sense, because the `.gitignore` does not affect the files in the `.git` directory.

#### If this is a bug, what are the steps to reproduce the behavior?

1. In a git repository, create a `.gitignore` file with the above content.
2. Run `rg --files`.
3. See that `rg` shows files in the `.git` directory.

#### If this is a bug, what is the actual behavior?

`rg --files` shows the files in the `.git` directory.

#### If this is a bug, what is the expected behavior?

`rg --files` should not show the files in the `.git` directory because of a pattern of the `.gitignore` file.

---

_Comment by @BurntSushi on 2018-09-05 15:19_

This is a little tricky to fix in a consistent way. In particular, a `.git` directory is ignored because it's hidden, but the [ignore matching rules](https://docs.rs/ignore/0.4.3/ignore/struct.WalkBuilder.html#ignore-rules) permit any ignore-related file (.gitignore, .ignore, .rgignore) to override skipping hidden files when there is an explicit whitelist, which is the case here. To make this work like you expect, we'd have to special case the handling of git-specific ignore files to make it so it does not impact `.git` directories.

A work-around to this problem is to add `.git/` to a `.ignore` file alongside your `.gitignore`. The simplicity of the work-around makes it tempting to declare this as `wontfix`, but I'll leave it open as something to consider if and when the `ignore` crate gets an overhaul.

---

_Label `bug` added by @BurntSushi on 2018-09-05 15:19_

---

_Label `question` added by @BurntSushi on 2018-09-05 15:19_

---

_Label `icebox` added by @BurntSushi on 2019-01-24 16:36_

---

_Comment by @BurntSushi on 2020-05-08 11:40_

I think the right answer here is to probably just add `.git` to your ignore file. It's a corner case with a simple work around that I think is better than complicating `ignore` more than it is.

---

_Closed by @BurntSushi on 2020-05-08 11:40_

---

_Label `icebox` removed by @BurntSushi on 2020-05-08 11:40_

---

_Label `gitignore` added by @BurntSushi on 2020-05-08 11:40_

---

_Label `wontfix` added by @BurntSushi on 2020-05-08 11:40_

---

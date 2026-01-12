```yaml
number: 824
title: 0.8.0 always follows symlinks
type: issue
state: closed
author: chrmarti
labels:
  - bug
assignees: []
created_at: 2018-02-19T20:17:24Z
updated_at: 2018-02-21T10:39:29Z
url: https://github.com/BurntSushi/ripgrep/issues/824
synced_at: 2026-01-12T16:13:22Z
```

# 0.8.0 always follows symlinks

---

_@chrmarti_

#### What version of ripgrep are you using?

0.8.0

#### What operating system are you using ripgrep on?

Windows 10 64bit

#### If this is a bug, what are the steps to reproduce the behavior?

0.8.0 always follows symlinks, also without `--follow`:
- create two sibling folders `cwd` and `linktarget`
- create one file in each folder `cwd\\one.txt` and `linktarget\\two.txt`
- create a link in the `cwd` folder targeting the `linktarget` folder:
  - `mklink /d link ..\\linktarget`
- run ripgrep in `cwd`: `rg --files`

Also reproduces without `--files`.

#### If this is a bug, what is the actual behavior?

```
DEBUG/grep::search/grep\src\search.rs:195: regex ast:
Repeat {
    e: Literal {
        chars: [
            'z'
        ],
        casei: false
    },
    r: Range {
        min: 0,
        max: Some(
            0
        )
    },
    greedy: true
}
one.txt
link\two.txt
```

#### If this is a bug, what is the expected behavior?

```
one.txt
```

/cc @roblourens

---

_Closed by @BurntSushi on 2018-02-20 01:52_

---

_Comment by @BurntSushi on 2018-02-20 01:53_

@chrmarti Thanks for the great bug report! This was fallout as a result of fixing #705. This should be fixed on master.

---

_Label `bug` added by @BurntSushi on 2018-02-21 02:53_

---

_Comment by @BurntSushi on 2018-02-21 02:53_

@chrmarti @roblourens [ripgrep 0.8.1](https://github.com/BurntSushi/ripgrep/releases/tag/0.8.1) is out with this and #820 fixed.

---

_Comment by @chrmarti on 2018-02-21 10:39_

Great, thanks @BurntSushi!

---

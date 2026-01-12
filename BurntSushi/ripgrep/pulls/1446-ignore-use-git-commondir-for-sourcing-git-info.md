```yaml
number: 1446
title: "ignore: use git commondir for sourcing .git/info/exclude"
type: pull_request
state: closed
author: krobelus
labels:
  - rollup
assignees: []
base: master
head: ignore-git-info-exclude-in-linked-worktree
created_at: 2019-12-11T18:37:56Z
updated_at: 2020-02-17T22:16:37Z
url: https://github.com/BurntSushi/ripgrep/pull/1446
synced_at: 2026-01-12T18:23:13Z
```

# ignore: use git commondir for sourcing .git/info/exclude

---

_@krobelus_

Git looks for this file in GIT_COMMON_DIR, which is usually the same
as GIT_DIR (.git). However, when searching inside a linked worktree,
.git is usually a file that contains the path of the actual git dir,
which in turn contains a file "commondir" which references the directory
where info/exclude may reside, alongside other configuration shared across
all worktrees. This directory is usually the git dir of the main worktree.

Unlike git this does *not* read environment variables GIT_DIR and
GIT_COMMON_DIR, because it is not clear how to interpret them when
searching multiple repositories.

Fixes #1445

---

_Label `rollup` added by @BurntSushi on 2020-02-17 02:49_

---

_Comment by @BurntSushi on 2020-02-17 02:51_

Thanks! I'll be merging this with some minorish changes in #1486. The main changes were:

* Fix the style of the code. Namely, the lines were quite long. It's best to stick to the style of the surrounding code. I'll be switching to rustfmt soon, so this hopefully shouldn't be a problem too much longer.
* I added an integration level regression test.
* I slightly refactored the code such that it doesn't introduce an extra stat call for every directory. These can add up and wind up being surprisingly expensive. Since we are already checking for the existence of `.git` in each directory, I just reused the result of that stat call.

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---

```yaml
number: 6197
title: "build(uv-cli): Set correct gitdir and head path for worktree"
type: pull_request
state: closed
author: eth3lbert
labels: []
assignees: []
base: main
head: uv-cli-worktree
created_at: 2024-08-19T03:46:23Z
updated_at: 2024-08-29T18:23:01Z
url: https://github.com/astral-sh/uv/pull/6197
synced_at: 2026-01-10T12:53:33Z
```

# build(uv-cli): Set correct gitdir and head path for worktree

---

_Pull request opened by @eth3lbert on 2024-08-19 03:46_

## Summary

Resolves #6196 .

## Test Plan

Run `cargo check` multiple times within the worktree and inspect the outputs. There should be no `Compiling` message after the first check.

---

_Comment by @eth3lbert on 2024-08-19 04:27_

Some related info:
```
:)  tree /Users/eth/workspace/astral-sh -L 1
/Users/eth/workspace/astral-sh/
├── uv/
└── uv-dev/

(devshell) :)  git worktree list
/Users/eth/workspace/astral-sh/uv      20ef94b15 [main]
/Users/eth/workspace/astral-sh/uv-dev  3c313126b [uv-cli-worktree]
```

content of `.git` file in worktree:
```
(devshell) :)  /bin/cat /Users/eth/workspace/astral-sh/uv-dev/.git
gitdir: /Users/eth/workspace/astral-sh/uv/.git/worktrees/uv-dev
```
content of `HEAD` of worktree:
```
(devshell) :)  /bin/cat /Users/eth/workspace/astral-sh/uv/.git/worktrees/uv-dev/HEAD
ref: refs/heads/uv-cli-worktree
```

content of `commondir` of worktree:
```
(devshell) :)  /bin/cat /Users/eth/workspace/astral-sh/uv/.git/worktrees/uv-dev/commondir 
../..
```



---

_Assigned to @zanieb by @zanieb on 2024-08-19 14:19_

---

_Comment by @zanieb on 2024-08-23 23:51_

cc @charliermarsh can you actually review this since you use worktrees?

---

_Unassigned @zanieb by @zanieb on 2024-08-23 23:51_

---

_Comment by @charliermarsh on 2024-08-23 23:55_

Sure, no prob.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-23 23:55_

---

_Closed by @BurntSushi on 2024-08-29 18:11_

---

_Closed by @BurntSushi on 2024-08-29 18:11_

---

_Comment by @BurntSushi on 2024-08-29 18:13_

@eth3lbert I just merged #6825. I ended up submitting that before realizing this PR or #6196 existed. I merged that one over this one because I liked having the logic split out into a separate function. But this overall looked good too. Apologies for the duplicated effort and thank you for noticing this problem and submitting a fix. :-)

---

_Comment by @eth3lbert on 2024-08-29 18:22_

> @eth3lbert I just merged #6825. I ended up submitting that before realizing this PR or #6196 existed. I merged that one over this one because I liked having the logic split out into a separate function. But this overall looked good too. Apologies for the duplicated effort and thank you for noticing this problem and submitting a fix. :-)

All good, thanks!

---

_Branch deleted on 2024-08-29 18:23_

---

```yaml
number: 2751
title: Add option to ignore nested git repositories
type: pull_request
state: closed
author: zaneduffield
labels: []
assignees: []
base: master
head: ignoreNestedRepos
created_at: 2024-03-09T06:30:54Z
updated_at: 2025-02-01T13:51:42Z
url: https://github.com/BurntSushi/ripgrep/pull/2751
synced_at: 2026-01-12T18:23:14Z
```

# Add option to ignore nested git repositories

---

_@zaneduffield_

This implements the suggestion made in #23 to provide an option to ignore nested git repositories.

A nested git repository is identified by the presence of a .git file or directory. It's a directory in the regular case, but it's a file for git worktrees and git submodules.

This option is disabled by default.

I'm open to suggestions for better names for things. In particular I'm not sure about the name `ignore_nested_git_repo` used in `IgnoreOptions` and `IgnoreBuilder` because the concept of 'nesting' is outside of that layer; it has no knowledge of the level of a `DirEntry` and relies on the calling code to make sure that the level 0 directory is never ignored.

---

_Comment by @mvnetbiz on 2024-07-26 02:21_

I'm successfully using this in one of my repos with submodules. Thanks!

---

_Comment by @ndavd on 2024-10-06 23:32_

Any updates on this? 

---

_Comment by @zaneduffield on 2025-02-01 02:14_

It seems like there isn't much interest in getting this merged. I'm closing it for now. 
Anyone can pick this back up if they want.

---

_Closed by @zaneduffield on 2025-02-01 02:14_

---

_Comment by @ltrzesniewski on 2025-02-01 13:51_

FWIW, the maintainer has little time to review PRs, and merges them by rollups when he finds time for that. I'd let this open if you still want this to be merged.

---

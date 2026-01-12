```yaml
number: 2771
title: global ignore / ripgreprc shell parameter expansion
type: issue
state: closed
author: anuramat
labels:
  - duplicate
assignees: []
created_at: 2024-03-30T12:32:47Z
updated_at: 2024-03-30T14:05:05Z
url: https://github.com/BurntSushi/ripgrep/issues/2771
synced_at: 2026-01-12T16:13:24Z
```

# global ignore / ripgreprc shell parameter expansion

---

_@anuramat_

# Motivation:

There are certain files that I want to keep in the vcs but don't want to ever search over.

As far as I understand, my options for now are (ordered by attractiveness, descending):
- A bunch of lines of `--glob` options in `ripgreprc`, which doesn't let me e.g. reuse [fd](https://github.com/sharkdp/fd) ignore file and kinda bloats the file
- Keeping the full path of global `.ignore` file in my `ripgreprc`, which would break on a different machine
- Using `alias rg="rg --ignore-file=$XDG_CONFIG_HOME/ripgrep/ignore"`, which wouldn't properly work e.g. with [telescope.nvim](https://github.com/nvim-telescope/telescope.nvim)

# Requested feature description

**Allow shell parameter expansion in ripgreprc**

This way one could use something like `--ignore-file=$XDG_CONFIG_HOME/ignore` straight in the ripgreprc

Besides, there might be other uses for this (probably not)


---

_Comment by @BurntSushi on 2024-03-30 13:27_

Duplicate of #931.

As for your specific problem, you've missed at least a couple choices. The first being the repository-specific-but-not-tracked-in-vcs `.git/info/exclude`. ripgrep supports that. And there is also [`core.excludesfile` in `git`](https://stackoverflow.com/questions/7335420/global-git-ignore) that ripgrep supports too. That seems like it's probably what you want.

---

_Closed by @BurntSushi on 2024-03-30 13:31_

---

_Label `duplicate` added by @BurntSushi on 2024-03-30 13:31_

---

_Comment by @anuramat on 2024-03-30 14:05_

This used to work for me, but now the overlap between the git-ignored and ripgrep-ignored files isn't 100% (set difference is non-empty both ways)

Would something like `$RIPGREP_IGNORE_PATH` be unacceptable too?

---

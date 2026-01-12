```yaml
number: 713
title: .git/rr-cache
type: issue
state: closed
author: unphased
labels:
  - question
assignees: []
created_at: 2017-12-12T23:21:38Z
updated_at: 2024-11-30T22:55:14Z
url: https://github.com/BurntSushi/ripgrep/issues/713
synced_at: 2026-01-12T16:13:22Z
```

# .git/rr-cache

---

_@unphased_

I'm not sure if it is configuration error on my part, but I understand that rg (by default) filters out things in the git repository hidden folder. 

I do configure rg in my alias with `--hidden` because many of the things i search for ARE in hidden directories and files (such as `.vimrc`, `.config/`), and this for whatever reason leads to the dredging up of `.git/rr-cache/<hash>/{pre,post,this}image` files.

Since I haven't encountered other git repo stuff yet, I'm not sure if this is a bug, I'm curious about what rg *should* do about things in e.g. `.git/` in this situation where `--hidden` is specified.

---

_Comment by @BurntSushi on 2017-12-12 23:59_

Yeah if you specify `--hidden` then it's supposed to search `.git`. If you don't want ripgrep to search `.git` then put it in an `.ignore` file? e.g., `echo '.git' > .ignore`.

---

_Label `question` added by @BurntSushi on 2017-12-12 23:59_

---

_Comment by @unphased on 2017-12-13 00:24_

Yep, makes sense. I'll be doing that. 

---

_Closed by @unphased on 2017-12-13 00:24_

---

_Comment by @Wilfred on 2024-11-30 22:33_

@BurntSushi I hit this today, and I think it's a slight UI gotcha, particularly for users have rerere enabled in git.

`rg foo` doesn't search my `.github/` workflows, but `rg --hidden foo` searches both `.github/` workflows and `.git/`. The `.git/` directory is prone to unwanted matches, particularly `.git/rr-cache` because it has copies of files similar to the ones I'm searching.

It seems strange that this happens even when `--ignore-vcs` is used.

Would you be open to changing this?

---

_Comment by @BurntSushi on 2024-11-30 22:55_

No, sorry. The way to manipulate this is with ignore files.

---

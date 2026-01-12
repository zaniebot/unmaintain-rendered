```yaml
number: 2424
title: Add an option to search current git repo
type: issue
state: closed
author: nyurik
labels:
  - wontfix
assignees: []
created_at: 2023-02-21T04:49:44Z
updated_at: 2023-02-21T18:44:21Z
url: https://github.com/BurntSushi/ripgrep/issues/2424
synced_at: 2026-01-12T16:13:24Z
```

# Add an option to search current git repo

---

_@nyurik_

#### Describe your feature request

As developers, we often need to search the git repo we currently use, even if the current dir is somewhere inside that repo. Ripgrep already has a notion of the git repo (using `.gitignore` files), but it doesn't seem to have an option to search the whole git.

---

_Comment by @BurntSushi on 2023-02-21 11:56_

I'm not sure I follow. Why not `cd` into the repo root? Or just pass a path to the repo root to ripgrep? I'm not sure I see a reason to bake this into ripgrep itself.

---

_Comment by @nyurik on 2023-02-21 17:50_

@BurntSushi I am currently working with a giant monorepo (we sadly have a few of those).  Most often, I am deep within the monorepo's structure.  The `cd ../../../../..` is a massive pain to type, esp when uncertain how many of those i would need.  This is so bad that I had to create this alias:

```
alias gitroot='git rev-parse --show-toplevel'
```

which I use with fd:

```
fd 'something.*\.o$' `gitroot`
```

I think it would be useful to have a flag like `--root` to avoid requiring such workaround.

---

_Comment by @BurntSushi on 2023-02-21 18:10_

Understood. But I'm going to disagree that this belongs in ripgrep. I think your solution of `rg foo $(gitroot)` is quite fine and only barely less convenient than `--root`. You could make it even more convenient by putting it in an alias, like, `rg-root` or something.

---

_Closed by @BurntSushi on 2023-02-21 18:10_

---

_Label `wontfix` added by @BurntSushi on 2023-02-21 18:10_

---

_Comment by @nyurik on 2023-02-21 18:44_

I think it would have to be a function, like this (untested):

```bash
function rgroot(){
    rg ${@:1} $(git rev-parse --show-toplevel)
}
```

---

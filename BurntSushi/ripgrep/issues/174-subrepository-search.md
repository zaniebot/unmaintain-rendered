```yaml
number: 174
title: subrepository search
type: issue
state: closed
author: androm3da
labels: []
assignees: []
created_at: 2016-10-13T14:07:12Z
updated_at: 2016-10-13T14:24:09Z
url: https://github.com/BurntSushi/ripgrep/issues/174
synced_at: 2026-01-12T18:23:11Z
```

# subrepository search

---

_@androm3da_

AFAICT `rg` stops recursing at subrepo boundaries.  I don't prefer this behavior by default -- besides, `rg` is so fast that I can afford searching in potentially unrelated repos :)

If it makes sense as a default, it'd be nice if I could add a command line arg or `.rgconfig` option to change this behavior.


---

_Comment by @BurntSushi on 2016-10-13 14:15_

What is a "subrepo boundary"? Could you provide a small reproducible example with `mkdir` and `git init` calls?

You may want to check whether your subrepos are in your `.gitignore` file. If they are, `rg` will respect them and ignore them. If this behavior is undesirable, you can drop a `.ignore` file in the same directory and whitelist the paths you want to search, e.g., `!subrepo`.


---

_Comment by @androm3da on 2016-10-13 14:24_

tsk, sorry, indeed the project is configured with `ignore` for those subrepos.  Whoops!  Carry on.


---

_Closed by @androm3da on 2016-10-13 14:24_

---

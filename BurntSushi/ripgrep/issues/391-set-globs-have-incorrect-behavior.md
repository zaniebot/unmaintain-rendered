```yaml
number: 391
title: set globs have incorrect behavior
type: issue
state: closed
author: BurntSushi
labels:
  - bug
  - help wanted
assignees: []
created_at: 2017-03-03T13:35:54Z
updated_at: 2017-03-08T15:13:29Z
url: https://github.com/BurntSushi/ripgrep/issues/391
synced_at: 2026-01-12T16:13:21Z
```

# set globs have incorrect behavior

---

_@BurntSushi_

Repro:

```
$ git clone git://github.com/bag-man/dotfiles
$ cd dotfiles
$ rg --no-ignore --hidden --follow --files --glob '!{.git,node_modules,plugged}/**' --glob '*.{js,json,php,md,styl,scss,sass,pug,html,config,py,cpp,c,go,hs}'
```

The output includes files in the `.git` directory. Interestingly, removing some of the extensions in the second `--glob` impacts the output in unexpected ways. Changing the first glob to not use a recursive glob also seems to make it work.

If I look at the debug output, it looks like the generated regex for the second glob is:

```
(?-u)^(?:/?|.*/).*\\.hs|c|cpp|py|config|html|pug|sass|scss|styl|md|php|json|js$
```

Which is just plain wrong. The alternation should be in a group.

---

_Label `bug` added by @BurntSushi on 2017-03-03 13:35_

---

_Comment by @BurntSushi on 2017-03-03 13:39_

Yup, the bug is here: https://github.com/BurntSushi/ripgrep/blob/cf750a190fcea9cabe16cd1bef4b21f62929a370/globset/src/glob.rs#L657-L665

All it needs is a regression test and some parens pushed around the alternation.

---

_Label `help wanted` added by @BurntSushi on 2017-03-03 13:39_

---

_Closed by @BurntSushi on 2017-03-08 15:13_

---

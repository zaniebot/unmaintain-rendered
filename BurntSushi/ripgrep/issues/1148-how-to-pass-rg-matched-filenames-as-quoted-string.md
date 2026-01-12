```yaml
number: 1148
title: "How to pass rg matched filenames as \" quoted string instead of literal?"
type: issue
state: closed
author: stardiviner
labels: []
assignees: []
created_at: 2018-12-24T02:16:39Z
updated_at: 2018-12-24T04:07:21Z
url: https://github.com/BurntSushi/ripgrep/issues/1148
synced_at: 2026-01-12T16:13:23Z
```

# How to pass rg matched filenames as " quoted string instead of literal?

---

_@stardiviner_

I got problem on passing matched filenames (from option `--files-with-matches`) to command `xargs`. But when filename contains space, it report error.

Like command:
```
rg --files-with-matches --type org -e "Emacs" ~/Org | xargs rg --files-with-matches -e "Linux" | xargs rg --heading -e "Arch"
```

Here is the detailed link: https://github.com/dajva/rg.el/issues/49#issuecomment-449677077

---

_Comment by @BurntSushi on 2018-12-24 04:03_

This is your shell, not ripgrep. Either don't create file paths with whitespace in them or ask ripgrep to print file paths delimited by NUL with the `-0/--null` flag. Once you do that, you need to tell xargs to read NUL delimited file paths with the `-0` flag.

---

_Closed by @BurntSushi on 2018-12-24 04:03_

---

_Comment by @stardiviner on 2018-12-24 04:07_

Thanks. @BurntSushi 

---

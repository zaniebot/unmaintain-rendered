```yaml
number: 48
title: File name search
type: issue
state: closed
author: kaushalmodi
labels: []
assignees: []
created_at: 2016-09-24T04:12:24Z
updated_at: 2016-09-24T04:30:40Z
url: https://github.com/BurntSushi/ripgrep/issues/48
synced_at: 2026-01-12T18:23:11Z
```

# File name search

---

_@kaushalmodi_

I have noticed that the `rg` `-g` option is like `ag`'s `-G` option. But `rg` seems to be missing `ag`'s `-g` option.

I use `ag -g` heavily as `find` command as I believe it is faster than find and the `.agignore` applies there too.. so no need to provide a complex `! \( -regex -o ..\)` command to `find`.

If I want to get a list of all `*.sv` files, I would do `ag -g '\.sv'`.

Can that option please be added to `rg`?

_It would be even more awesome if the meanings of `-g` and `-G` synced up between the two._ 


---

_Comment by @BurntSushi on 2016-09-24 04:16_

Does `-g 'glob' --files` meet your criteria?


---

_Comment by @kaushalmodi on 2016-09-24 04:30_

That works. Thanks!


---

_Closed by @kaushalmodi on 2016-09-24 04:30_

---

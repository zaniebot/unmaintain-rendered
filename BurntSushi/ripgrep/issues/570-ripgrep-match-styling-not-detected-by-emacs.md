```yaml
number: 570
title: ripgrep match styling not detected by emacs
type: issue
state: closed
author: tspiteri
labels: []
assignees: []
created_at: 2017-07-29T22:50:40Z
updated_at: 2017-07-30T21:46:17Z
url: https://github.com/BurntSushi/ripgrep/issues/570
synced_at: 2026-01-12T16:13:22Z
```

# ripgrep match styling not detected by emacs

---

_@tspiteri_

Emacs seems to be matching a particular ANSI pattern in `grep.el` to be able to jump directly to the match in a line, using the pattern `"\033\\[0?1;31m\\(.*?\\)\033\\[[0-9]*m"`. This works for GNU grep which styles the match using `"\x1b[01;31m"`, but not for rg which styles the match using `"\x1b[31m\x1b[1m"`.

Note: As a workaround, I wrapped rg with a script:
```sh
#!/bin/sh
rg --color ansi -nH --no-heading "$@" | sed 's/\o033\[31m\o033\[1m/\o033[1;31m/g'
```

---

_Comment by @okdana on 2017-07-30 00:59_

Related? https://github.com/BurntSushi/ripgrep/issues/244#issuecomment-262168037

---

_Comment by @BurntSushi on 2017-07-30 21:46_

I don't think it's reasonable to expect that ripgrep's ANSI styling matches grep's ANSI styling byte-for-byte. Your work-around seems fine to me?

---

_Closed by @BurntSushi on 2017-07-30 21:46_

---

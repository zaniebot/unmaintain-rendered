```yaml
number: 1195
title: rg.exe still shows v0.10
type: issue
state: closed
author: leeoniya
labels:
  - bug
assignees: []
created_at: 2019-02-10T17:48:33Z
updated_at: 2019-02-10T18:14:37Z
url: https://github.com/BurntSushi/ripgrep/issues/1195
synced_at: 2026-01-12T16:13:23Z
```

# rg.exe still shows v0.10

---

_@leeoniya_

`rg.exe` in the grep-searcher-0.1.3 release shows correct commit but incorrect version?

`ripgrep 0.10.0 (rev d6feeb7ff2)`

could be useful to add the build date in there as well to quickly grok how old the build is without git/github spelunking.

cheers!

---

_Closed by @BurntSushi on 2019-02-10 17:52_

---

_Comment by @BurntSushi on 2019-02-10 17:53_

Those aren't releases. Windows CI is just over-eager. I've pushed what I hope is a fix, but I'm not sure.

I think a revision number is sufficient for now.

---

_Label `bug` added by @BurntSushi on 2019-02-10 17:53_

---

_Comment by @leeoniya on 2019-02-10 18:14_

ok, thanks!

---

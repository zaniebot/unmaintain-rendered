```yaml
number: 1047
title: "Feature Request: Adding a Type for *.impex files"
type: issue
state: closed
author: SpikePy
labels:
  - question
assignees: []
created_at: 2018-09-11T08:27:55Z
updated_at: 2018-09-11T11:34:33Z
url: https://github.com/BurntSushi/ripgrep/issues/1047
synced_at: 2026-01-12T16:13:22Z
```

# Feature Request: Adding a Type for *.impex files

---

_@SpikePy_

Could you implement a type for *.impex files? They are used in hybris and it would be nice if I could just type `rg "pattern" -timpex` instead of `rg "pattern" -g "*.impex"`.

---

_Comment by @BurntSushi on 2018-09-11 11:34_

I'm not sure ripgrep should be in the business of adding every file type extension used by every type of proprietary software. I could reverse course on that if there's a strong enough motivation, but you don't need the type to be in ripgrep core to make easy use of it. Have you  read the [guide's section on file types](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#manual-filtering-file-types)? The section on [configuration files](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#configuration-file) is also probably useful.

---

_Closed by @BurntSushi on 2018-09-11 11:34_

---

_Label `question` added by @BurntSushi on 2018-09-11 11:34_

---

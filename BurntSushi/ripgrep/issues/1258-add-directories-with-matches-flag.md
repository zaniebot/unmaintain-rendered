```yaml
number: 1258
title: "Add `--directories-with-matches` flag"
type: issue
state: closed
author: ad-si
labels:
  - question
assignees: []
created_at: 2019-04-18T15:44:42Z
updated_at: 2019-04-18T16:00:52Z
url: https://github.com/BurntSushi/ripgrep/issues/1258
synced_at: 2026-01-12T16:13:23Z
```

# Add `--directories-with-matches` flag

---

_@ad-si_

#### Describe your feature request

`rg --files-with-matches` shows all files containing a match. E.g.:

```sh
$ rg --files-with-matches test
projectA/readme.md
projectA/main.hs
projectB/readme.md
```

It would be cool if there was a flag to show all directories which contain files with matches:

```sh
$ rg --directories-with-matches test
projectA
projectB
```

This could be  for example used to check which of the projects has a certain dependency:
```sh
rg --directories-with-matches --max-depth 1 jquery-v3.4.0
```

---

_Comment by @BurntSushi on 2019-04-18 15:53_

I'm going to have to decline. Sorry. I suggest using a shell pipeline for this. For example:

```
$ rg foo -l -0 | xargs -0 dirname | sort -u
```

---

_Closed by @BurntSushi on 2019-04-18 15:53_

---

_Label `question` added by @BurntSushi on 2019-04-18 15:53_

---

_Comment by @ad-si on 2019-04-18 16:00_

😭That's much longer and definitely not user friendly

---
